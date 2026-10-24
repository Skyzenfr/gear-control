# Gear Control 3.2.18 — recherches et onglets

## Barres de recherche

Avec le focus, les barres de recherche affichaient jusqu’à trois contours superposés : celui du champ, celui de la zone de saisie et le contour de focus du navigateur. Elles n’affichent plus qu’un seul halo doux de la couleur d’accent, sur le champ entier, et l’icône loupe s’allume.

Sont concernées la recherche de **Réglages généraux → Périphériques**, celle du catalogue de compatibilité et celle des jeux dans **Profils de jeu**. Une classe commune (`search-field`) s’applique aussi aux futures recherches.

## Onglets des Réglages généraux

- Les huit onglets tiennent sur une seule ligne. Si la fenêtre est étroite, la barre défile horizontalement et l’onglet ouvert reste visible.
- Une pastille glisse vers l’onglet ouvert. Une seconde pastille, plus discrète, suit la souris d’un onglet à l’autre, et l’icône survolée se soulève légèrement.
- Le double contour sur l’onglet actif a disparu. Le contour de focus clavier est maintenant tracé à l’intérieur de l’onglet.

## Fiche de la souris

Le module de diagnostic de la souris n’est plus affiché dans la page de réglages.
