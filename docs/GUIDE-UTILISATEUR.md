# Gear Control

<img src="../app/public/brand/gear-control-128.png" alt="Logo Gear Control" width="80" height="80" />

Une interface locale en français pour **détecter, ajouter, organiser et régler vos périphériques** sur Windows. Chaque utilisateur compose son installation ; les contrôles audio Windows sont génériques, et les réglages propriétaires dépendent des adaptateurs compatibles avec le modèle. Le logiciel comprend notamment des adaptateurs pour **Logitech G FITS**, **Logitech HID++ 2**, **Razer DeathAdder V4 Pro**, **Wooting RGB**, **Sony INZONE Buds**, **Creative Pebble Pro**, **SC440**, **DualSense**, **MAD68 HE**, **ATK A9 Ultra** et **Stream Deck Mini**.

Les applications G HUB et INZONE Hub peuvent rester fermées. Les réglages des périphériques ne nécessitent aucun compte ni abonnement. Les favoris Spotify nécessitent une connexion au compte via son API officielle. L’égaliseur Sony nécessite **Windows 11 et le pilote audio Sony** : sur un nouveau PC, installez INZONE Hub une fois pour disposer de ce pilote, puis fermez l’application. Les batteries et le contrôle du bruit Sony fonctionnent sans cette dépendance.

## Installer et ouvrir le hub

