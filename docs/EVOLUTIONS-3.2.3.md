# Gear Control 3.2.3 — Stream Deck à l’endroit et nouvel installateur

## Stream Deck Mini

**Icônes à l’envers.** L’image envoyée au Mini était retournée verticalement. L’encodage correspond désormais octet par octet à la capture USB de référence du Mini publiée par le projet `node-elgato-stream-deck` (transformation `rotate + yFlip`, pixels BGR). Un test vérifie chaque pixel selon cette transformation.

**Icônes effacées à la fermeture d’Elgato.** Le logiciel Elgato partage l’accès USB au Stream Deck. Pendant qu’il tourne, il remplace les visuels de Gear Control ; à sa fermeture, il efface les écrans. Gear Control détecte maintenant `StreamDeck.exe` :

- tant qu’il est ouvert, le configurateur affiche un avertissement : il faut le quitter, y compris depuis la zone de notification ;
- dès qu’il se ferme, Gear Control renvoie les six visuels et la luminosité, et les touches restent actives.

La détection utilise l’observateur de processus Windows lorsqu’il est disponible. Sinon, une vérification `tasklist` a lieu toutes les 3 secondes, uniquement quand des touches sont actives.

## Installateur

L’assistant adopte un thème sombre et violet, avec une barre d’étapes à gauche : Bienvenue, Emplacement, Tâches supplémentaires, Installation et Finalisation.

- **Emplacement** : le dossier d’installation se choisit à chaque installation, y compris lors d’une mise à jour. Par défaut, il reste dans `%LOCALAPPDATA%\Programs\GearControl`.
- **Tâches** : des interrupteurs pour le démarrage avec Windows, le raccourci sur le bureau et la télécommande, avec « Tout sélectionner ».
- **Installation** : une barre de progression et une liste d’étapes cochées au fil de l’installation.
- **Finalisation** : trois boutons, pour ouvrir le dossier d’installation, fermer l’assistant ou lancer Gear Control.

Les visuels sont générés depuis `packaging/installer-art/art.html` (voir `INSTALLATION.md`). Les pages sans visuel dédié, comme une erreur d’arrêt du service, conservent la mise en page standard d’Inno Setup.

## Vérification

- **Stream Deck :** les tests couvrent l’orientation, ainsi que l’avertissement puis le renvoi des visuels après la fermeture d’Elgato.
- **Installateur, pages Bienvenue, Emplacement et Tâches :** vérifiées sur une version d’essai réelle, avec la navigation et les interrupteurs.
- **Installateur, pages Installation et Finalisation :** seule la composition de leurs visuels a été vérifiée, hors écran.
