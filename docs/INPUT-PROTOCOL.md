# Accès USB direct au clavier MAD68 et à la souris ATK A9 Ultra

Inspection et essais réalisés le 11 octobre 2026 sur le PC de l’utilisateur. L’adaptateur est `app/server/adapters/input-devices.mjs`. Il ne charge aucun logiciel ni code constructeur à l’exécution : `node-hid` utilise les interfaces HID Windows existantes. Les bundles analysés restent dans `research/`, ignoré par Git, et ne sont pas distribués avec la webapp.

## Sources et REA

Sources publiques téléchargées en lecture seule :

- [ATK HUB](https://hub.atk.pro/), bundle [index-F-MYsmWR.js, version 3.2.27](https://bpcdn.atkgear.com/hub-v3/production/3.2.27/static/index-F-MYsmWR.js), SHA-256 `275ec10d7bf0916fea8f27f6cc6415c3eff6e02e682c8164e286626f0f394256`.
- [FGG HUB](https://hub.fgg.com.cn/), [index-YgtSwnZj.js](https://hub.fgg.com.cn/assets/index-YgtSwnZj.js), SHA-256 `9c073a429d4b8f54515275394ba54e2219acda7b5557e23d410088da3198e513`.
- FGG transport et inventaire des produits : [index-CfSMCN5O.js](https://hub.fgg.com.cn/assets/index-CfSMCN5O.js), SHA-256 `c2855cf201713fe102b110f96423f70310c8e7623afc0a943119a66d6dd27418`.

Les chaînes FGG sont indirectes : la table littérale, sa rotation et sa fonction de résolution ont été isolées et évaluées dans un contexte JavaScript sans accès au réseau ni au système. Le bootstrap de l’application n’a pas été lancé. Les noms de propriétés reconstruits ont servi à lire les formats.

[REA 6.4.0](https://github.com/morluto/rea) a été exécuté via `analyze-javascript-application`. L’analyse de l’intégralité des gros bundles a atteint sa limite de 60 secondes et a conservé une observation partielle. L’analyse ciblée des déclarations Compx et VIA a ensuite terminé en environ quatre secondes :

```text
rea analyze-javascript-application research/input-focus --analysis-timeout-ms 30000 --format json
evidence_id: ev_9729bff0538191619e7804008c7d8405fd2e72e589250bc10e7b5d92e1b821b8
digest: 9d66578010a569d06c6229c392f123ca0b4b791ae7aecd79d26ce87ecb96b433
```

La provenance des plages se trouve dans `research/input/focused-provenance.json`; les résultats statiques sont `rea-input-analysis.json` et `rea-focused-analysis.json`. L’analyse statique indique des structures et relations, elle ne prouve pas le fonctionnement du matériel. Les lectures et écritures HID ci-dessous ont apporté cette confirmation.

## ATK A9 Ultra

Le périphérique USB `373B:101B`, nommé `ATK Mouse 8K Dongle`, regroupe plusieurs collections. Le site WebHID sélectionne `FF04:0002`; sous Windows, les rapports de configuration n° 8 sont effectivement accessibles avec `node-hid` sur la collection `FF02:0002`, interface 1. Les autres collections ont refusé les écritures avec `ERROR_INVALID_FUNCTION`. Seule la collection validée est ouverte par l’application.

Le rapport OUTPUT contient un ID `08`, puis 16 octets :

| Position dans le payload, hors report ID | Signification |
| --- | --- |
| 0 | Commande |
| 1 | Statut de réponse, zéro pour le succès |
| 2–3 | Adresse EEPROM, entier big endian |
| 4 | Longueur de données |
| 5–14 | Données |
| 15 | Checksum : somme du report ID et du payload entier = `0x55` modulo 256 |

Les réponses INPUT ont le même report ID. Requête de lecture EEPROM confirmée : `08 08 00 00 0C 08 00 00 00 00 00 00 00 00 00 00 31`.

Commandes utilisées : 3 = souris en ligne, 4 = batterie, 16 = CID/MID, 18 = firmware, 8 = lecture EEPROM, 7 = écriture EEPROM. CID 2 / MID 87 correspond à `ATK A9 Ultra`, capteur `PAW3950Ultra` dans l’inventaire ATK. Firmware observé `3.01`.

La lecture de batterie a renvoyé 45 %, charge inactive. Le statut « en ligne » vient du premier octet de données, et non du champ longueur. Lorsque ce statut vaut zéro, la webapp affiche la veille et masque les réglages/batterie : des réponses EEPROM ou batterie du dongle peuvent être mises en cache. Déplacer physiquement la souris la réveille.

EEPROM : le bloc 0 de 10 octets contient polling, nombre de niveaux et niveau DPI actif. Encodage polling : `1=1000`, `2=500`, `4=250`, `8=125`, `16=2000`, `32=4000`, `64=8000` Hz; l’octet suivant vaut `85-valeur` modulo 256. Une écriture de polling préserve les huit autres octets, notamment les niveaux DPI et les modes des boutons.

Les huit niveaux DPI sont stockés par paires, blocs de huit octets aux adresses 12, 20, 28, 36. Chaque niveau contient X, Y, bits d’extension et checksum `85-(X+Y+extension)`. Le niveau actif observé était zéro, avec `4F 4F 00 B7`, soit 800 DPI sur les deux axes. Pour le capteur Ultra, la plage fine jusqu’à 10 000 se calcule avec `(base+1)*10`; l’extension passe ensuite à `base*50+10050`. L’application accepte 10–30 000 DPI, pas de 10 jusqu’à 10 000 et pas de 50 ensuite. Modifier le DPI concerne uniquement le niveau actif, sur X et Y. Les autres niveaux restent intacts.

### USB filaire et charge

La souris branchée par câble est un périphérique distinct du dongle : `373B:1119`, produit `ATK A9 Ultra`, firmware `3.01`. Le catalogue ATK contient exactement le PID décimal 4377, le capteur `PAW3950Ultra` et CID/MID `2,87`. Sur le PC, seule sa collection `FF02:0002`, interface 1 / `Col05`, est ouverte. La détection ne s’étend pas aux autres PID ou aux collections de souris/clavier protégées. Les rapports n° 8, les blocs EEPROM et l’encodage des DPI sont identiques au transport du dongle.

Les lectures filaires réelles ont retourné :

| Commande | Payload reçu sans report ID | Interprétation |
| --- | --- | --- |
| 3 | `03 00 00 00 01 01 66 22 FB 00 00 00 00 00 00 C5` | Online = 1 ; identifiant radio `66:22:FB` dans l’ordre du paquet |
| 16 | `10 00 00 00 03 02 57 FC 00 00 00 00 00 00 00 E5` | CID 2 / MID 87 ; les quatre octets `uniqueId` du site ne sont pas renseignés |
| 4 | `04 00 00 00 02 3C 01 10 6D 00 00 00 00 00 00 8D` | Batterie 60 %, charge = 1 |
| 1 | `01 00 00 00 08 48 15 7A F2 02 57 03 00 00 00 1F` | Type `wired8K` = 3, CID/MID concordants |

La commande 1 est la requête d’information normale du configurateur (`getMouseInfo` / `AD.get`), avec quatre octets de challenge et quatre zéros ; elle ne sert pas ici à transférer du firmware. Le type filaire est lu une fois par ouverture de transport. Les valeurs officielles 2, 6, 7 et 3 correspondent respectivement à 1, 2, 4 et 8 kHz ; l’interface ne propose ni n’accepte une fréquence supérieure à cette capacité. La souris testée annonce bien 8 kHz, même si son réglage actuel est 1 000 Hz.

Le champ charge de la commande 4 vaut 0 pour décharge et 1 pour charge. L’adaptateur publie `battery.charging` seulement à partir de ce champ : aucune déduction n’est faite depuis la présence d’un câble. Une valeur inconnue ou une réponse dépourvue des deux octets utiles donne `null`. Une interface indisponible ou une souris en veille conserve une batterie et une charge inconnues. `connectionType` distingue `USB filaire` et `2,4 GHz`.

### Deux interfaces d’une même souris

Le site ATK utilise les trois octets RF de la commande 3 comme identifiant de souris (`getWirelessMouseOnline`). Une fusion exige un identifiant non nul/non entièrement `FF`, CID/MID vérifié `2,87` et au plus une interface USB 1119 plus une interface dongle 101B. Une égalité de modèle, de DPI ou de batterie n’est jamais utilisée pour identifier deux appareils.

En cas d’égalité RF exacte, une seule carte est publiée. Son identité est dérivée de CID/MID et RF, indépendamment de l’USB, du port et de l’ordre de découverte ; elle reste identique après redémarrage. Les lectures et écritures passent en priorité par la connexion filaire active. Le retrait d’une interface ferme uniquement son propre transport. Le bail d’accès à l’outil officiel libère toutes les interfaces de cette paire vérifiée ; pendant ce bail, aucune requête HID de l’adaptateur n’est envoyée.

La lecture radio réelle du récepteur 101B a confirmé `66:22:FB`, CID/MID `2,87`, identique à la lecture filaire 1119. Cette vérification a utilisé le bail de libération USB, puis rendu l’accès au service ; aucune configuration n’a été écrite.

Les associations entre collection et RF déjà vérifiées sont conservées dans `mouse-connections.json`. Elles permettent de regrouper un dongle silencieux pendant que le câble est actif. Cette mémoire ne donne aucune autorisation d’écriture : le transport choisi doit être connecté et son identité relue en direct. Un RF différent reçu remplace l’association précédente. Un récepteur indisponible sans souris identifiée n’ajoute pas une carte fictive. Deux souris actives d’identités différentes restent séparées. La fusion, les deux ordres de débranchement, le redémarrage, le remplacement d’une souris et le refus d’écritures périmées sont vérifiés par tests avec paquets enregistrés.

## MAD68 HE Lightless

USB `373B:1057`, série `MAD HE`, collection `FF60:0061`, interface 1. Le catalogue FGG le désigne `MAD 68 Lightless` / `MAD68 HE`, modèle sans RGB. Le transport appelé `DuckBread` dans le bundle utilise un protocole VIA avec extensions fabricant.

Variantes acceptées : dans `index-CfSMCN5O.js`, les filtres `MAD 68 RGB` (`1058`), `MAD 68 Ultra` (`1059`) et `MAD 68 Limit` (`105A`, `105C`) utilisent la même classe `DuckBread`, la même collection `FF60:0061`, la même matrice 5 × 15 et la même version de touches avancées 1.03 que le Lightless. Seul le module de disposition visuelle diffère. Ces variantes reçoivent donc les mêmes réglages ; chaque écriture reste relue avant confirmation. Elles n’ont pas été vérifiées sur du matériel. Les MAD68 HE V2 (`1123`–`1125`), MAD68 Pro/R et les MAD60 (matrice 5 × 14) restent exclus.

Éclairage RGB : la classe `LightInfo` lit avec `08 41` et écrit avec `07 41 effet 00 vitesse R V B luminosité`. Les composants qui définissent la liste des effets et les plages de vitesse et de luminosité ne figurent pas dans les bundles analysés. Aucune commande d’éclairage n’est donc envoyée : le RGB reste dans l’outil officiel tant qu’une lecture réelle n’a pas été faite sur un MAD68 HE RGB.

Rapports OUTPUT sans ID numéroté : `node-hid` reçoit un zéro initial, puis 32 octets de payload. INPUT retourne directement 32 octets. Aucune interface clavier protégée ni rapport de frappe n’est ouvert ou écrit.

| Commande / début du payload | Fonction utilisée |
| --- | --- |
| `02 96 00` | Version fabricant, observée `1.06` |
| `02 96 11` | Fonctions générales : WASD, Mac, verrou Windows, NKRO |
| `12 offset_hi offset_lo longueur` | Lecture de la matrice de touches; aucun remappage |
| `02 96 03 ligne colonne` | Point d’activation d’une touche |
| `03 96 03 ligne colonne valeur_hi valeur_lo` | Modifier le point d’activation |
| `02 96 07 ligne colonne` | Paramètres Rapid Trigger d’une touche |
| `03 96 07 ligne colonne actif release_hi release_lo press_hi press_lo` | Modifier le Rapid Trigger |
| `03 96 11 rgb wasd mac win_lock nkro` | Modifier le verrou Windows en préservant les autres valeurs |

Les distances du protocole V2 sont des entiers big endian en centièmes de millimètre. La matrice réelle contient 5 × 15 positions dont 68 touches utilisées. Elle est lue en six fragments, puis les labels sont déduits de leurs codes HID présents. Les lettres affichées correspondent aux codes matériels, indépendamment du layout Windows. Les classes VIA LT0 à LT3 sont présentées comme FN.

Chaque périphérique garde une sélection de touche dans la mémoire de l’adaptateur. `{key:"2:1"}` sélectionne A et relit ses valeurs, sans modifier de réglage. Les distances et RT ne concernent ensuite que la touche sélectionnée. Le verrou Windows concerne le clavier entier.

Depuis la version 2.4, l’interface démarre sans aucune touche sélectionnée. La lecture groupée `{keys:["2:1","2:5"]}` ne modifie rien dans le clavier. `settings.keySettings` conserve les dernières valeurs lues par position, et `settings.keySettingsReadAt` leur horodatage. La carte thermique déclenche une lecture explicite des touches ; elle distingue les valeurs inconnues et inclut les modifications en attente. Les actualisations normales relisent uniquement la première touche sélectionnée, sans rescanner les 68 touches. Débranchement, fermeture du transport et accès à l’outil officiel invalident les snapshots.

Une écriture groupée utilise des patches, par exemple `{keySettings:{"2:1":{actuation:1},"2:5":{actuation:1}}}`. L’intégralité des clés et valeurs est validée avant la première écriture. Modifier une distance RT commence par lire les valeurs réelles de chaque touche et préserve son autre distance et son état RT individuel. Chaque écriture est relue et confirmée avant de passer à la suivante. Un échec en cours de lot peut laisser les premières touches appliquées : le service signale l’échec et relit l’état, sans prétendre à une transaction matérielle atomique.

Ctrl / Maj + clic et rectangle ajoutent une sélection multiple ; les champs affichent leurs valeurs différentes. Les boutons +/− incrémentent chaque valeur individuelle. Ctrl Z et l’historique préparent un patch inverse, y compris après application ; ce patch suit le même bouton Appliquer. Copier / coller porte sur les quatre réglages supportés. Réinitialiser rétablit les premières valeurs lues pendant la session, sans émettre une réinitialisation usine. Les profils CS2, FPS et Bureautique sont conservés par identité dans le navigateur ; leurs suggestions sont explicitement indiquées et ne s’appliquent qu’aux touches choisies.

Swap WASD / flèches utilise le champ WASD déjà identifié du paquet général `02/03 96 11`, octet 4. Il est proposé uniquement si la lecture donne 0 ou 1 et l’écriture préserve RGB, Mac, verrou Windows et NKRO.

Le 13 octobre 2026, des requêtes GET seules ont confirmé les formats FGG des groupes RS/SOCD (`02 96 20 00 00 01 index 00 id`), d’un slot Tap Dance (`FE 0D 01 index`) et du buffer ADC/course (`02 96 1C 00 00 offset_hi offset_lo taille`). Les groupes lus sont inactifs et ne désignent aucune paire (positions FF), le slot Tap Dance est vide et les courses au repos sont nulles. Ces lectures ne prouvent ni la modification sûre des groupes avec leurs remappages associés, ni la fidélité d’une télémétrie de mouvement en continu. SOCD, TGL, RS et la course en direct restent donc explicitement disponibles via l’outil officiel ; aucun paquet de calibration ni écriture avancée n’a été envoyé. Preuves : `research/input/advanced-keyboard-read-verification.json`.

L’interface cite [l’annonce officielle Valve du 19 août 2024](https://store.steampowered.com/news/posts/?appids=730&enddate=1724103958&feed=steam_community_announcements) sur l’automatisation des mouvements : certaines formes de contre-strafe assisté entraînent l’exclusion d’un match sur les serveurs officiels CS2. Le profil CS2 ne programme aucune automatisation des entrées opposées.

Valeurs observées : Échap 1,50 mm, RT actif, press/release 0,50 mm; A 0,50 mm, RT actif, press/release 0,02 mm. Verrou Windows inactif et NKRO actif. Les contrôles acceptent un point d’activation 0,10–3,80 mm et des distances RT 0,01–3,80 mm, par pas de 0,01.

## Vérification matérielle

Chaque modification suivante a été confirmée par une lecture réelle, puis restaurée et relue :

| Périphérique | Modification temporaire | Valeur restaurée |
| --- | --- | --- |
| A9 Ultra | DPI 810 | 800 |
| A9 Ultra | Polling 500 Hz | 1000 Hz |
| A9 Ultra USB 1119 | DPI 810 | 800 |
| A9 Ultra USB 1119 | Polling 500 Hz | 1000 Hz |
| MAD68 Échap | Activation 1,51 mm | 1,50 mm |
| MAD68 Échap | RT relâchement 0,51 mm | 0,50 mm |
| MAD68 Échap | RT désactivé | Activé |
| MAD68 | Verrou Windows activé | Désactivé |

Le premier test de changement de polling avait un délai de réponse de 800 ms : une réponse a expiré. La restauration a été confirmée. Le délai souris a été aligné sur les 3 secondes du transport ATK, puis le test complet 500 → 1000 Hz a réussi. Les essais sont conservés dans `research/input/*write-verification.json`, avec les sauvegardes initiales correspondantes.

Les essais USB 1119 ont confirmé la batterie 60 % et la charge active avant et après ces écritures. Les blocs EEPROM 0, 12, 20, 28 et 36 ont été relus et restaurés à l’octet près. Les preuves se trouvent dans `research/input/wired-mouse-probe.json`, `wired-mouse-info-probe.json`, `wired-mouse-write-backup.json` et `wired-mouse-write-verification.json`. Le branchement USB a été testé physiquement ; les retraits de câble/dongle sont couverts par simulation, sans prétendre à une vérification physique de ces transitions.

Tests de protocole et de refus des valeurs invalides : `node --test app/server/adapters/input-devices.test.mjs app/scripts/keyboard-settings.test.mjs`. Aucun de ces tests n’écrit au matériel automatiquement. Installation de firmware, réinitialisation usine, calibration, appairage, macros et remappage ne sont pas implémentés dans cet adaptateur.

### Réglages avancés A9 Ultra, version 2.4

Le 13 octobre 2026, un probe en lecture seule sur le récepteur 373B:101B, CID/MID 2/87 et firmware 3.01 a confirmé les blocs décrits dans le source officiel ATK 3.2.27. L’accès du service a été libéré par un bail, puis rendu dans `finally`. Aucun de ces nouveaux réglages n’a été changé physiquement. Les écritures sont vérifiées par fixtures contenant les paquets réels et une EEPROM mutable, avec tests de préservation des champs et de confirmation après lecture.

| Bloc / commande | Données lues | Réglages supportés |
| --- | --- | --- |
| EEPROM 169, 10 octets | `00 55 00 55 06 4F 00 55 00 55` | Motion Sync à +2, ligne droite à +6 ; fermeture éclairage à +4 |
| EEPROM 181, 6 octets | `00 55 06 4F 00 55` | Veille activée à +0, délai à +2, mode capteur à +4 |
| EEPROM 189, 4 octets | `00 55 00 55` | Angle signé à +0, activation à +2 |
| EEPROM 10, 2 octets | `04 51` | LOD Ultra : 1–11 = 0,7–1,7 mm, donc 1,0 mm observé |
| GET 23 / SET 22 | `00 00 00 00 00 00 00 00 00 00` | Mode longue distance, premier octet booléen |
| GET 25 / SET 24 | `00 FF 00 00 09 09 01 00 00 00` | Mode d’éclairage du récepteur, premier octet |

Les EEPROM avancées contiennent des paires valeur/checksum dont la somme doit valoir 85 modulo 256. Une réponse de longueur, checksum ou domaine incohérent retire la capacité concernée. Les écritures lisent tous les blocs nécessaires avant d’en modifier un, ne changent que les paires demandées, conservent chaque autre octet et sont relues. Les deux commandes du récepteur conservent les neuf autres octets, notamment couleur, vitesse, luminosité et extinction. Ces deux fonctions ne sont proposées que sur le transport sans fil validé.

Le « mode du firmware » demandé est le mode de performance du capteur (`sensorModel`) de la commande ATK `setMousePerformance`, pas une installation de firmware : 0 = base, 1 = compétitif ATK Shard, 2 = compétitif ATK Shard Max. Le catalogue de mise à niveau officiel autorise le mode Max à partir de 3.00 pour CID/MID 2/87 ; la souris présente est 3.01. L’interface garde l’installation de firmware dans l’outil constructeur.

Le délai de veille utilise des secondes : un octet représente dix secondes, plage 10–2550. Changer ce délai suit l’implémentation officielle en actualisant la fermeture d’éclairage et le délai capteur, puis en activant la veille. Un contrôle séparé permet de la désactiver. La rotation est un angle entier de −30° à +30°, négatifs encodés sur un octet signé, avec activation séparée. Une modification d’angle active la rotation. Les modes d’éclairage autorisés sont 0 éteint, 9 fixe, 8 respiration, 12 fréquence USB, 13 batterie et 14 batterie faible.

Le diagnostic publie l’heure de lecture, batterie et charge réelles, présence du récepteur, firmware et fréquence **configurée**. `measuredReportRate` et `connectionQuality` restent `null` : les rapports de configuration ne mesurent ni la cadence des mouvements ni la qualité radio. Preuves en lecture seule : `research/input/advanced-mouse-read-verification.json`.

## Pages officielles en complément

Les deux réponses HTTP principales, inspectées le 11 octobre 2026, ne comportaient ni `X-Frame-Options`, ni CSP `frame-ancestors`, ni `Permissions-Policy`. Cela permet de proposer un iframe avec `allow="hid"`. L’absence de ces en-têtes n’est pas une preuve d’accès WebHID : le navigateur doit supporter cette API, la permission USB doit être accordée dans le document du fabricant et les politiques peuvent changer.

Le complément reste explicitement identifié comme page officielle chargée depuis son origine. Aucun proxy ni retrait des en-têtes de sécurité n’est employé. La configuration native de DPI, polling, activation et RT fonctionne sans ces pages ni connexion Internet. Les fonctions complémentaires du fabricant peuvent nécessiter le site officiel.
