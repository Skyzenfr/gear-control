# Gear Control 2.11

## Microphone SC440

Le SC440 apparaît dans les périphériques du PC et sur l’accueil mobile. Touchez sa carte pour régler **Niveau du micro** ou couper/rétablir le **Micro**. L’écran mobile **Audio** propose aussi un accès direct à sa fiche avec son état actuel.

Le service identifie le périphérique USB SC440 et son entrée de capture Windows. Les commandes utilisent cette entrée précise et relisent le niveau et la sourdine après application. Elles ne changent pas le microphone par défaut. Si le SC440 est débranché, les commandes sont désactivées.

## Manette PS5

Les manettes **DualSense** et **DualSense Edge** sont détectées par leur interface HID de jeu. Le hub indique leur connexion USB ou Bluetooth, le niveau de batterie et la charge lorsqu’ils sont transmis. Les pourcentages sont des estimations par tranches de 10 % ; les états inconnus restent inconnus.

Le mode Bluetooth de base ne transmet pas la batterie. Dans ce cas, la fiche explique l’absence de mesure ; une connexion USB permet sa lecture. Les rapports Bluetooth complets sont vérifiés par leur CRC. Gear Control ne force pas un changement de mode de la manette.

Lorsque l’identité physique est disponible, la même carte et son nom suivent la manette entre USB et Bluetooth. L’identité USB est lue sans écrire de configuration ; en Bluetooth, seule l’identité fournie par Windows est utilisée. Une identité absente ne provoque jamais la fusion de deux manettes différentes.

Le suivi est en lecture seule. Les vibrations, les gâchettes adaptatives, l’éclairage et les profils de boutons restent gérés par les jeux et logiciels habituels. Aucun réglage de réduction de bruit ou d’écouteurs n’est présenté pour la manette.

## Batterie sur mobile

Les cartes et fiches ne présentent une batterie que si le périphérique possède cette capacité. Les enceintes, le SC440 et le clavier MAD68 filaire ne montrent aucune batterie ni message de batterie indisponible. Une vraie batterie sans mesure exploitable reste distincte d’un périphérique sans batterie. Une déconnexion supprime les mesures affichées.

## Lancer des applications sur le PC

Dans **Accueil → PC connecté → Raccourcis**, ou **Plus → Contrôle du PC** :

- **FACEIT** lance le client installé. Si seul l’Anti-Cheat est installé, le bouton porte le nom **FACEIT Anti-Cheat** et lance cet outil.
- **Spotify** ouvre l’application Windows installée, indépendamment de la connexion à l’API Spotify.
- **Matchmaking FACEIT · Web** ouvre `https://www.faceit.com/fr/matchmaking` dans le navigateur par défaut du PC.

Ces commandes s’exécutent sur le PC associé au téléphone. Les applications absentes sont désactivées. Les raccourcis utilisent une liste fixe d’actions ; le téléphone ne fournit ni chemin d’exécutable ni URL arbitraire. Une session Windows ouverte reste nécessaire.
