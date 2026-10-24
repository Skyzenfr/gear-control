# Logitech G FITS : accès direct USB

Implémentation d'interopérabilité du 11 octobre 2026, indépendante de G HUB. Elle utilise la pile HID Windows et `node-hid`. Aucune bibliothèque, configuration ou ressource Logitech n'est chargée par le serveur.

## Sources et observations

- Transport Centurion : [Solaar base.py](https://github.com/pwr-Solaar/Solaar/blob/master/lib/logitech_receiver/base.py) et [device.py](https://github.com/pwr-Solaar/Solaar/blob/master/lib/logitech_receiver/device.py), sources primaires de l'implémentation de ce protocole. Les fichiers consultés sont conservés dans `gfits-direct-solaar-*.py` à titre de recherche, jamais importés par l'application. Le nouvel adaptateur est une implémentation originale en JavaScript des échanges observés.
- Interface observée par `node-hid` et par `HidP_GetCaps` Windows : VID `046D`, PID `0A7B`, collection vendor `FFA0:0001`, report d'entrée/sortie `50`, longueur 64 octets. La collection consumer et la collection HID++ `FF13` ne sont jamais ouvertes par l'adaptateur.
- REA a déjà analysé le bundle Electron G HUB dans la recherche précédente. `rea inspect-binary-layout` a été tenté sur `lghub_agent.exe` pour ce travail ; son fournisseur natif refuse ce PE Windows (`unsupported_provider`, fournisseur Linux x64 uniquement). Aucun succès natif REA n'est revendiqué.
- Les noms et signatures des fonctions de G HUB ont été lus dans les chaînes de son exécutable installé. Un désassembleur local MIT, `iced-x86`, a permis de vérifier les identifiants de fonction et les paramètres. Scripts et sorties sous `gfits-direct-*.cjs` / `*-asm.txt`. Ces outils ne font pas partie du serveur.
- Les correspondances de gestes ont ensuite été mesurées en changeant temporairement un geste via l'API locale G HUB, en lisant la trame USB puis en restaurant le geste original. `gfits-direct-ghub-crosscheck.json` conserve les résultats. G HUB a ensuite été fermé ; les tests finaux utilisent uniquement HID.

## Trames et ponts

La trame émise est `[50, adresse, longueur, flags, featureIndex, fonction<<4|softwareId, paramètres..., padding]`. L'adresse mesurée des deux récepteurs est `23`. `longueur` inclut `flags`. Les logiciels identifient une réponse avec son adresse, son index de fonction, la fonction et le softwareId. Les ACK ne prouvent pas l'application d'un réglage ; l'adaptateur relit jusqu'à confirmation, au maximum quatre fois en 475 ms hors temps USB.

Les indices de fonction sont découverts avec ROOT.GetFeature ; ils ne sont pas supposés identiques entre firmwares. Le récepteur annonce le pont `0003`. Sa fonction 1 transporte `[remoteId<<4|lenHi, lenLo, subCpl=0, subFeatureIndex, subFunction|softwareId, paramètres]`. La réponse est un MessageEvent du pont avec softwareId 0 et le message interne correspondant.

Pour les appareils présents : remote 0 est l'écouteur droit/principal, remote 1 le gauche. Les numéros de série et mesures de la paire noire correspondent aux composants publiés par G HUB. L'écouteur principal annonce à son tour le pont `0003` ; son remote 0 est le boîtier. La lecture du boîtier nécessite donc deux enveloppes de pont. Un boîtier absent donne `null`, jamais 0 %.

## Fonctions exploitées

| Fonction | Lecture | Écriture validée |
| --- | --- | --- |
| `0100` DeviceInfo | fonction 2 : numéro de série du récepteur | aucune |
| `0104` BatterySOC | fonction 0 : niveau exact, seuil suivant, état de charge | aucune |
| `0604` Sidetone v1 | fonction 0 : `[nombreMicros=1, micId=0, niveau=0 ou 100]` | fonction 1 : `[micId=0, activation=0 ou 1]` |
| `0701` TapControl v2 | fonction 3 : `[bud=0 gauche/1 droite, tap=0 simple/1 double]` | fonction 2 : `[bud,tap,action]` |

Action des gestes, table mesurée : OFF=0, VOICE_ASSISTANT=1, PLAY_PAUSE=2, VOLUME_UP=3, VOLUME_DOWN=4, SKIP_TRACK=5, PREVIOUS_TRACK=6, TOGGLE_BLUETOOTH_GAMING_MODE=10. Les gestes simples sont limités aux options annoncées par G HUB : skip, précédent, volume haut/bas et off.

État de charge BatterySOC : 0 décharge, 1/2 recharge, 3 charge complète. L'interprétation suit Solaar ; les six composants observés lors de cette session étaient en décharge. Les transitions de charge n'ont pas été exercées.

## Validation indépendante

`gfits-direct-validation.json` : lecture des deux paires, puis activation/désactivation du retour vocal et modification/restauration d'un geste simple de chaque paire. Huit opérations de réglage sont confirmées par lecture directe. Les paramètres finaux correspondent aux paramètres initiaux. L'application `lghub.exe` et `lghub_agent.exe` sont arrêtées pendant cette validation. Le service de mise à jour `lghub_updater.exe`, protégé par Windows SYSTEM, reste présent ; il n'écoute pas l'API utilisée précédemment et n'est pas appelé par le nouvel adaptateur.

Mesure finale : blanc gauche 100 %, droit 96 %, boîtier 56 % ; noir gauche 92 %, droit 93 %, boîtier 62 %. Ces niveaux évoluent avec l'usage. L'appairage, le changement de connexion et le cycle physique extinction/allumage n'ont pas été exercés ; les timeouts et absences de réponse rendent les mesures inconnues.

Tests de protocole : `node --test app/server/gfits.test.mjs`. Les fixtures proviennent des vraies réponses USB ; les tests vérifient le décodage des deux ponts, le rejet des réponses d'un autre appareil ou d'une autre requête et les mesures inconnues.

## Limites concrètes

- G FITS annonce également `0636` OnboardEQ. ROOT.GetFeature et GetEQInfos répondent : `80 18 F4 0C 05`. Les paramètres d'égalisation, coefficients et sauvegarde n'ont pas été suffisamment décodés ni validés. L'égaliseur reste désactivé ; aucune commande d'écriture EQ n'a été envoyée.
- Le volume et le niveau du microphone exposés par G HUB sont des contrôles de l'interface audio Windows, pas la fonction `0200` (absente des écouteurs mesurés). Le wrapper gfits-complete.mjs les intègre désormais via Core Audio, avec association physique et préservation du mute. Voir WINDOWS-AUDIO.md.
- Le boîtier relaie sa dernière mesure via l'écouteur principal. La date affichée est celle de la lecture USB ; ce n'est pas une preuve d'une nouvelle mesure interne du boîtier.
- Les commandes de firmware, reset, appairage et moulage `0700` sont exclues. Aucune installation ni aucun démarrage automatique de Logitech n'a été modifié.
