# Gear Control 3.2.8 — barre supérieure personnalisable et liste des périphériques

## Logo de la source média

**Symptôme :** dans le panneau média, le logo de l’application (Spotify par exemple) restait décalé d’environ 3 pixels vers la droite dans sa pastille de couleur, malgré la correction de la 3.2.7.

**Cause :** la pastille utilisait la classe `brand`, déjà utilisée par le logo Gear Control de la barre latérale. Elle recevait donc aussi sa marge horizontale (`padding: 0 7px`), ce qui poussait le logo vers la droite.

**Correction :** la pastille utilise désormais une classe qui lui est propre (`app-tile`), dans la carte média comme dans la barre supérieure. Le logo est centré avec 4 pixels de marge de chaque côté.

## Ordre de la barre supérieure

Vous pouvez choisir l’ordre, de gauche à droite, de l’action épinglée, des scènes, du média en cours et de la sortie audio dans **Réglages généraux › Apparence › Barre supérieure** :

- glissez un élément par sa poignée, ou utilisez les flèches ← → ;
- **Ordre par défaut** rétablit l’ordre d’origine.

L’ordre s’applique tout de suite, sans recharger les contrôles : un média en lecture ou un menu ouvert ne sont pas interrompus. Il est enregistré par le service local (`layout.json` dans le dossier de données) et suit donc toutes les fenêtres de Gear Control sur ce PC. Si un ordre enregistré est incomplet, par exemple après l’ajout d’un nouvel élément, les éléments manquants sont ajoutés à la fin.

## Périphériques hors de la zone visible

Quand la liste **Mes périphériques** de la barre latérale dépasse la hauteur disponible :

- le bas de la liste s’estompe pour montrer qu’il y a d’autres périphériques plus bas ;
- une petite pastille **+N** indique combien de périphériques restent cachés. Un clic fait défiler la liste ;
- une fois la liste défilée, le haut s’estompe à son tour.

Le compte est mis à jour quand la fenêtre change de taille ou que des périphériques sont ajoutés ou masqués.
