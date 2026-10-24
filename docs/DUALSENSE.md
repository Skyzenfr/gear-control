# DualSense : réglages et test interactif

Dans les réglages d’une **DualSense ou DualSense Edge**, le visuel réagit aux entrées de la manette branchée au PC. Les boutons s’allument, les sticks suivent leur position et les gâchettes indiquent leur course. Cliquez sur une zone du visuel pour ouvrir son réglage ou son diagnostic. La télécommande mobile utilise la même manette, reliée au PC ; aucune connexion de manette au téléphone n’est nécessaire.

## Personnaliser

- **Éclairage** : activation, couleur libre, palette et intensité de la barre lumineuse.
- **Voyants** : les cinq points joueur, leur luminosité et le voyant du micro. Ce dernier ne change pas la sourdine Windows.
- **Vibrations** : force des deux moteurs, réduction de puissance et test de 0,1 à 3 secondes, avec arrêt automatique.
- **Gâchettes** : course libre, résistance, point de pression ou vibration, avec réglages indépendants de L2/R2. Les dix zones affichent l’emplacement de l’effet ; le soulignement suit la course réellement reçue.
- **Préréglages et profils** : Discret, Équilibré, Immersion et jusqu’à douze profils personnels par manette, enregistrés sur le navigateur utilisé.

Modifier un champ, choisir un préréglage ou charger un profil change l’aperçu. **Appliquer les réglages** envoie les préférences à la manette. **Tester les vibrations** envoie uniquement les forces des moteurs et la durée du test. **Arrêter les effets** arrête les vibrations et libère les gâchettes.

Les préférences affichées sont les dernières envoyées ou préparées, car la manette ne permet pas de relire ses couleurs et effets actifs. Après une reconnexion, appliquez-les à nouveau. Un jeu peut les remplacer.

## Réglages épinglés

Dans la vue d’ensemble, la roue dentée de **Réglages épinglés** permet de choisir neuf réglages DualSense : activation, couleur et intensité de la barre ; voyants joueur et leur luminosité ; voyant du micro ; intensités gauche/droite et durée du prochain test de vibration. Le choix est mémorisé pour cette manette et ce navigateur.

Les réglages lumineux épinglés sont envoyés immédiatement. Les intensités et la durée du test sont seulement enregistrées ; les moteurs se démarrent avec **Tester les vibrations** dans le configurateur. Les modifications épinglées se répercutent dans les réglages complets, en conservant les autres modifications locales en attente.

## Tester les commandes

L’onglet **Test des entrées** ouvre le diagnostic : positions XY et valeurs des sticks sans zone morte ajoutée, tracé facultatif des 64 derniers échantillons, pression L2/R2, boutons maintenus et liste des boutons détectés. Les compteurs d’appuis conservent les pressions très brèves entre deux lectures. **Réinitialiser le test** efface uniquement la session de diagnostic.

Les rapports complets fournissent aussi les deux contacts du pavé tactile et les mesures brutes du gyroscope et de l’accéléromètre. Les mesures de mouvement ne sont pas calibrées ni converties en unités physiques. Le mode Bluetooth de base ne transmet pas ces données ni le bouton micro ; une connexion USB permet les rapports complets. Le clic du pavé reste testable en mode de base.

La lecture fonctionne en USB et Bluetooth, ne modifie aucun réglage matériel et ne réalise aucun calibrage. Elle se met en pause quand la page est masquée ou que l’on quitte l’écran de la manette sur mobile. Le bouton Pause permet aussi de l’arrêter. Un signal interrompu ou une déconnexion remet les indicateurs au repos. Le remappage et les zones mortes utilisées en jeu restent à régler dans le jeu ou Steam.

## Notes techniques

Le service décode les rapports HID Sony de la session physique existante. Le navigateur lit `GET /api/devices/:id/controller-input?identity=…`, avec les mêmes protections d’identité, d’accès local et d’appairage mobile que les autres commandes. La lecture ne déclenche ni écriture HID, ni activation forcée du mode Bluetooth complet, ni sauvegarde des entrées. Les images instantanées, compteurs d’appuis et numéros de séquence restent en mémoire ; ils ne sont pas inclus dans les événements globaux ou l’inventaire. Le flux se suspend après 400 ms sans lecteur, et les entrées de plus de 250 ms sont neutralisées côté service.

Les rapports sont vérifiés contre les sources primaires du pilote [Linux hid-playstation](https://github.com/torvalds/linux/blob/master/drivers/hid/hid-playstation.c) et du pilote [SDL PS5](https://github.com/libsdl-org/SDL/blob/main/src/joystick/hidapi/SDL_hidapi_ps5.c). Les tests utilisent des rapports synthétiques et des sessions HID simulées ; ils ne valident pas le ressenti physique des moteurs ou des gâchettes.
