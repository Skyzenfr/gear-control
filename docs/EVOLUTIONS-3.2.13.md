# Gear Control 3.2.13 — nouvelle identité et réglages dans la vue d’ensemble

## Nouvelle identité visuelle

Gear Control adopte son nouveau logo : un anneau ouvert, une sphère centrale et une pièce de contrôle en forme de « 7 », dans un dégradé violet lumineux.

- Dans la barre latérale, le logo s’anime à l’ouverture, en suivant le guide de motion : l’anneau se trace, la sphère apparaît, la pièce de droite vient s’assembler, le logo marque un léger rebond lumineux, puis le nom « Gear Control » glisse en place. Ensuite, une lueur très douce respire en boucle.
- L’icône de l’application (raccourcis, barre des tâches, installateur, zone de notification) et le favicon reprennent le nouveau logo. Aux petites tailles (16 à 32 px), seul le symbole est affiché, sans sa tuile, pour rester lisible.
- Les animations sont désactivées lorsque Windows demande de réduire les mouvements.

## Un seul thème : sombre OLED violet

Le choix entre thème « Sombre » et « Sombre OLED » disparaît. L’interface utilise désormais un fond noir pur, avec des surfaces légèrement teintées par la couleur d’accentuation. Le violet devient la couleur par défaut. Dans **Réglages généraux → Apparence**, seule la couleur d’accentuation reste à choisir.

Les préférences existantes sont conservées : une couleur déjà choisie reste appliquée, sur le nouveau fond OLED.

## Les réglages directement dans la vue d’ensemble

L’onglet **Réglages périphériques** est supprimé. La vue d’ensemble garde la grille de cartes en haut. En dessous, pour l’appareil choisi :

1. **Le grand bloc du périphérique** : visuel, état de connexion, batterie, contrôle du bruit pour les INZONE Buds, et le bouton **Ouvrir les réglages**.
2. **Paramètres rapides**, à côté : les réglages que vous épinglez pour cet appareil. Chaque changement est appliqué immédiatement, sans bouton **Appliquer**. Le bouton ⚙ permet de choisir les réglages affichés ; ce choix est mémorisé pour chaque appareil.
3. **Tous les réglages** de l’appareil, plus bas : ceux de l’ancien onglet, avec **Appliquer** et **Annuler**, le nom, le visuel et les outils avancés. **Ouvrir les réglages** et **Voir tous les réglages** descendent directement à cette partie.

La recherche Ctrl+K mène désormais aux réglages dans la vue d’ensemble.

## Autonomie restante

Sous le pourcentage de batterie, Gear Control affiche une estimation du temps d’utilisation restant, par exemple « ≈ 4 h 30 min ».

- Pendant l’utilisation, l’estimation suit la décharge réellement observée : elle apparaît après une quinzaine de minutes et au moins 2 % de batterie consommés.
- Gear Control retient la vitesse de décharge de chaque appareil. Lors des sessions suivantes, une estimation est donc disponible dès le branchement.
- Pour les INZONE Buds, l’autonomie annoncée par Sony (12 heures) sert de point de départ jusqu’à la première mesure.
- Rien ne s’affiche pendant la charge. Survolez l’estimation pour savoir sur quoi elle repose.

## La liste des périphériques dans Réglages généraux

**Réglages généraux → Périphériques** commence par la liste **Mes périphériques**. Chaque ligne montre l’appareil, sa marque, sa connexion, sa batterie et son état. La liste propose :

- des filtres par type avec leur nombre, et une recherche ;
- un clic sur une ligne pour ouvrir les réglages de l’appareil dans la vue d’ensemble ;
- un menu **…** pour ouvrir les réglages ou l’égaliseur, ou pour masquer ou réafficher l’appareil dans la vue d’ensemble.

L’ajout et la découverte de nouveaux appareils restent disponibles juste en dessous.
