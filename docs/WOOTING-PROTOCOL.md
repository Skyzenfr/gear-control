# Contrôle RGB Wooting

L’adaptateur `app/server/adapters/wooting-rgb.mjs` envoie réellement une couleur statique et permet de rendre l’éclairage au profil Wootility. Il utilise `node-hid`, déjà présent dans Gear Control, sans DLL ni dépendance supplémentaire. Il ne règle pas le point d’activation, Rapid Trigger, les remappages ou les profils analogiques.

## Références primaires

Protocole étudié le 16 octobre 2026 sur le dépôt officiel Wooting, révision `451f40dc056e0edefccdaba7807a72b694520e92` du 28 septembre 2026 :

- [Transport USB et identification des modèles](https://github.com/WootingKb/wooting-rgb-sdk/blob/451f40dc056e0edefccdaba7807a72b694520e92/src/wooting-usb.c).
- [Commandes, dimensions et métadonnées du transport](https://github.com/WootingKb/wooting-rgb-sdk/blob/451f40dc056e0edefccdaba7807a72b694520e92/src/wooting-usb.h).
- [Encodage des couleurs et restitution de l’éclairage](https://github.com/WootingKb/wooting-rgb-sdk/blob/451f40dc056e0edefccdaba7807a72b694520e92/src/wooting-rgb-sdk.c).
- [API publique et contrat de fermeture](https://github.com/WootingKb/wooting-rgb-sdk/blob/451f40dc056e0edefccdaba7807a72b694520e92/src/wooting-rgb-sdk.h).

L’implémentation JavaScript a été écrite séparément à partir des formats et identifiants publics. Aucun fichier source du SDK, distribué sous MPL-2.0, n’est incorporé au logiciel.

## Périmètre matériel

Le VID est `31E3`. Chaque base ci-dessous accepte les variantes officielles `base`, `base | 1` et `base | 2` liées à la compatibilité du pilote de manette.

| Base PID | Modèle | Paquets par défaut hors interface V3 |
| --- | --- | --- |
| `1100` | Wooting One, révision VID moderne | 257 octets |
| `1200` | Wooting Two, révision VID moderne | 257 octets |
| `1210` | Wooting Two Lekker Edition | 257 octets |
| `1220` | Wooting Two HE | 257 octets |
| `1230` | Wooting Two HE ARM | 4 × 65 octets |
| `1300` | Wooting 60HE | 257 octets |
| `1310` | Wooting 60HE ARM | 4 × 65 octets |
| `1320` | Wooting 60HE+ | 4 × 65 octets |
| `1340` | Wooting 60HE V2 | 257 octets |
| `1400` | Wooting 80HE | 257 octets |
| `1510` | Wooting UwU RGB | 4 × 65 octets |

Les anciens appareils `03EB:FF01/FF02` utilisent un adressage LED différent et ne reçoivent aucune commande de cet adaptateur. Le UwU sans RGB, base `1500`, est également exclu.

Seules les collections de configuration `usagePage=1337` ou `FF55` sont acceptées. Le SDK ne fixe pas de numéro d’interface ou d’usage unique pour tous les modèles : Gear Control exige des valeurs valides et leur correspondance exacte entre l’énumération initiale, l’énumération actuelle et `getDeviceInfo()` du handle ouvert. Le chemin HID, le VID/PID, le numéro de série, l’interface, la page d’usage et l’usage doivent tous correspondre. Sans numéro de série exploitable, les commandes sont désactivées.

## Commandes et confirmation

Une commande Feature fait 8 octets : identifiant de rapport, octets de protocole, commande puis quatre paramètres nuls. La commande `33` initialise le contrôle RGB ; `32` restitue l’éclairage au profil d’origine. Le transport classique utilise l’identifiant `0` et les octets `D0 DA`. La collection V3 utilise l’identifiant `1` et `D1 DA`.

L’initialisation et la restitution attendent une réponse Input comme le SDK : 256 octets cumulés pour l’interface classique, ou un rapport numéroté non vide pour V3. Une réponse absente, excessive ou incomplète échoue. Ces réponses ne contiennent pas la couleur actuelle.

La couleur statique remplit la matrice de 6 × 21 cellules en RGB565, stocké en petit-boutiste. Le rapport de sortie commence par l’identifiant, `D0 DA` ou `D1 DA`, puis la commande de matrice `11`. Le transport classique transmet 257 octets ou quatre fragments de 65 octets selon les valeurs par défaut du modèle officiel. V3 utilise le rapport numéro `5`, de 2047 octets, conformément à la constante du SDK cité. Il n’existe pas de mécanisme de sélection automatique d’un autre format après un échec.

La sélection des tailles classiques repose sur les valeurs par défaut du SDK pour le modèle exact. Le SDK C peut aussi consulter le descripteur HID ; `node-hid` n’expose pas cette fonction. Une révision qui changerait cette taille peut donc nécessiter une adaptation. Aucun modèle ou firmware n’est déclaré testé sur du matériel réel dans cette livraison.

Le protocole public ne permet pas de relire les pixels. Gear Control publie donc `confirmation=transport-acknowledgement`, `state=last-transmitted` et la date du dernier envoi. L’interface précise cette limite. `rgbColor` utilise la source `wooting-rgb-last-transmitted` ; `rgbOverride` représente la prise en charge par la session locale, avec la source `gear-control-session`. Ces champs sont marqués `writeOnly` dans les descriptions de capacités afin de ne jamais prétendre à une relecture matérielle.

Avant le premier envoi, aucune couleur matérielle n’est inventée. Le sélecteur affiche une proposition `#c4a3ff`, exposée comme `controlDefaults`, qui est envoyée seulement si l’utilisateur l’applique. Activer explicitement `rgbOverride` sans fournir de couleur utilise cette proposition ; changer seulement la couleur active aussi le contrôle RGB.

## Cycle de vie et erreurs

- L’énumération, la découverte et l’actualisation n’ouvrent aucun clavier et n’écrivent aucun rapport.
- Toutes les valeurs du lot sont validées avant l’initialisation. Les commandes d’activation, de mémoire, de firmware et les paquets arbitraires sont rejetés.
- Le périphérique doit être ajouté à l’inventaire. L’identité actuelle et l’appartenance sont revérifiées avant chaque commande et chaque fragment de matrice.
- Les opérations natives utilisent l’API asynchrone et un délai. Si une opération non annulable expire, une nouvelle commande sur cette identité reste bloquée tant que la promesse native n’est pas terminée. Une actualisation ne supprime pas ce blocage.
- Une erreur retire la couleur présentée comme transmise ; aucun nouvel essai automatique n’est effectué.
- Désactiver explicitement le contrôle RGB envoie `32`, puis ferme le handle. Ce lot ne doit pas contenir simultanément une nouvelle couleur.
- Le hook `releaseDevice(identity)` permet au retrait explicite de l’inventaire de restituer une session précédemment prise en charge. Seule cette restitution d’une session active peut ignorer l’appartenance qui vient d’être retirée ; l’identité USB exacte reste obligatoire. Le hook ne touche pas un autre clavier ni une session non active.
- À l’arrêt du service, un clavier encore accessible, ajouté, et effectivement pris en charge par cette session reçoit `32`, comme le contrat de fermeture du SDK. Aucun autre périphérique n’est initialisé ou réinitialisé.
- Rien n’est enregistré dans un fichier propre à l’adaptateur et aucune valeur mise en cache par le hub n’est réappliquée au démarrage. Les profils Wootility restent indépendants. Un autre programme peut modifier l’éclairage entre deux commandes ; l’historique du dernier envoi ne constitue pas une mesure permanente du RGB.

## Vérification

`node --test server/adapters/wooting-rgb.test.mjs` valide les identifiants exacts, les formats RGB565 classiques et V3, les fragments, la passivité de la découverte, les identités discordantes, l’appartenance à l’inventaire, les erreurs d’acquittement, la restitution, l’absence de réapplication et le blocage après expiration. Tous les transports sont simulés ; ces tests n’ouvrent aucun périphérique réel.

## Intégration

Instancier `WootingRgbAdapter({ isAdded })` avec la fonction d’appartenance du hub et l’ajouter à la liste des adaptateurs. Les exports `wootingRgbModels` décrivent les PID et les modèles pour le catalogue. Le constructeur accepte les options communes supplémentaires sans les utiliser. Ajouter le fichier `.mjs` à la liste des modules serveur empaquetés ; aucun nouveau composant natif n’est requis.

Sur un retrait explicite de l’inventaire, appeler `releaseDevice(identity)` avant l’actualisation qui ferme les handles retirés. Si le clavier est devenu inaccessible, le retrait peut être poursuivi et Wootility peut reprendre l’éclairage ensuite. Une actualisation périodique n’appelle jamais ce hook.

L’UI générique doit accepter les contrôles `rgbOverride` de type `toggle` et `rgbColor` de type `color`, y compris lorsque la couleur n’est pas dans `settings`, grâce à `settingsMetadata.writeOnly` et `controlDefaults`. Le texte `settingsMetadata.rgbControl.help` doit être visible avec les réglages pour expliquer l’absence de relecture et la portée de la session.