1. Sur **Windows 10 ou 11, 64 bits**, téléchargez et lancez le dernier installateur `GearControl-Setup-<version>.exe` depuis [les versions publiées](https://github.com/Skyzenfr/gear-control/releases/latest).
2. Suivez l’installation. Elle ne nécessite ni droits administrateur ni installation de Node.js ; les composants nécessaires sont inclus.
3. Lancez **Gear Control** depuis le menu Démarrer, puis ouvrez [http://127.0.0.1:3210](http://127.0.0.1:3210). Vous pouvez aussi choisir **Ouvrir le hub** dans le menu de son icône près de l’horloge.
4. Au premier lancement, choisissez vos appareils dans l’assistant. Vous pouvez différer cette étape. Plus tard, utilisez **Ajouter un périphérique** ou **Réglages généraux → Périphériques** pour rechercher, ajouter, ignorer, renommer ou associer une fiche préparée à un appareil branché. La découverte ne modifie aucun réglage matériel.

L’installateur n’est pas signé commercialement : Windows peut afficher un avertissement SmartScreen à la première exécution.

**Démarrer avec Windows** est activé par défaut. Le service démarre à l’ouverture de votre session et se relance s’il s’arrête de façon inattendue. Fermer la page web laisse le service disponible. Le menu de l’icône permet de relancer le service, d’ouvrir les journaux ou de désactiver le démarrage automatique. **Quitter** l’arrête jusqu’au prochain lancement manuel ou à la prochaine ouverture de session, si le démarrage automatique reste activé.

## Utiliser vos périphériques

Les **réglages DualSense** disposent d’un aperçu interactif et d’un test des entrées en direct : boutons, sticks, course L2/R2, pavé tactile et capteurs selon les rapports disponibles. Les onglets réunissent éclairage, voyants, vibrations, gâchettes et profils locaux. Voir [le guide DualSense](DUALSENSE.md).

La version **3.2.29** ajoute cet aperçu DualSense, le diagnostic des commandes et les profils locaux. Voir [les évolutions](EVOLUTIONS-3.2.29.md).

La version **3.2.30** améliore les curseurs DualSense, corrige les proportions de la manette et dégage les quatre boutons et gâchettes L1/R1/L2/R2. Voir [les évolutions](EVOLUTIONS-3.2.30.md).

La version **3.2.31** raccorde L1/R1 à la coque, corrige les panneaux autour des boutons, allège l’en-tête DualSense et rend neuf réglages disponibles dans **Réglages épinglés**. Voir [les évolutions](EVOLUTIONS-3.2.31.md).

La version **3.2.32** allège le hero et les réglages, ajoute les visuels dans la recherche, cinq couleurs d’accentuation et un formulaire de profil en pleine largeur. Le volume par application est ouvert par défaut. Voir [le récapitulatif de la publication](EVOLUTIONS-3.2.32.md).

La version **3.2.33** harmonise les indicateurs de charge avec une batterie à éclair intégré et une pastille « En charge » dans le hero. Voir [les évolutions](EVOLUTIONS-3.2.33.md).

La version **3.2.34** remplace les cases à cocher par des anneaux de sélection et des interrupteurs, anime le survol des couleurs d’accentuation et donne de vrais boutons à **Ignorer** et **Retirer**. Voir [les évolutions](EVOLUTIONS-3.2.34.md).

La version **3.2.35** propose d’ajouter chaque appareil dès qu’il est branché, liste les appareils Bluetooth avec leur batterie et reconnaît les manettes Xbox et compatibles XInput. Voir [les évolutions](EVOLUTIONS-3.2.35.md).

La version **3.2.36** règle les écrans DDC/CI et les webcams, reconnaît les autres souris Razer, les claviers QMK/VIA et les manettes PS4, et permet de demander la prise en charge d’un appareil en un clic. Voir [les évolutions](EVOLUTIONS-3.2.36.md).

La version **3.2.37** configure toute la gamme Stream Deck (Mini MK.2, MK.2, XL, Pedal), nomme les appareils inconnus grâce à usb.ids et à un catalogue signé mis à jour avec chaque version, et laisse choisir si les familles connues peuvent être identifiées. Voir [les évolutions](EVOLUTIONS-3.2.37.md).

La version **3.2.38** signale les batteries faibles (notification Windows), affiche leur historique, lance des scènes selon la connexion ou la batterie d’un appareil et sauvegarde toute la configuration dans un fichier. Voir [les évolutions](EVOLUTIONS-3.2.38.md).

La version **3.2.39** pilote l’éclairage des contrôleurs gérés par OpenRGB, reconnaît les casques Logitech sans fil, règle la luminosité des écrans de portable, ajoute des actions « augmenter / baisser » et conserve les profils dans Gear Control plutôt que dans le navigateur. Voir [les évolutions](EVOLUTIONS-3.2.39.md).

La version **3.2.40** ajoute l’interface en anglais (Réglages généraux → Apparence → Langue) : tout l’écran du PC est traduit, y compris les textes envoyés par les périphériques. La télécommande du téléphone reste en français pour l’instant. Voir [les évolutions](EVOLUTIONS-3.2.40.md).

La version **3.2.41** étend les réglages des écrans ASUS : couleurs, GameVisual, GamePlus et protections OLED selon le modèle et les capacités annoncées, avec relecture des modifications et traduction anglaise. Les nouvelles écritures restent à vérifier sur matériel ; toutes les fonctions de DisplayWidget Center ne sont pas encore couvertes. Voir [les évolutions](EVOLUTIONS-3.2.41.md).

La version **3.2.42** ajoute les réglages AOC/AGON et un profil étendu AG276QZD2 pour les couleurs, le jeu, Light FX et l’OLED selon les réponses de l’écran. Les commandes étendues nécessitent le composant signé de G-Menu installé et l’application fermée. Support non vérifié sur matériel et encore partiel pour la gamme entière. Voir [les évolutions](EVOLUTIONS-3.2.42.md).

La version **3.3.0** publie ces réglages AOC/AGON et ajoute le nettoyage automatique des anciens installateurs après une compilation réussie et vérifiée par SHA-256, dans le projet et sa copie de compilation. Voir [les évolutions](EVOLUTIONS-3.3.0.md).

La version **3.3.1** permet de choisir et modifier les réglages des profils de jeu et du Bureau, avec retour au Bureau dès un Alt-Tab ou à la fermeture du jeu. Les automatisations et raccourcis combinent réglages, sortie audio, lancement d’application et touches clavier, selon la connexion d’un appareil ou le lancement d’une application. Voir [les évolutions](EVOLUTIONS-3.3.1.md) et [les exemples de configuration](windows-audio-automation.md).

La version **3.3.2** ajoute la sélection multiple de périphériques, plusieurs déclencheurs reliés par OU, les horaires, les événements de charge et le premier plan des applications. Les actions disposent de réglages éditables par appareil, de pauses, de la duplication et d’un historique détaillé. Voir [les évolutions](EVOLUTIONS-3.3.2.md) et [le guide des automatisations](windows-audio-automation.md).

La version **3.3.6** ajoute les actions **À l’arrêt de Gear Control**, notamment pour éteindre une lumière avant de quitter, reprend les anciennes règles de fermeture de `gearcontrol.exe` et corrige les automatisations affichées en double comme profils manuels. Voir [les évolutions](EVOLUTIONS-3.3.6.md).

La version **3.3.7** exécute aussi les actions **À l’arrêt de Gear Control** lorsque Windows s’arrête, redémarre ou ferme la session. La règle d’extinction existante est conservée ; un arrêt du PC annulé laisse Gear Control actif. Voir [les évolutions](EVOLUTIONS-3.3.7.md).

La version **3.3.5** ajoute les commandes **Allumer**, **Éteindre** et **Allumer / éteindre** pour les lumières Hue sur le Stream Deck, ainsi qu’un événement **Au démarrage de Gear Control** et un préréglage pour choisir l’état d’une lumière au lancement du service. Voir [les évolutions](EVOLUTIONS-3.3.5.md).

La version **3.3.4** rend Philips Hue facultatif et replié, détecte les ponts automatiquement sur le réseau local même sans Internet et permet de reconnecter un pont dont l’adresse a changé. Les diagnostics de connexion sont plus précis et la saisie manuelle reste disponible. Voir [les évolutions](EVOLUTIONS-3.3.4.md).

La version **3.3.3** ajoute les **lumières Philips Hue** via un pont Hue : allumage, luminosité, couleur et température du blanc selon les capacités de chaque lampe. Dans **Réglages généraux → Périphériques → Lumières Philips Hue**, cherchez le pont, appuyez sur son bouton physique puis associez-le. Ajoutez ensuite les lumières à votre espace. Hue Sync peut rester ouvert ; les réglages sont suspendus pendant une synchronisation active. Voir [les évolutions](EVOLUTIONS-3.3.3.md) et [le protocole et ses limites](HUE-PROTOCOL.md).

La version **3.2.28** ajoute **Examiner mon PC** (Réglages généraux → Périphériques). Ce bouton crée un rapport anonyme du matériel à envoyer pour demander la prise en charge d’un périphérique. Voir [les évolutions](EVOLUTIONS-3.2.28.md).

La version **3.2.27** ajoute des sons de confirmation discrets : réglages, égaliseur et profils appliqués, changement de sortie audio, action épinglée et commande refusée. Ils se coupent dans Réglages généraux → Apparence. Voir [les évolutions](EVOLUTIONS-3.2.27.md).

La version **3.2.26** rend le service plus fluide et plus sûr : plus de gels périodiques ni d’écritures disque inutiles, une commande ne relit plus que son périphérique, la télécommande s’active et se révoque depuis les réglages, et l’interface mobile télécharge environ huit fois moins de données. Voir [les évolutions](EVOLUTIONS-3.2.26.md).

La version **3.2.25** ajoute l’historique des versions dans l’onglet Mises à jour. On peut y rechercher, et chaque version affiche ses notes, sa date d’installation sur ce PC et son lien GitHub. L’onglet donne aussi accès au dépôt, aux installateurs et au signalement d’un problème. Voir [les évolutions](EVOLUTIONS-3.2.25.md).

La version **3.2.24** écrit tous les fichiers de données sur le disque avant de remplacer l’ancienne version, pour qu’une coupure ne les abîme plus. Une clé de télécommande illisible est remplacée au lieu de bloquer le service. Voir [les évolutions](EVOLUTIONS-3.2.24.md).

La version **3.2.23** permet au service de démarrer même quand l’inventaire et sa sauvegarde sont illisibles après une coupure. Il les reconstruit à partir des appareils connus et conserve les copies endommagées. Voir [les évolutions](EVOLUTIONS-3.2.23.md).

La version **3.2.22** remplace les jauges vides d’un périphérique déconnecté par une animation « Hors ligne » dans la vue d’ensemble. Voir [les évolutions](EVOLUTIONS-3.2.22.md).

La version **3.2.21** verrouille aussi les périphériques dont le logiciel du fabricant était déjà ouvert au démarrage de Gear Control, sans doublon dans la détection Windows. Voir [les évolutions](EVOLUTIONS-3.2.21.md).

La version **3.2.20** donne la priorité au logiciel du fabricant (Elgato Stream Deck, Logitech G HUB, Razer Synapse, Wootility, Sony INZONE Hub) : tant qu’il est ouvert, Gear Control ne communique plus avec le périphérique concerné, l’affiche en orange et grise ses réglages, puis reprend la main à sa fermeture. Voir [les évolutions](EVOLUTIONS-3.2.20.md).

La version **3.2.19** laisse entièrement le Stream Deck au logiciel Elgato tant qu’il est ouvert (y compris au démarrage de Windows) et le reprend automatiquement à sa fermeture : les deux logiciels n’écrivent plus jamais en même temps, ce qui pouvait bloquer le Stream Deck et le hub USB partagé avec la souris. Voir [les évolutions](EVOLUTIONS-3.2.19.md).

La version **3.2.18** affiche un seul halo de focus sur les barres de recherche, anime les onglets des **Réglages généraux** avec une pastille glissante et masque le diagnostic de la souris. Voir [les évolutions](EVOLUTIONS-3.2.18.md).

La version **3.2.17** permet de saisir l’autonomie annoncée par le fabricant dans **Réglages généraux → Autonomie** (la mesure réelle reste prioritaire), remplace le bas de la barre latérale par des crédits et allège les indicateurs du menu Affichage. Voir [les évolutions](EVOLUTIONS-3.2.17.md).

La version **3.2.16** fait passer au rouge les actions destructives, demande confirmation dans une fenêtre avant chaque suppression, redessine le **Test audio et balance stéréo**, retire les derniers fonds gris (télécommande mobile comprise) et allège le Stream Deck. Voir [les évolutions](EVOLUTIONS-3.2.16.md).

La version **3.2.15** passe l’interface en vrai OLED (noir pur, accent en contours et halos), rend défilante la liste des **Paramètres rapides**, réunit les deux modules de **Réglages généraux → Périphériques** en un seul et recentre le logo. Voir [les évolutions](EVOLUTIONS-3.2.15.md).

La version **3.2.14** corrige le démarrage du service : depuis la 3.2.13, il plantait faute d’un module absent de l’installateur. Voir [les évolutions](EVOLUTIONS-3.2.14.md).

La version **3.2.13** adopte la nouvelle identité Gear Control (logo animé, thème OLED violet unique), place les réglages de chaque appareil dans la vue d’ensemble avec un module de **Paramètres rapides** épinglés, estime l’autonomie restante et déplace la liste des périphériques dans **Réglages généraux → Périphériques**. Voir [les évolutions](EVOLUTIONS-3.2.13.md).

La version **3.2.12** refond la vue d’ensemble : un grand bloc réunit le visuel, la batterie et **Ouvrir les réglages**, et les périphériques s’affichent en liste avec filtres par type, recherche et menu d’actions. Le thème sombre suit la couleur d’accent. Voir [les évolutions](EVOLUTIONS-3.2.12.md).

La version **3.2.11** permet de réorganiser la barre supérieure directement depuis la barre, avec le même glisser-déposer que la grille des périphériques. Voir [les évolutions](EVOLUTIONS-3.2.11.md).

La version **3.2.10** remplit toute la zone de l’action épinglée au survol, comme le média en cours. Voir [les évolutions](EVOLUTIONS-3.2.10.md).

La version **3.2.9** se met à jour seule : elle détecte les nouvelles versions publiées sur GitHub, les vérifie (signature et empreinte) et les installe en un clic, sans dépendre d’un autre service. Voir [les évolutions](EVOLUTIONS-3.2.9.md).

La version **3.2.8** permet de choisir l’ordre des éléments de la barre supérieure, signale les périphériques cachés en bas de la barre latérale et centre le logo de la source média. Voir [les évolutions](EVOLUTIONS-3.2.8.md).

La version **3.2.7** affiche les pochettes d’Apple Music et centre précisément les logos des applications dans les médias et le mélangeur audio. Voir [les évolutions](EVOLUTIONS-3.2.7.md).

La version **3.2.6** rend l’installateur plus vivant : survol animé des boutons et des options, interrupteurs qui glissent, et un onglet Installation qui ne clignote plus et suit réellement chaque étape. Voir [les évolutions](EVOLUTIONS-3.2.6.md).

La version **3.2.5** n’affiche plus deux fois le même lecteur dans les médias, reconnaît Deezer et Apple Music, propose une nouvelle palette pour le fond des touches du Stream Deck et réduit le délai entre l’appui sur une touche et l’action. Voir [les évolutions](EVOLUTIONS-3.2.5.md).

La version **3.2.4** corrige le blocage du service (hub inaccessible) lorsque le Stream Deck est activé dans Gear Control. Voir [la correction](EVOLUTIONS-3.2.4.md).

La version **3.2.3** remet les icônes du Stream Deck à l’endroit et les renvoie après la fermeture du logiciel Elgato. Elle apporte aussi un nouvel installateur, qui permet de choisir le dossier d’installation. Voir [les évolutions](EVOLUTIONS-3.2.3.md).

La version **3.2.2** reprend automatiquement la configuration Elgato du Stream Deck. Elle ajoute aussi un configurateur de pages animé, l’action **Ouvrir un logiciel** et la prise en charge des MAD68 HE RGB, Ultra et Limit. Voir [les évolutions](EVOLUTIONS-3.2.2.md).

La version **3.2.1** corrige les images personnalisées agrandies ou coupées dans les cartes et la fiche du périphérique, ainsi que l’erreur `structuredClone` empêchant le chargement du configurateur Stream Deck. Les images d’origine et les configurations enregistrées sont conservées. Voir [les corrections](EVOLUTIONS-3.2.1.md).

La version **3.2.0** ajoute les commandes natives **DeathAdder V4 Pro**, **Logitech HID++ 2** et **Wooting RGB**. Les réglages disponibles dépendent du modèle, de la connexion et des réponses matérielles ; les fonctions analogiques Wooting restent dans Wootility. Le panneau **Réglages** permet de modifier ou supprimer l’image du périphérique, et l’icône de l’action épinglée suit son état audio confirmé. Le configurateur Stream Deck est aussi accessible dans Réglages. Voir [les fonctions, limites et parcours de vérification](EVOLUTIONS-3.2.md).

La version **3.1.0** ajoute le configurateur natif du **Stream Deck Mini** : six touches LCD, luminosité, noms, couleurs, visuels, commandes Gear Control, scènes, médias et raccourcis clavier. La fiche déjà ajoutée est conservée et l’appareil reste passif tant que sa configuration n’est pas activée. Voir [le guide et les limites](EVOLUTIONS-3.1.md).

La version **3.0.3** ajuste automatiquement les visuels personnalisés : recadrage des marges transparentes ou blanches, redimensionnement proportionnel et centrage sur les cartes, dans l’aperçu et dans le raccourci épinglé. Les originaux restent conservés. Voir [les détails de la version 3.0.3](EVOLUTIONS-3.0.3.md).

La version **3.0.2** conserve la fiche INZONE entre veille et reconnexion, regroupe les interfaces Windows appartenant au même appareil et retire les anciennes fiches de découverte en double. Les sélections suivent le thème du site, les champs de visuel sont espacés et le menu **Affichage** indique les appareils hors de la zone visible. Les explications de compatibilité restent dans la gestion des périphériques. Voir [les détails de la version 3.0.2](EVOLUTIONS-3.0.2.md).

La version **3.0.1** corrige le retrait des appareils et l’alignement des boutons. **Affichage**, au-dessus de la grille, permet de masquer une carte en conservant les commandes de l’appareil. **Personnaliser**, dans la gestion des périphériques, et la configuration de l’action épinglée acceptent un PNG ou un lien image, avec aperçu et retour au visuel automatique. Voir [les détails de la version 3.0.1](EVOLUTIONS-3.0.1.md).

La version **3.0** ajoute un inventaire propre à chaque utilisateur, l’assistant initial, la découverte HID en lecture seule, les sorties et entrées audio Windows génériques, les fiches manuelles, le catalogue et le diagnostic exportable. L’emplacement de la barre supérieure devient une **action épinglée configurable** : choisissez l’appareil, la commande, le nom et l’icône. L’ancien raccourci personnel **Son PS5** est migré uniquement depuis une installation existante, sur son entrée audio exacte. Les installations neuves commencent sans action épinglée. Voir [les fonctions, limites et vérifications de la version 3.0](EVOLUTIONS-3.0.md).

La version **2.12** ajoute les **réglages DualSense** sur PC et mobile : couleur et luminosité de la barre, voyants joueur et micro, vibrations avec test court et arrêt, réduction de puissance et quatre modes de gâchettes adaptatives. Son état distingue **En charge**, **Batterie pleine** et **Sur batterie**. Le bouton **Son PS5**, illustré par la console dans les barres supérieures PC et mobile, active ou désactive l’écoute de l’entrée ligne Realtek. Le SC440 et la manette utilisent désormais des photos produit réelles. Voir [les détails et limites](EVOLUTIONS-2.12.md).

La version **2.11** ajoute le **SC440** avec niveau et sourdine Windows pilotables sur mobile, ainsi que la **DualSense / DualSense Edge** en USB ou Bluetooth avec suivi de connexion et batterie lorsque le rapport matériel le permet. Les appareils sans batterie n’affichent aucune mesure ni avertissement de batterie sur mobile. L’écran **PC → Raccourcis** permet de lancer **FACEIT**, **Spotify** et la **page de matchmaking FACEIT** sur le PC. Si seul l’Anti-Cheat est installé, le bouton indique explicitement **FACEIT Anti-Cheat**. Voir [les détails et limites](EVOLUTIONS-2.11.md).

La version 2.10.3 ajoute le **scan QR en direct sur iOS**, via un [scanner HTTPS sans compte](https://gear-control-scanner.maximemeyre.chatgpt.site) ouvert depuis Gear Control. La caméra et le décodage restent sur le téléphone. Le PC affiche aussi le lien personnel complet, sélectionnable et copiable, avec un QR agrandi. Internet est nécessaire pour charger le scanner ; les commandes du PC restent locales.

La version 2.10.2 limite le **volume par application à la sortie audio actuelle** : les sessions des autres sorties restent masquées, y compris avec « Voir toutes les applications ». Le panneau Spotify présente une configuration guidée et l’avertissement Premium applicable à la connexion API. L’égaliseur adopte des commandes illustrées et une comparaison avant/après plus lisible ; le spectre en direct est retiré.

La version 2.10 propose une **interface mobile inspirée d’iOS** : navigation en bas, accueil illustré, cartes avec les photos de vos appareils et écrans Audio, Spotify, Apparence et PC. Le volume par application privilégie les applications actives et les lecteurs reconnus, avec leurs logos locaux ; **Voir toutes les applications** retrouve les sessions masquées. L’écran **PC** ajoute arrêt, redémarrage, veille, verrouillage, déconnexion, capture d’écran et raccourcis vers l’Explorateur, Steam et Discord. Les actions qui interrompent la session demandent une confirmation. Voir [les nouveautés et leur utilisation](EVOLUTIONS-2.10.md).

La version 2.7 propose des **profils de jeu**, un éditeur visuel du clavier et un aperçu animé du switch. Les derniers réglages connus sont conservés pour enregistrer un périphérique déconnecté dans un profil. Voir [les nouveautés](EVOLUTIONS-2.7.md).

La version 2.6.6 corrige la validation du Client ID Spotify : un identifiant valide de 32 caractères est désormais accepté par le formulaire du navigateur.

La version 2.6.5 modernise le menu de notification Windows et y affiche la version installée. Le logo du site est animé et suit l’accent choisi, tout comme le favicon. La recherche est placée à gauche de la barre supérieure ; Scènes précède le lecteur, qui reste voisin de la sortie audio.

La version 2.6.4 remplace les manipulations de Spotify par son **API officielle**. Connectez votre compte dans **Réglages généraux → Spotify** pour une vérification des Titres likés au chargement du titre, puis après un ajout. Aucun menu ni fenêtre Spotify n’est ouvert. Voir [la connexion et les limites](SPOTIFY.md).

La version 2.6.3 corrige l’association de la télécommande au premier chargement et maintient les commandes disponibles si seul le flux temps réel est interrompu. Les préréglages DPI sont alignés sur leur champ de saisie. Un bouton **Scènes** apparaît dans la barre supérieure dès qu’une scène est enregistrée. Les favoris Spotify sont lus dans les **Titres likés**, et leur état confirmé est conservé entre les mises à jour du lecteur.

La version 2.6.2 ajoute **Réglages généraux → Apparence** : thème sombre classique ou **Sombre OLED** à fond noir pur, et cinq couleurs d’accentuation (vert, bleu, violet, rose, orange). Le choix est appliqué immédiatement et mémorisé par navigateur ; la télécommande mobile propose aussi **Apparence**. Dans le lecteur Spotify et la barre supérieure, le cœur ajoute le morceau courant aux **Titres likés**, après connexion à l’API Spotify depuis les réglages généraux. L’ajout est confirmé par Spotify et ne retire jamais un morceau déjà enregistré. Voir [les limites des favoris](windows-media.md).

La version 2.6 regroupe le mélangeur audio, les scènes, les automatisations et la télécommande dans **Réglages généraux**. Le volume par application est fermé par défaut. Les règles Windows utilisent une liste des processus actuellement lancés, avec recherche et actualisation. Les visuels sont cadrés sur le produit visible et accompagnent aussi la sélection des périphériques dans les scènes. Recherche, média et sortie audio ont la même hauteur dans la barre supérieure. Voir [les nouveautés de l’interface](EVOLUTIONS-2.6.md).

Depuis la version 2.6.1, ouvrez le lecteur depuis la barre supérieure puis cliquez ou glissez sur sa progression pour avancer ou reculer dans le morceau. Les flèches du clavier fonctionnent aussi ; Échap annule un déplacement préparé. La commande est disponible lorsque le lecteur Windows l’autorise, sur PC et dans la télécommande mobile.

La version 2.4 ajoute la sélection multiple du clavier, les profils, l’édition rapide, Ctrl+K, la comparaison EQ et son analyseur, le mélangeur Windows et les scènes locales. Consultez [les nouveautés et leur utilisation](EVOLUTIONS-2.4.md).

La version 2.5 ajoute **Média en cours** dans la barre supérieure du PC et la télécommande mobile : application active, titre, artiste, pochette et commandes précédent, lecture/pause et suivant. Dans la barre, utilisez lecture/pause directement ou cliquez sur le titre pour ouvrir le lecteur complet. Spotify et les autres lecteurs qui publient une session média Windows sont détectés automatiquement ; aucun compte Spotify n’est nécessaire. Si plusieurs lecteurs sont ouverts, choisissez la session à contrôler. Les changements de morceau et de lecture arrivent par les événements Windows. Voir [le fonctionnement et les limites](windows-media.md).

Cliquez sur une carte pour sélectionner le périphérique. **Actualiser** relit son état. Le menu de sortie audio, en haut à droite, change la sortie Windows pour le son et les communications. Il permet aussi de régler le volume Windows de la sortie principale avec le curseur, les boutons −/+ par pas de 5 % et le bouton de sourdine. À l’ouverture, le hub sélectionne le périphérique correspondant à la sortie audio principale quand il peut l’identifier.

Pour organiser **Mes périphériques** sur PC, glissez la poignée à droite d’une carte jusqu’à sa nouvelle place. Les cartes s’écartent avec une animation et un emplacement indique où la déposer. **Échap** annule le déplacement. Au clavier, placez le focus sur la poignée, appuyez sur **Espace**, utilisez les **flèches**, puis **Espace** ou **Entrée** pour valider. L’ordre est repris dans la barre latérale et conservé dans ce navigateur à la même adresse, y compris pour les périphériques indisponibles. Réorganiser la liste dans **Égaliseur** conserve la place des appareils masqués.

Les appareils indisponibles sont grisés et restent sélectionnables pour consulter leurs informations. La liste distingue **Filaire · USB**, **Sans fil · 2,4 GHz** et **Sans fil · LIGHTSPEED** : un récepteur branché en USB relie ses écouteurs ou sa souris sans fil.

Dans **Vue d’ensemble**, consultez les batteries. L’éclair indique **En charge** uniquement si le matériel transmet cet état. Le libellé distingue le boîtier des écouteurs, y compris sur les cartes. Un tiret signifie que la mesure est indisponible. Le niveau et l’état de charge du boîtier sont sa dernière mesure relayée par un écouteur ; ils ne se mettent pas nécessairement à jour lorsque le boîtier est isolé. Si le boîtier Sony transmet un état inconnu, le hub ne le déduit pas du branchement USB.

Dans la **Vue d’ensemble**, choisissez un appareil : le module **Paramètres rapides** applique immédiatement vos réglages épinglés, et **Ouvrir les réglages** descend vers tous les réglages de l’appareil. Préparez-y les changements, puis cliquez sur **Appliquer**. Attendez la confirmation après relecture du périphérique. **Annuler** abandonne les changements en attente. Pour donner un nom à une paire ou à un appareil, utilisez **Nom du périphérique → Enregistrer**.

## Utiliser le téléphone comme télécommande

1. Laissez Gear Control actif sur le PC et connectez le téléphone au **même réseau local** : le PC peut être en Ethernet et le téléphone en Wi-Fi sur la même box.
2. Sur le PC, ouvrez **Réglages généraux → Télécommande mobile**. Sur le téléphone, utilisez **Scanner le QR code** depuis l’écran de connexion ou **Plus → PC enregistré**, puis **Scanner en direct**. La page HTTPS ouvre la caméra après votre autorisation et détecte le code automatiquement. Gear Control enregistre ensuite la connexion. En secours, collez le lien complet affiché sur le PC, choisissez une image existante ou scannez avec l’appareil photo iOS.
3. Naviguez avec les onglets **Accueil**, **Audio**, **Spotify**, **Apparence** et **Plus**. L’accueil affiche les photos et les batteries de tous vos périphériques. Touchez une carte pour accéder aux commandes disponibles : volume, micro et contrôle du bruit des écouteurs, DPI et fréquence USB de la souris, verrou Windows et échange WASD/flèches du clavier. L’écran **Audio** regroupe sortie Windows, volume général et volume par application ; toucher le logo d’une application coupe ou rétablit son son. Les changements sont appliqués directement, puis relus sur le PC.
4. Ajoutez la page à l’écran d’accueil du téléphone pour la retrouver facilement. Le PC doit rester allumé et Gear Control doit fonctionner.

Depuis l’accueil, touchez **PC connecté** ou **Power** pour les commandes Windows. Une confirmation précède l’arrêt, le redémarrage, la veille, le verrouillage et la fermeture de session. **Capture écran** affiche une image du bureau à enregistrer sur le téléphone. Les raccourcis Steam et Discord sont disponibles si leurs protocoles sont enregistrés sur le PC. Après un arrêt ou une veille, rallumez ou réveillez le PC pour retrouver la télécommande.

Le lien personnel autorise la télécommande sur ce PC. Depuis la version 2.10.1, la connexion est enregistrée dans le téléphone et dans un cookie persistant, pour fonctionner aussi après l’ajout à l’écran d’accueil iOS. Pendant un redémarrage du PC ou de Gear Control, le téléphone conserve son association et réessaie automatiquement. Une seule association suffit tant que les données du site et l’adresse du PC sont conservées. Si la box attribue une nouvelle IP, scannez le nouveau QR code ; le panneau sur le PC affiche le lien à jour.

Lors de l’installation, l’option **Autoriser la télécommande sur le réseau local** configure une règle de pare-feu limitée au service et au sous-réseau local ; Windows demande une confirmation administrateur pour cette option. Sans elle, le service n’écoute que ce PC : activez la télécommande plus tard dans **Réglages généraux → Télécommande mobile**. Le bouton **Révoquer les téléphones** du même écran déconnecte tous les téléphones associés. Pour une installation existante, lancez le script **Enable-RemoteAccess.ps1** situé dans le dossier du logiciel avec PowerShell en tant qu’administrateur. Les réseaux Wi-Fi invités ou avec isolation entre appareils peuvent empêcher la connexion.

| Périphérique | Réglages disponibles |
| --- | --- |
| G FITS blancs et noirs | Batteries gauche/droite/boîtier, charge par composant quand transmise, retour vocal, gestes simples et doubles, volume et niveau du micro Windows propres à chaque paire |
| Sony INZONE Buds | Trois batteries, volume, balance jeu/conversation, retour vocal, coupure du micro, réduction du bruit et son ambiant, paramètres vocaux et profils tactiles ; égaliseur à dix bandes via le pilote Sony |
| Creative Pebble Pro | Sélection comme sortie Windows, volume et sourdine de leur sortie audio ; bouton physique et éclairage réglables sur les enceintes |
| SC440 | Niveau et sourdine de son entrée Windows, accessibles sur PC et mobile ; aucune batterie |
| DualSense / DualSense Edge | Connexion USB/Bluetooth, batterie et charge ; couleur, luminosité et voyants, test de vibrations, puissance et gâchettes adaptatives ; remappage géré dans les jeux/Steam |
| MAD68 HE Lightless | Sélection d’une touche sur le clavier visuel, point d’activation, Rapid Trigger et distances de réactivation/relâchement, verrou Windows ; lecture du NKRO et du firmware |
| ATK A9 Ultra | Batterie, DPI, fréquence USB, modes de performance, synchronisation, correction de ligne, LOD, veille et rotation ; longue distance et éclairage sur récepteur compatible |

**Sony : changer le mode sonore.** Portez les deux écouteurs, sélectionnez leur carte, puis cliquez sur **Réduction du bruit**, **Son ambiant** ou **Désactivé** dans la vue d’ensemble. Ces boutons appliquent le mode immédiatement et vérifient sa confirmation ; ils ne nécessitent pas le bouton Appliquer des réglages.

**Clavier : régler des touches.** Ouvrez Réglages et sélectionnez les touches souhaitées par clic, Ctrl/Maj + clic ou rectangle. Les valeurs différentes restent visibles ; un changement commun ne modifie que le champ choisi. Préparez vos modifications puis cliquez sur **Appliquer**. Ctrl+Z, l’historique, les profils, copier/coller et la carte thermique complètent le configurateur.

**Souris : DPI et fréquence USB.** Une seule carte suit la souris entre le câble USB et le récepteur 2,4 GHz ; son type de connexion change automatiquement. Ouvrez Réglages, saisissez une valeur DPI, utilisez les boutons −/+ ou un préréglage, puis choisissez la fréquence USB. Cliquez sur **Appliquer**. La valeur affichée après confirmation est celle relue sur la souris.

## Régler et enregistrer l’égaliseur

1. Ouvrez **Égaliseur** et sélectionnez les Sony. Cette page affiche les écouteurs dont l’EQ est pris en charge, avec la cause si l’EQ est temporairement indisponible ; l’EQ matériel G FITS reste indisponible. Quittez INZONE Hub depuis son icône près de l’horloge et désactivez son lancement automatique avec Windows pour éviter un conflit après redémarrage.
2. Choisissez **Neutre**, **Musique**, **Basses**, **Voix** ou **Jeu**, ou ajustez les dix bandes de −12 à +12 dB. Vérifiez que l’EQ est activé.
3. Cliquez sur **Appliquer** pour envoyer le réglage au traitement audio de la sortie **INZONE Buds Game**.
4. Pour conserver ce réglage comme profil personnel, donnez-lui un nom et cliquez sur **Enregistrer**. Vous pouvez ensuite le sélectionner, le modifier, le renommer ou le supprimer. **Mettre à jour** enregistre les changements d’un profil existant.

**Enregistrer un profil et Appliquer sont deux actions distinctes.** Le profil est conservé dans ce navigateur ; le sélectionner prépare un réglage qu’il faut ensuite appliquer. Conservez le même navigateur et l’adresse exacte `http://127.0.0.1:3210` pour retrouver vos profils. Effacer les données du site les supprime.

L’EQ Sony nécessite les effets audio Windows sur la sortie Game à 48 kHz. Un lecteur utilisant le mode exclusif ou RAW peut contourner ce traitement.

## Fonctions complémentaires et dépannage

Pour les macros ou le remappage du clavier et de la souris, ouvrez **Réglages → Configuration avancée → Ouvrir l’outil**. Le site officiel s’affiche dans le hub et reçoit temporairement l’accès USB. Cette partie nécessite Internet et Chrome ou Edge avec l’autorisation WebHID. Si le cadre refuse la connexion, choisissez **Ouvrir en onglet**. Revenez ensuite dans la vue d’ensemble pour reprendre l’accès direct.

| Problème | Action |
| --- | --- |
| La page ne s’ouvre pas | Lancez Gear Control depuis le menu Démarrer. Dans son icône près de l’horloge, choisissez **Relancer le service**, puis rechargez la page. |
| Un appareil est indisponible | Vérifiez son récepteur, son mode LIGHTSPEED/PC ou son câble, puis cliquez sur **Actualiser**. Fermez l’outil officiel s’il utilise encore l’USB. |
| Une batterie affiche un tiret ou aucun éclair | La mesure ou l’état de charge n’est pas transmis. Cela ne signifie ni 0 % ni une panne de batterie. |
| Le mode sonore Sony est refusé | Portez les deux écouteurs, vérifiez le mode PC, puis réessayez. |
| L’EQ Sony est indisponible ou inaudible | Si INZONE Hub est ouvert, quittez-le depuis son icône près de l’horloge et désactivez son lancement automatique avec Windows, puis actualisez. Vérifiez la présence du pilote Sony, sélectionnez **INZONE Buds Game**, activez les effets audio Windows et utilisez 48 kHz. Quittez le mode exclusif/RAW du lecteur si nécessaire. |
| Un réglage échoue ou un bouton est grisé | Lisez le message affiché, vérifiez la connexion et les limites des valeurs, puis actualisez. Le hub ne confirme pas une écriture que le matériel n’a pas confirmée. |
| Le problème persiste | Utilisez **Ouvrir les journaux** dans le menu Gear Control pour consulter le diagnostic du service. |

L’application n’effectue aucune mise à jour du firmware, aucun appairage, aucune réinitialisation et aucun moulage des G FITS. Le MAD68 connecté est la variante sans RGB. L’égaliseur G FITS et le son spatial Sony ne sont pas configurables dans le hub.

Pour les chemins des fichiers, la sauvegarde, les mises à jour et le développement, voir [Installation et maintenance](INSTALLATION.md). Les recherches d’interopérabilité s’appuient notamment sur [REA de morluto](https://github.com/morluto/rea) ; les sources, essais et limites sont documentés dans [G FITS](GFITS-PROTOCOL.md), [Sony](SONY-PROTOCOL.md), [clavier/souris](INPUT-PROTOCOL.md) et [audio Windows](WINDOWS-AUDIO.md). Gear Control est une application indépendante des fabricants.
