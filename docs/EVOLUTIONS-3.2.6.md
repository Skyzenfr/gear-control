# Gear Control 3.2.6 — un installateur plus vivant

## Onglet Installation

**Symptômes :** la page clignotait pendant la copie et les étapes s’affichaient mal. Toutes les étapes avaient le même aspect, « Démarrage du service » était sauté, et la liste n’était jamais entièrement cochée avant la page finale.

**Causes :**

- À chaque mise à jour de la progression (des dizaines par seconde), l’installateur réaffectait les six pastilles et le pourcentage. Ils étaient redessinés par-dessus l’image de fond, sans double tampon.
- Les libellés des étapes faisaient partie de l’image de fond : ils ne pouvaient pas changer d’état.
- L’étape « Démarrage du service » n’était jamais activée, et le passage à « tout terminé » arrivait seulement après l’affichage de la page finale.

**Corrections :**

- La fenêtre est dessinée en double tampon, et seuls les éléments qui changent réellement sont redessinés.
- Chaque étape est une ligne avec trois états : en attente (grisée), en cours (surlignée, « En cours… », sablier animé) et terminée (coche animée, « Terminé »).
- Les étapes suivent les événements réels de l’installation : fin de la copie, création des raccourcis, lancement du service. L’ordre de ces événements n’a pas d’importance. Chaque changement d’étape est noté dans le journal d’installation (`Checklist: step N`).
- Toutes les étapes s’affichent cochées un court instant avant la page « Installation terminée ».
- La barre de progression glisse vers sa valeur au lieu d’avancer par à-coups.
- Les petits carrés gris autour des pastilles ont disparu : leurs zones transparentes laissent voir le fond.

## Boutons et options

- Les boutons (Suivant, Retour, Installer, Ouvrir le dossier, Fermer, Lancer) s’éclairent progressivement au survol, avec un reflet et un léger glissement de leur icône. Ils s’enfoncent sous le clic.
- Le curseur devient une main sur tout ce qui est cliquable.
- Dans **Tâches supplémentaires**, chaque carte s’illumine au survol et toute la carte est cliquable, pas seulement l’interrupteur.
- Les interrupteurs glissent d’une position à l’autre et réagissent au survol.
- La touche Entrée déclenche toujours le bouton principal de la page. Avant, sur la page des tâches, elle pouvait rester sans effet.

## Pour les développeurs

- Les images de survol, des interrupteurs, du sablier, des coches et des lignes d’étapes sont générées depuis `packaging/installer-art/art.html`. `render-installer-art.mjs` lit désormais cette liste dans la page et supprime les images qui ne sont plus définies.
- Inno Setup ne fournit ni événement de survol ni minuteur aux scripts. L’animation repose sur un minuteur Windows (`SetTimer` avec `CreateCallback`) qui lit la position du curseur toutes les 25 ms.
