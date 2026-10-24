# Gear Control 3.2.7 — pochettes Apple Music et logos centrés

## Pochettes d’Apple Music

**Symptôme :** la carte média affichait une note de musique à la place de la pochette pendant une lecture dans Apple Music. Spotify et les navigateurs n’étaient pas concernés.

**Causes :** Apple Music publie ses pochettes différemment des autres lecteurs :

- le type annoncé est `image/jpeg,image/jpe,image/jpg`, alors que Gear Control n’acceptait que `image/jpeg` exactement ;
- l’image est en pleine taille (environ 260 Ko), au-delà de la limite de 256 Ko, et elle était donc écartée.

**Correction :**

- Le type de l’image est désormais déterminé d’après son contenu (signatures PNG, JPEG ou WebP), quel que soit le libellé envoyé par le lecteur.
- Les grandes pochettes (jusqu’à 8 Mo) sont réduites à 512 pixels en JPEG au lieu d’être écartées. Une pochette Apple Music pèse alors environ 60 Ko.

## Logos des applications

Les logos de Spotify, Brave, Apple Music et des autres applications étaient légèrement décalés dans leur pastille de couleur. Les marges internes faisaient un demi-pixel : avec la mise à l’échelle de Windows (125 % par exemple), le navigateur arrondissait la position du logo d’environ un pixel.

Les tailles des logos sont désormais choisies pour que leurs marges tombent sur des pixels entiers à 100 %, 125 % et 150 %. Cela concerne l’en-tête de la carte média, la barre supérieure et le mélangeur audio.
