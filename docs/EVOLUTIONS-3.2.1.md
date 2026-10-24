# Gear Control 3.2.1 — images et configurateur Stream Deck

## Images personnalisées

Un grand PNG ou une image distante pouvait imposer sa taille naturelle à une ligne implicite de la grille de rendu. La zone visible restait limitée à la taille de la carte : le produit apparaissait alors très agrandi, coupé ou hors du cadre, même avec `object-fit: contain`.

L’image est désormais positionnée dans une boîte bornée par son conteneur, indépendamment de ses dimensions naturelles. Les proportions, le centrage et les marges restent conservés dans les cartes et dans la fiche principale. La grille interne ne peut plus grandir en fonction de l’image. Le traitement des marges transparentes et blanches est conservé ; les fichiers et liens enregistrés ne sont pas modifiés.

## Stream Deck Mini

Le chargement affectait la configuration à un état réactif Svelte, puis tentait de copier cet état avec `structuredClone`. Le navigateur refuse de cloner un proxy, d’où le message « could not be cloned » et l’absence des six touches.

Les copies passent maintenant par un instantané non réactif. Le chargement, l’application et l’annulation conservent chacun leur propre configuration. Les tests vérifient le refus des proxies par le clone natif : le substitut JSON précédent masquait cette erreur.

Après installation de **3.2.1**, rechargez le hub avec **Ctrl + F5**, puis ouvrez le Stream Deck dans **Réglages**. Les six touches doivent être accessibles. Si sa prise en charge n’était pas encore activée, cochez **Utiliser avec Gear Control**, configurez les touches et appliquez. Fermez le logiciel Elgato pour permettre l’accès au périphérique.

## Vérification

Les tests couvrent les styles réellement compilés, les cadres d’images de grandes dimensions, le cadrage automatique, le chargement du Stream Deck, la modification des touches, l’annulation et l’application avec un clone natif qui rejette les proxies. Les tests DOM ne mesurent pas les pixels d’un navigateur : aucune vérification visuelle native n’est déclarée dans cet environnement. Les commandes matérielles existantes ne sont pas modifiées par cette correction.
