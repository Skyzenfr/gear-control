# Gear Control 2.12

## Réglages DualSense sur PC et mobile

La fiche de la manette et la page Réglages permettent de préparer les effets, puis de les envoyer avec **Appliquer** :

- **Éclairage** : couleur RGB, activation et luminosité de la barre lumineuse ; sélection individuelle des cinq voyants joueur ; intensité des voyants ; voyant micro éteint, fixe ou pulsé.
- **Vibrations** : intensité indépendante des moteurs gauche et droit, test de 0,1 à 3 secondes avec arrêt automatique, réduction de puissance.
- **Gâchettes** : réglage indépendant de L2 et R2, modes libre, résistance, point de pression et vibration ; début de l’effet, force, fin du point de pression ou fréquence selon le mode ; réduction de puissance des effets.
- **Arrêter les effets** : stoppe les vibrations et libère les deux gâchettes.

Les préférences sont enregistrées sur le PC par identité physique de la manette. Après une reconnexion, cliquez à nouveau sur Appliquer ; une actualisation ne déclenche pas d’effet. Les scènes peuvent conserver l’éclairage et les gâchettes, sans lancer de test de vibration.

La manette ne fournit pas de relecture de sa couleur ni de ses effets actifs. Le hub distingue donc **réglages envoyés** et **préférences enregistrées**, sans annoncer une confirmation matérielle qu’il ne possède pas. Un jeu ou Steam peut remplacer ces effets. Une commande explicite en Bluetooth peut activer les rapports étendus jusqu’à la reconnexion ; aucune actualisation ne force ce mode.

Le remappage des boutons, les profils internes spécifiques à la DualSense Edge, la calibration et le routage audio de la manette ne sont pas configurés ici. Les profils et remappages des jeux ou de Steam restent utilisables. Gear Control ne modifie pas le firmware, l’appairage ou la mémoire interne de la manette. Le voyant micro change l’éclairage du bouton et ne coupe pas le SC440.

## Charge de la manette

Les fiches montrent explicitement **En charge**, **Batterie pleine**, **Sur batterie**, **Charge interrompue** ou **État de charge inconnu**, selon les informations reçues. Un câble USB branché ne suffit pas à déduire une charge en cours. Un état périmé est effacé lors d’une déconnexion. Le mode Bluetooth de base peut ne pas transmettre la batterie.

## Son PS5 dans la barre supérieure

Le bouton **Son PS5**, avec la photo de la console, apparaît sur PC et mobile. Le bouton principal ouvre le panneau de contrôle ; le bouton audio adjacent active ou désactive **Écouter ce périphérique** pour l’entrée ligne Realtek transportant le son de la PS5. Le libellé et l’état du bouton changent après relecture Windows.

La destination d’écoute déjà réglée dans Windows est conservée. Le SC440 et le volume des autres appareils restent indépendants. Si l’entrée est absente ou ambiguë, le bouton devient indisponible. Voir [le fonctionnement Windows et sa vérification](PS5-AUDIO.md).

## Photos produit

Les illustrations vectorielles du SC440 et de la DualSense ont été remplacées par des photographies produit Thomann et Sony. La photo de PS5 provient aussi de Sony. Elles sont conservées localement et leur affichage ne contacte aucun site constructeur. Les sources et cadrages figurent dans [les crédits des visuels](DEVICE-IMAGES.md).

## Correctifs 2.12.1

Le rapport USB DualSense utilise les 48 octets attendus sous Windows (les 47 octets des effets plus l’identifiant). Les rapports Bluetooth restent à 78 octets avec CRC. Les écritures incomplètes restent rejetées ; un remplissage natif supplémentaire est accepté. Un envoi USB sans vibration a été vérifié sur la manette connectée : 48 octets envoyés, 48 acceptés. Référence du format USB : [implémentation SDL](https://github.com/libsdl-org/SDL/blob/main/src/joystick/hidapi/SDL_hidapi_ps5.c).

Le contrôle Son PS5 reprend la hauteur, le survol, le focus clavier et le panneau déroulant des commandes voisines. Le bouton audio séparé conserve le basculement rapide. Les photos SC440 et PS5 sont remplacées par les pièces jointes utilisateur, avec leur transparence et sans retouche. Le serveur déclare aussi correctement les types MIME WebP et JPEG.
