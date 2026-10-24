# Visuels des périphériques

Les fichiers sont servis localement par l'application dans `app/public/devices`. Ce sont des photos et rendus produit des constructeurs, de Thomann et des pièces jointes fournies par l’utilisateur, représentant les vrais modèles. Les illustrations vectorielles du SC440 et de la DualSense ont été retirées en version 2.12. Leur affichage ne lance aucun logiciel constructeur et ne demande aucune connexion à leurs sites.

## Fichiers utilisés

| URL locale | Modèle représenté | Dimensions | Source originale | Fond |
| --- | --- | --- | --- | --- |
| `/devices/gfits-white.png` | Logitech G FITS blancs, deux écouteurs | 1293 × 768 | Logitech G HUB, dépôt officiel installé, `front_white.png` | Transparent |
| `/devices/gfits-black.png` | Logitech G FITS noirs, deux écouteurs | 1293 × 768 | Logitech G HUB, dépôt officiel installé, `front_black.png` | Transparent |
| `/devices/sony-inzone-white.png` | Sony INZONE Buds WF-G700N blancs, deux écouteurs vus de face | 1000 × 1000 | [Sony Australie, PNG produit officiel](https://store.sony.com.au/dw/image/v2/abbc_PRD/on/demandware.static/-/Sites-sony-master-catalog/default/dw52fb4b79/images/WFG700NW/WFG700NW_1.png) | Transparent d'origine |
| `/devices/mad68-black.png` | MADLIONS MAD68 HE, variante noire du configurateur | 1068 × 386 | [Configurateur officiel FGG, MAD68 noir](https://hub.fgg.com.cn/devices/mad68/black.webp) | Transparent |
| `/devices/atk-a9-ultra-black.png` | ATK A9 Ultra noir, vue de dessus | 627 × 1102 | [Configurateur officiel ATK, A9 noir](https://bpcdn.atkgear.com/hub-v3/production/3.2.27/static/black-DnZlczbq.webp) | Transparent |
| `/devices/creative-pebble-pro.png` | Creative Pebble Pro noirs, deux enceintes | 720 × 720 | [Creative, PNG produit officiel](https://img.creative.com/images/products/large/pdt_23944.11.png?width=720) | Transparent d’origine |
| `/devices/sc440-user.png` | the t.bone SC 440 USB noir | 300 × 300 | Pièce jointe utilisateur, `image-removebg-preview.png` | Transparent, conservé |
| `/devices/dualsense-white.png` | DualSense blanche, vue frontale | 1000 × 563 | [Sony, photo produit officielle](https://gmedia.playstation.com/is/image/SIEPDC/dualsense-controller-product-thumbnail-01-en-14sep21?fmt=png-alpha&wid=1000), depuis la [fiche DualSense](https://www.playstation.com/en-us/accessories/dualsense-wireless-controller/) | Transparent d’origine |
| `/devices/ps5-console-user.webp` | PS5 Digital blanche | 3840 × 3840 | Pièce jointe utilisateur, `PlayStation5-Digital-CFI2100-06-1x1-01-09apr26$en.webp` | Transparence originale conservée |

Les couleurs Sony blanc, ATK noir et clavier noir ont été confirmées par l'utilisateur. Les deux paires G FITS sont identifiées par leurs numéros de série dans l'adaptateur. Le clavier utilise la variante noire du catalogue officiel correspondant à son modèle.

## Cadrage dans l’interface

`DeviceArt.svelte` utilise les limites du produit visible déclarées dans `device-art.ts`, mesurées sur les PNG. Un `viewBox` SVG exclut les marges du canevas ; aucune image source n’est retouchée. Toutes les variantes partagent une zone centrée et conservent leurs proportions. Dans une carte de 75 × 62 pixels, G FITS, Creative et INZONE occupent une largeur visible de 66 pixels, au lieu d’environ 67, 49 et 51 auparavant. Le même composant sert aux cartes, à la vue principale, aux réglages et aux scènes.

## Variantes supplémentaires

| URL locale | Dimensions | Source originale |
| --- | --- | --- |
| `/devices/sony-inzone-black.png` | 1094 × 820 | [Sony Japon, image produit noire](https://www.sony.jp/inzone/special/INZONE_BUDS/images/spec_black_1.webp) |
| `/devices/mad68-white.png` | 1068 × 384 | [Configurateur officiel FGG, MAD68 blanc](https://hub.fgg.com.cn/devices/mad68/white.webp) |

## Provenance et traitement

Les deux PNG Logitech ont été copiés sans modification depuis `C:/ProgramData/LGHUB/depots/876190/65f9e7a5-2f4c-4ee3-8b5c-10d856baf847/`. La [page officielle Logitech G FITS](https://www.logitechg.com/en-ae/shop/p/fits-true-wireless-gaming-earbuds) permet de vérifier le modèle. Les copies intégrées à l'application restent disponibles après fermeture ou désinstallation de G HUB.

Le catalogue décodé du [configurateur FGG](https://hub.fgg.com.cn/) associe le VID `373B`, PID `1057` à `MAD 68 Lightless`, titre `MAD68 HE`, couverture `mad68`, et thèmes `white` et `black`. Sa fonction d'image construit `/devices/{cover}/{skin}.webp`.

Le module du [configurateur ATK](https://hub.atk.pro/) version `3.2.27` associe `../assets/device/mouse/a9/black.webp` au fichier CDN `black-DnZlczbq.webp`. La [fiche officielle ATK A9](https://www.atk.store/products/atk-dragonfly-a9-series-lightweight-wireless-mouse) permet aussi de vérifier sa forme et ses variantes.

Le PNG Sony blanc actuel est directement lié depuis la [fiche officielle Sony Australie WF-G700N blanc](https://store.sony.com.au/headphones-gaming/WFG700NW.html). Il a été téléchargé et copié sans modification. Son alpha a été vérifié : 780 166 pixels sont transparents et 219 834 pixels sont opaques, épousant les silhouettes des deux écouteurs. Les deux logotypes SONY et les détails de grille sont ceux de l'image officielle. Le SHA-256 de la source et de la copie locale est identique : `AF78673E35247CC67607643961DA25F894A04FA3F88624B89A5F3D37B9FB9C4B`.

La variante Sony noire et l'ancien rendu Sony blanc proviennent de la [page officielle Sony Japon](https://www.sony.jp/inzone/special/INZONE_BUDS/). Leur fond blanc fait partie des sources. Les PNG Sony Scene7 examinés avaient aussi un rectangle blanc derrière les écouteurs. Deux essais de détourage par l'outil intégré imagegen ont été rejetés à cause de franges et de différences de microdétails ; aucun de ces essais n'est servi par l'application. L'ancien original blanc et les essais sont conservés dans `research/product-images/` pour comparaison.

Les WebP FGG, ATK et Sony noir ont seulement été réencodés en PNG avec Sharp, en conservant les dimensions et la transparence lorsqu'elle existait. Les PNG Logitech et Sony blanc actuels sont copiés sans réencodage. Aucun élément du produit dans ces PNG n'a été redessiné ou généré. Les originaux et le script de conversion sont conservés sous `research/product-images/`, hors du runtime de l'application.

Le PNG Creative a été téléchargé sans retouche depuis le CDN officiel, lié dans la galerie de la [fiche Creative Pebble Pro](https://us.creative.com/p/speakers/creative-pebble-pro). La galerie identifie `pdt_23944.11.png` comme la variante noire. Le paramètre CDN `width=720` fournit un PNG RGBA de 720 × 720 pixels ; la transparence d’origine a été conservée. Ce visuel représente le modèle confirmé via la sortie audio Windows, mais sa couleur est illustrative : elle n’est pas exposée par Windows. Aucun éclairage ni élément du produit n’a été ajouté.

En version 2.12.1, les visuels SC440 et PS5 sont les fichiers fournis par l’utilisateur, copiés sans retouche ni réencodage. Le SC440 n’a plus d’encart blanc. Le PNG DualSense Sony conserve sa transparence originale. Les limites du produit dans `device-art.ts` et le viewBox du bouton PS5 ne changent que le cadrage d’affichage. La couleur blanche de la coque DualSense et le modèle de PS5 sont illustratifs et ne sont pas lus depuis le PC.

Crédits des visuels : Logitech, Sony, MADLIONS/FGG, ATK, Creative et Thomann. Les marques et images restent la propriété de leurs ayants droit ; elles ne sont pas couvertes par une éventuelle licence du code de ce projet.
