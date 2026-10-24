# Sony INZONE Buds — accès indépendant

Adaptateur : `app/server/adapters/sony.mjs`. Vérifié sur le récepteur USB présent sur ce PC le 11 octobre 2026, après arrêt d’INZONE Hub. Batteries, ANC, commandes et profils tactiles utilisent directement le protocole HID. L’égaliseur PC utilise séparément **l’APO Sony déjà installé dans Windows**, sans lancer INZONE Hub. Cette dernière fonction dépend donc encore du pilote audio Sony, pas de son application. Aucun pilote n’a été installé ou remplacé, et le service audio Windows n’a pas été redémarré.

## Résultats sur le matériel

Lecture indépendante : écouteur gauche **100 %**, droit **100 %**, boîtier **37 %**. Le niveau du boîtier est une mesure relayée par un écouteur, et peut rester inchangé pendant que le boîtier charge. Dans cette capture, le statut du boîtier est `FF` : sa charge reste donc inconnue.

L’API expose désormais `battery.components.left/right/case={percentage,charging}`. Le statut provient exclusivement de l’octet propre à chaque composant dans `0x04`, également inclus dans la lecture groupée `0x06`. L’assembly Sony installé définit `BATTERY_STATUS` : **0 = décharge, 1 = charge, 2 = erreur** (`research/sony/sony-all-decompiled.cs:53535`). Son affichage teste explicitement `batteryStatusCase == 1` pour `IsCaseCharging` (`research/sony/sony-headsettab-decompiled.cs:298`). L’adaptateur utilise ces constantes d’interopérabilité ; il n’incorpore aucun code Sony.

Un statut **1** déclenche l’indicateur du composant concerné, même si son pourcentage est inconnu. Le statut **0** donne `charging=false` seulement si le pourcentage est valide : les captures documentent un écouteur docké avec statut 0 et pourcentage `FF`, qui ne permet pas de conclure qu’il ne charge pas. Erreur 2, sentinelle `FF`, autres statuts et réponses invalides donnent `charging=null`. Le statut global `battery.charging` concerne les écouteurs ; l’interface peut afficher séparément **Boîtier en charge** à partir de `components.case.charging`.

