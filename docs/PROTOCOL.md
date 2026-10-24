# Ancienne version : protocole local G HUB

Ce document conserve l’analyse de la première version, qui communiquait avec l’agent G HUB. **Gear Control 2 utilise désormais USB HID directement ; l’ancien pont et son interface ont été retirés des sources de l’application.** Les protocoles directs sont décrits dans les autres documents de ce dossier. Les limitations ci-dessous concernent uniquement l’ancien pont.

L’installation analysée le 11 octobre 2026 utilise l’agent **2026.6.981214** et **Electron 39.1.2**. Le fichier d’installation `LGHUB/version` contient la version Electron ; ce nombre ne désigne pas le build de l’agent.

## Sources et méthode

[morluto/rea](https://github.com/morluto/rea) a réellement été utilisé avec son CLI, conformément à son [guide d’analyse JavaScript/Electron](https://github.com/morluto/rea/blob/main/docs/javascript-artifact-reconstruction.md) :

```powershell
npx -y rea-agents@latest analyze-javascript-application "C:/Program Files/LGHUB/resources/app.asar" --json
```

Les résultats ont été complétés par la lecture ciblée des fichiers installés `main.js`, `app.min.js` et `preload.bundle.js`, extraits dans le répertoire de recherche, puis par des lectures locales et des modifications réversibles contrôlées sur le matériel. La configuration produit G FITS `default_configurations.json` a également été consultée. Aucun fichier installé de G HUB n’a été modifié et aucun binaire ou asset Logitech n’est nécessaire au livrable.

Les preuves locales sont conservées dans `research/rea-ghub-analysis.json`, `research/rea-ghub-focused-analysis.json` et `research/ghub-protocol-notes.md`. REA a analysé 10 fichiers et relevé 553 résultats, sans erreur de parsing. Son analyse sémantique reste partielle à cause de limites mémoire sur les gros bundles ; les formats ci-dessous proviennent aussi de lectures ciblées et des observations locales, pas d’une reconstruction complète de G HUB.

## Transport

Le pont Node local ouvre `ws://127.0.0.1:9010` avec le sous-protocole WebSocket `json`. Une requête contient un identifiant de corrélation, un verbe, un chemin et éventuellement un objet `payload` :

```json
{"msgId":"requete-1","verb":"GET","path":"/devices/list"}
```

La réponse reprend `msgId` et contient notamment `result: {code, what}` et `payload`. Une modification effective doit retourner `result.code: "SUCCESS"`, puis être confirmée par relecture. Une écriture identique à la valeur courante peut retourner `INVALID_ARG` avec `what: "No change"` : ce cas ne démontre pas une incompatibilité du chemin. Les types protobuf peuvent apparaître sous `payload["@type"]` ; ce transport utilise des objets JSON, sans encodage protobuf côté webapp.

G HUB expose également `SUBSCRIBE` et `UNSUBSCRIBE`. Les notifications sont corrélées par leur chemin. Le pont complète les notifications de présence et de batterie par une actualisation périodique.

## Inventaire et batteries

| Fonction | Requête | Champs utilisés |
|---|---|---|
| Appareils présents | GET `/devices/list` | `deviceInfos[]`, `id`, `state`, `deviceModel`, `deviceUnitId`, `capabilities` |
| Changement de présence | SUBSCRIBE `/devices/state/changed` | Notification déclenchant une relecture |
| Motif de blocage | GET `/devices/{id}/blockers` | `deviceBlockers[].blocker.type` |
| Sous-appareils, pour analyse | GET `/devices/subdevices/list`, payload `{id}` | `subdevicesInfo` |
| Batterie d’un composant | GET `/multibattery/{id}/{type}/state` | `componentType`, `percentage`, `charging`, `fullyCharged` |

Un G FITS est identifié par `deviceModel: "g_fits"` et `deviceType: "EARBUDS"`. Chaque récepteur conserve son ID : deux appareils du même modèle restent distincts.

Les composants sont annoncés par `capabilities.multiBatterySupport.components[].type`, avec les valeurs `LEFT_EARBUD`, `RIGHT_EARBUD` et `CASE`. `percentage` est déjà une valeur sur 100. Une lecture absente, une batterie non prise en charge ou un appareil non actif produit une valeur inconnue, jamais un niveau inventé.

## Réglages interopérables

Les contrôles sont activés seulement après une lecture valide sur un appareil `ACTIVE`. Chaque écriture est suivie d’une relecture pour confirmer la valeur.

| Contrôle | GET | SET : objet `payload` |
|---|---|---|
| Volume sortie | `/audio/{id}/volume`, `{channel:"OUTPUT"}` | Même chemin, `{channel:"OUTPUT",gain:0..1,muted:étatCourant}` |
| Volume micro, si disponible | `/audio/{id}/volume`, `{channel:"INPUT"}` | Même chemin, `{channel:"INPUT",gain:0..1,muted:étatCourant}` |
| Retour micro | `/audio/{id}/sidetone`, `{channel:"SIDETONE"}` | Même chemin, `{channel:"SIDETONE",gain:0 ou 1,muted:étatCourant}` |
| Actions tactiles proposées | `/audio/{id}/tap_action_list` | Lecture seule : `singleTapActionNameList`, `doubleTapActionNameList` |
| Geste tactile | `/audio/{id}/tap_user_action` | Même chemin, `{budSide:"LEFT" ou "RIGHT",tapType:"SINGLE" ou "DOUBLE",tapAction:actionProposée}` |

Les lectures audio retournent notamment `{channel,gain,muted}`. Les gestes assignés sont dans `actionsMap`, avec `budsideTaptypePair` tel que `LEFT_DOUBLE`. Une action doit appartenir à la liste proposée par l’appareil.

Les écritures de volume et de retour micro validées ont conservé le type JSON observé à la lecture : `"@type": "type.googleapis.com/logi.protocol.audio.Volume"`. La réponse à une écriture tactile annonce `type.googleapis.com/logi.protocol.audio.TapControl.BudActionMap`. Son payload de commande utilise les trois champs `budSide`, `tapType` et `tapAction` ; l’acceptation du seul champ `@type` ne garantit pas la validité de la commande. Une lecture valide ne suffit pas à prouver qu’une écriture a fonctionné ; la webapp relit et compare la valeur demandée.

Ces commandes appliquent les réglages à la session courante. La webapp ne modifie pas les profils G HUB et ne garantit pas la conservation des réglages après un changement de profil, une reconnexion ou un redémarrage. Les noms personnalisés de la webapp sont, eux, conservés localement dans `app/.local/names.json`.

## Égaliseur et validation matérielle

Le build G HUB analysé n’expose pas d’EQ pour les G FITS : sa logique d’égaliseur exclut les earbuds, les appareils observés annoncent `hasEq:false`, `hasProEq:false` et `hasOnboardEq:false`, et leur configuration par défaut ne contient que `SidetoneSettings`. Les routes EQ génériques d’autres produits ne suffisent pas à établir un support G FITS. La webapp refuse donc les écritures EQ.

Un EQ Web Audio ne traiterait que le son passant par la page ; il ne modifierait pas le son de toutes les applications Windows.

La validation locale a détecté deux récepteurs distincts. La paire noire est ensuite devenue `ACTIVE` en LIGHTSPEED ; ses lectures et ses écritures ont été contrôlées sur le matériel : volume sortie `1 → 0,99 → 1`, volume micro `0,500709534 → 0,490709543 → 0,500709534`, retour micro `0 → 1 → 0` et geste `LEFT_SINGLE: OFF → SKIP_TRACK → OFF`. Chaque changement a été relu et les réglages initiaux ont été rétablis.

La paire blanche reste `BLOCKED` avec le motif `MISSING_INTERFACE`. Ses valeurs indisponibles et ses commandes restent désactivées jusqu’à une connexion active ; le branchement d’un récepteur seul ne suffit pas à établir la présence des écouteurs.