**Limite de fraîcheur Sony :** `caseSnapshot=true` s’applique au niveau et au statut du boîtier. Les captures locales ont toujours observé `[00, percentL, 00, percentR, FF, 37]` ; aucun statut de charge 1 du boîtier n’a été observé sur ce PC. [Les essais matériels documentés par OpenInzone](https://github.com/penguinwokrs/openinzone/blob/main/docs/PROTOCOL.md#battery-0x04) ont aussi obtenu une réponse identique avant et après branchement du chargeur, avec `FF` pour le statut du boîtier. La prise en charge du statut explicite Sony est testée par fixtures, mais ne garantit pas que le firmware des Buds le relaie. Aucune charge n’est déduite du câble USB, d’une batterie qui augmente ou de la présence des écouteurs. Le libellé d’aide précise que la dernière mesure relayée peut être inconnue ou ancienne.

Les changements ci-dessous ont été envoyés directement par HID, relus depuis les écouteurs, puis restaurés à leurs valeurs initiales :

| Réglage | Test réalisé | État final |
|---|---|---|
| Volume casque | 22/30 → 21/30 → 22/30 | 22/30 |
| Balance jeu/chat | 50 → 60 → 50 | 50 |
| Retour de voix | 0/10 → 1/10 → 0/10 | 0/10 |
| Microphone muet | désactivé → activé → désactivé | désactivé |
| Niveau ambiant | 20 → 19 → 20 | 20 |
| Priorité aux voix | désactivée → activée → désactivée | désactivée |
| Extinction automatique | activée → désactivée → activée | activée |
| Guidage vocal | désactivé → activé → désactivé | désactivé |
| Langue du guidage | anglais → japonais → anglais | anglais |
| Bascule Bluetooth lors d’un appel | désactivée → activée → désactivée | désactivée |
| Profil tactile gauche | ANC/ambiant → volume → ANC/ambiant | ANC/ambiant |
| Profil tactile droit | volume → lecture multimédia → volume | volume |

Une seconde vérification, après que l’utilisateur a confirmé porter les écouteurs, a réussi : **ANC → ambiant → désactivé → ANC**, avec une lecture indépendante après chaque changement. Le mode initial ANC a été restauré. Le contrôle ANC / ambiant est donc exposé comme modifiable, avec un message invitant à porter les écouteurs si le matériel refuse le changement. La première tentative, avant cette confirmation, avait été ignorée par le récepteur ; le lien avec le port des écouteurs est cohérent avec cette observation mais la condition interne exacte n’est pas prouvée. Preuve supplémentaire : `research/sony/anc-worn-verified.json`.

La commande de coupure de la sortie audio est toujours **non exposée**, car elle n’a pas été confirmée. Aucune calibration ou commande de réinitialisation n’a été essayée.

La balance est appliquée par pas de 10 : une tentative à 51 n’a pas été conservée par le firmware. Le volume et le retour vocal sont présentés de 0 à 100 dans l’API, avec respectivement 30 et 10 pas matériels.

Preuves locales : `research/sony/probe-with-inzone-closed.json`, `writes-verified.json`, `touch-readonly.json`, `touch-writes-verified.json`.

## Transport et validation

Récepteur `VID_054C`, `PID_0EC2`, collection de contrôle `usagePage=0xFF04`, `usage=1`, `MI_05/COL03`. Les autres collections ne sont pas utilisées.

Chaque report HID fait 64 octets : identifiant `0x02`, longueur du contenu, contenu, puis zéros. Une commande GET utilise le paquet `[01, 00, FC, 08, 96, C3, 41, event, 01, transactionLo, transactionHi, checksum]`. L’adresse `0x41` signifie PC → écouteurs ; le statut radio est adressé au récepteur (`0x21`). Les réponses utilisent `0x14` ou `0x12`.

Le checksum des commandes additionne les octets à partir de l’indice 4 ; celui des événements commence à l’indice 3. L’adaptateur refuse les paquets à checksum, longueur, clé vendeur, adresse ou type incorrects. Une réponse doit correspondre à l’événement, au type et à la transaction attendus.

Les valeurs longues Sony sont divisées en **paquets HCI complets contenant chacun 50 octets de paramètres**, avec la même transaction. Un dernier paquet de moins de 50 octets termine la valeur, y compris un paquet vide si nécessaire. Cette règle, trouvée dans l’assembly installé, a permis de lire la table tactile entière. L’adaptateur assemble ces paquets par adresse, événement, type et transaction, avec une limite de taille et une expiration.

GET=`0x01`, SET=`0x02`, RET=`0x10`, NTFY=`0x20`. L’assembly Sony déclare et traite également NTFY_ACTIVE=`0xA0` ; l’adaptateur l’accepte comme notification. Une notification spontanée ne remplace jamais la réponse attendue à une autre transaction.

Les écritures préservent les autres octets du réglage, puis font une nouvelle lecture. Si la nouvelle valeur n’est pas confirmée, l’API renvoie une erreur. Les réglages sont sérialisés par récepteur. Le modèle et le numéro du dongle sont relus avant toute modification pour empêcher une écriture après changement de récepteur.

## Événements utilisés

| ID | Réglage / format |
|---|---|
| `0x01` | Statut de connexion gauche/droite, destination récepteur |
| `0x02` | Modèle et identités, modèle Buds = 4 |
| `0x04` | `[statusL, percentL, statusR, percentR, statusCase, percentCase]` ; statuts 0 décharge, 1 charge, 2 erreur, `FF` inconnu ; pourcentage `FF` absent |
| `0x06–0x08` | Lectures groupées des batteries et réglages |
| `0x21` | `[mute, volume0…30, percent]` |
| `0x22` | Balance 0…100 : 0 = jeu, 100 = chat |
| `0x23` | `[sidetone0…10, percent]` |
| `0x24` | `[mute, FF, FF]` ; gain du micro hors protocole HID |
| `0x41` | `[mode0…2, ambient1…20, FF, voiceFocus0…1]` |
| `0x81` | Extinction automatique : 0 ou 15 |
| `0x83` | Langue : 0 anglais, 1 japonais, 2 chinois |
| `0x84` | Guidage : 0 ou 1 |
| `0x8C` | Table des capacités tactiles : côtés, profils, gestes et fonctions |
| `0x8D` | Affectations tactiles actuelles ; écriture vérifiée des profils par côté |
| `0x8E` | Bascule automatique de connexion lors d’un appel |

La table tactile annonce ANC/ambiant, volume, lecture multimédia, balance jeu/chat et aucune fonction. Les profils personnalisés annoncés par Sony ne sont pas édités. Dans les réponses `0x8D`, l’octet du profil dans l’entête du côté est le **profil par défaut d’usine** ; le profil actif est celui du bloc qui suit. Ce détail a été confirmé par lecture après modification.

## Égaliseur PC sans INZONE Hub

Modules : `app/server/sony-eq.mjs`, `sony-eq.ps1`, `sony-eq.cs`. Les dix fréquences sont **31,5 / 63 / 125 / 250 / 500 / 1 000 / 2 000 / 4 000 / 8 000 / 16 000 Hz**, avec des gains entiers de **−12 à +12 dB**. L’API expose `settings.eq={enabled,bands,frequencies}`, `eqSource='sony-windows-apo'` et la capacité `equalizer` seulement lorsque le traitement Windows Sony est actif et vérifié.

L’association est fondée sur l’identité physique : chemin HID Sony → PnP `ContainerId` → endpoint Windows du même conteneur, avec interface USB **MI_03**, déclarée comme Game dans le pilote Sony installé. Un nom de sortie audio n’est jamais utilisé pour choisir le périphérique. Le fichier ciblé est exclusivement `%APPDATA%/Sony/INZONE Hub/APO/{GUID-de-cet-endpoint}.yaml`. Les profils d’autres écouteurs, la sortie Chat et le microphone sont préservés.

Avant toute écriture, le module vérifie les CLSID SFX/MFX Sony de l’endpoint, les propriétés `IAudioSystemEffectsPropertyStore` dans leurs contextes Sony, les améliorations audio Windows activées, le format 48 kHz et l’effet Sony `f40a9945-82c0-48ea-807e-f742d48cb6be` à l’état ON dans `IAudioEffectsManager`. Ce GUID est également présent dans le binaire Sony `INZONEVirtualizer.dll` installé. Un flux partagé **initialisé mais jamais démarré** permet à Windows de fournir la liste de ses effets : aucune trame audio n’est envoyée. Un petit helper persistant compile ses déclarations COM une fois ; les sondes du moteur sont mises en cache pendant 20 secondes hors écriture.

Les paramètres et coefficients sont modifiés ensemble dans la seule section `equalizer`, avec calcul des filtres peaking selon les [formules RBJ publiées par le W3C](https://www.w3.org/TR/audio-eq-cookbook/). Les bandes inchangées conservent leurs valeurs numériques existantes. Les autres sections du YAML sont conservées byte pour byte. Un profil de fréquences différent, des masques de bandes partiellement activés ou une structure non reconnue sont refusés. Le fichier est remplacé atomiquement sur le même volume, avec conservation des ACL et sauvegarde du précédent fichier dans `.yaml.gear-control.bak`. Une vérification SHA-256 avant l’écriture protège contre un changement concurrent ; si la confirmation après écriture échoue, une restauration est tentée uniquement si le fichier contient encore notre propre remplacement. Le registre et `SoundProfile.json` ne sont pas modifiés.

La méthode de sauvegarde observée dans l’application Sony écrit le YAML sans envoyer de commande HID ou de message supplémentaire au moteur. L’analyse statique du binaire APO montre les classes `ParameterFileObserver`, `PatrolFile` et les imports de lecture de dates de fichiers : le rechargement est géré dans le moteur. Cela ne constitue pas une mesure acoustique ou une lecture interne des coefficients chargés dans le DSP.

**Vérification réelle, INZONE Hub fermé :** profil initial `[-8,-7,-5,-4,-2,0,2,3,2,1]`, baisse de la bande 1 kHz de 0 à −1 dB, lecture du nouveau YAML et du moteur Sony actif, puis restauration byte pour byte du fichier original (SHA-256 `9517e5afe19a550838da722213d1726c0e80f88804cf447514892f5ea1ba5c39`). Un seul YAML, celui de la sortie Game Sony, a changé ; les sept autres sont restés identiques. Preuve : `research/sony/eq-verified.json`. Cette vérification prouve l’écriture, sa restitution exacte et la présence du moteur actif ; aucune capture audio ni mesure audible n’a été faite.

L’effet s’applique aux applications qui passent par la sortie **INZONE Buds Game** et les effets Windows. Les flux exclusifs ou RAW peuvent contourner ces effets. INZONE Hub est volontairement refusé comme concurrent pendant l’écriture et peut remplacer le fichier lors d’un futur lancement à partir de ses propres profils. Sans l’APO Sony, un autre moteur audio Windows serait nécessaire pour une indépendance complète vis-à-vis des composants Sony. Le son spatial et le gain du microphone ne sont pas exposés par cet adaptateur.

Références COM : [propriétés des effets audio Windows 11](https://learn.microsoft.com/en-us/windows-hardware/drivers/audio/windows-11-apis-for-audio-processing-objects) et [IAudioEffectsManager.GetAudioEffects](https://learn.microsoft.com/en-us/windows/win32/api/audioclient/nf-audioclient-iaudioeffectsmanager-getaudioeffects).

## Sources et usage de REA

Le [protocole OpenInzone](https://github.com/penguinwokrs/openinzone/blob/main/docs/PROTOCOL.md) fournit la structure et les plages des commandes. Le [pilote Buds de HeadsetControl](https://github.com/Sapd/HeadsetControl/blob/master/lib/devices/sony_inzone_buds.hpp) est une deuxième source pour les lectures batterie. L’ordre gauche/droite suit les observations documentées par OpenInzone, plutôt que les étiquettes inversées du pilote HeadsetControl.

[REA](https://github.com/morluto/rea) a été appelé sur `C:/Program Files/Sony/INZONE Hub/INZONEHub.dll` avec `inspect-managed-members`. Il a identifié l’assembly mais refusé l’inspection détaillée, car ses métadonnées dépassent le budget de représentation de 64 Mio. Le résultat exact est enregistré dans `research/sony/rea-inzone-managed-members.json` ; cette analyse ne prouve pas des méthodes qui n’ont pas été inspectées.

L’inspection statique locale a donc été complétée par [ILSpy](https://github.com/icsharpcode/ILSpy), installé uniquement dans le répertoire de recherche. Les déclarations d’événements, le format des paramètres et la segmentation ont été vérifiés dans l’assembly de SHA-256 `506f19b48f67128b8edbba449d819990160e11a3073b53d3f6591a7d93d9b76f`. Aucun code décompilé Sony n’est incorporé dans l’adaptateur ; les captures et lectures décompilées restent des fichiers de recherche locaux.

REA a également été appelé sur l’APO natif. L’analyse détaillée était indisponible, car aucun moteur Ghidra/IDA/Hopper compatible n’était configuré ; `research/sony/rea-apo-inspect.json` conserve ce diagnostic. L’implémentation EQ repose sur les fichiers de configuration observés, les déclarations COM publiques de Microsoft, les constantes d’interopérabilité Sony et les vérifications matérielles. Elle ne charge pas une copie de la DLL Sony dans le serveur.

Vérification automatique : `node --test server/sony.test.mjs server/sony-eq.test.mjs` depuis `app` — tests couvrant l’intégrité HID, les segments, les sentinelles, la charge indépendante du boîtier, les gestes, le refus de commandes non reconnues, la validation EQ, les filtres, la préservation des sections YAML, la vérification du moteur, la restauration après échec et la conservation d’un profil modifié par un autre processus. Les fixtures de charge vérifient le décodage des valeurs déclarées par Sony ; elles ne constituent pas une observation matérielle d’une charge du boîtier.
