# Gear Control 2.10 — Télécommande mobile

## Correctif 2.10.3 — QR en direct et lien complet

**Scanner en direct** ouvre un [scanner HTTPS sans compte](https://gear-control-scanner.maximemeyre.chatgpt.site), nécessaire à l’accès caméra sur iOS depuis une adresse locale HTTP. Après autorisation, la caméra arrière détecte les codes en continu. Une lecture valide arrête la caméra, renvoie vers le PC et enregistre la connexion. Aucun cliché n’est requis ; l’import d’une image et la saisie du lien restent disponibles en secours.

La page traite les images et le QR uniquement sur le téléphone. Elle ne contacte aucune API du PC et ne stocke pas sa clé. Le retour vers une fenêtre Gear Control existante vérifie son origine, la fenêtre et un nonce ; un changement d’adresse du PC utilise une navigation directe vers le nouveau lien. Internet est nécessaire pour charger cette page. Les commandes restent sur le réseau local.

Sur le PC, le QR est agrandi et le lien complet, y compris `#token`, reste visible, sélectionnable et copiable. Il reste accessible si la génération du QR échoue. Si la copie automatique est refusée, le lien est sélectionné pour une copie manuelle.

Safari et une application déjà ajoutée à l’écran d’accueil peuvent utiliser des stockages distincts. Lorsque Safari ouvre la connexion au lieu de l’application installée, ajoutez la page connectée à l’écran d’accueil pour conserver cette association dans la nouvelle application.

## Correctif 2.10.2 — Sortie audio et interfaces

Le mélangeur affiche uniquement les applications de la sortie Windows actuelle, déterminée par le rôle principal « console ». Le bouton **Voir toutes les applications** retrouve les sessions secondaires de cette même sortie. Les applications associées à d’autres écouteurs ou enceintes n’apparaissent plus dans cette liste. Un changement de sortie actualise le mélangeur et annule le glissement d’un curseur en cours. La sortie est vérifiée avant chaque écriture, puis à nouveau dans le composant Windows au moment du réglage.

Le panneau **Connexion Spotify** regroupe le statut du compte, l’association et une configuration guidée, avec copie de l’adresse de redirection. L’avertissement Premium est visible dès l’ouverture et précise qu’il concerne le propriétaire de l’application Spotify en mode développement. Les commandes multimédias Windows conservent leur fonctionnement indépendant de cette connexion API.

Le **spectre en direct est retiré de l’égaliseur**. L’interface organise les profils, la courbe, les dix bandes et la comparaison avant/après dans des zones plus lisibles, avec des icônes sur les actions courantes. Les réglages préparés, l’historique et l’application confirmée sur le périphérique sont conservés.

## Correctif 2.10.1 — Connexion iOS mémorisée

La connexion est enregistrée avec le nom et l’adresse du PC, puis complétée par un cookie persistant HttpOnly et SameSite=Strict. Au lancement, la télécommande vérifie cette session avant de charger ses commandes, même si son stockage JavaScript est vide. Cela permet de retrouver l’association dans une application ajoutée à l’écran d’accueil iOS : [WebKit précise que les cookies sont copiés lors de l’installation, contrairement au stockage local](https://webkit.org/blog/14787/webkit-features-in-safari-17-2/).

Les interruptions réseau et les redémarrages du service conservent l’association. Un nouvel essai est effectué automatiquement toutes les cinq secondes sans flux actif, ainsi qu’au retour du réseau ou de l’application au premier plan. Une réponse d’une ancienne association ne peut pas remplacer une connexion plus récente.

**Scanner le QR code** est disponible sur l’écran de connexion et dans **Plus → PC enregistré**. Depuis la version 2.10.3, le bouton principal lance la caméra en direct via HTTPS. Il est aussi possible de choisir une image ou de coller le lien. Les images restent sur le téléphone.

Après un changement d’adresse IP du PC ou un effacement des données du site, utilisez à nouveau le QR code. Les autorisations de télécommande et le contrôle des origines restent appliqués aux sessions enregistrées.

## Interface mobile

L’interface mobile adopte une navigation inspirée d’iOS : barre fixe en bas, espaces adaptés aux zones de sécurité de l’iPhone, cartes arrondies et commandes tactiles. L’accueil présente les périphériques avec leurs photos, y compris les appareils déconnectés. Le violet est proposé à la première ouverture ; les préférences d’apparence déjà enregistrées sont conservées.

- **Accueil** : état du PC, accès rapides, batteries et profils enregistrés.
- **Audio** : sortie Windows, volume général, sourdine et mélangeur par application.
- **Spotify** : lecteur média Windows existant, pochette lorsqu’elle est transmise, progression et commandes selon les capacités du lecteur.
- **Apparence** : thèmes sombre et OLED, cinq accents et aperçu de la navigation.
- **Plus** : accès au PC, profils et aide pour ajouter l’application à l’écran d’accueil.

Dans Safari, ouvrez le menu **Partager → Sur l’écran d’accueil** pour lancer la télécommande dans une fenêtre autonome. Elle reste une application web locale : le PC doit être disponible sur le même réseau.

## Contrôle du PC

Touchez **PC connecté** ou **Power** depuis l’accueil. L’écran propose arrêt, redémarrage, veille, verrouillage et fermeture de session, chacun après confirmation. Les requêtes passent par l’association existante du téléphone. La mise hors tension et la veille interrompent la connexion ; aucun réveil réseau n’est ajouté.

La capture d’écran fournit un aperçu PNG limité à 1 600 pixels sur son plus grand côté, avec un lien pour l’enregistrer sur le téléphone. Elle est transmise à la demande, sans fichier conservé par le service. Les raccourcis ouvrent l’Explorateur, Steam ou Discord sur le PC ; les deux derniers sont grisés si leur protocole Windows n’est pas enregistré.

## Applications audio

Le mélangeur masque par défaut les processus techniques et les applications inconnues inactives. Il trie d’abord les sessions actives, puis les applications reconnues, puis les noms. **Voir toutes les applications** permet de retrouver chaque session masquée. Des logos intégrés au logiciel identifient les applications courantes ; les autres reçoivent une icône ou une initiale.

Les sessions d’une même application sur plusieurs sorties restent distinctes. Le volume est envoyé à l’identifiant de session et à sa sortie Windows. Touchez le logo pour couper ou rétablir le son. Le déplacement du curseur affiche la valeur préparée et envoie le réglage au relâchement, puis vérifie la réponse de Windows.

## Vérification

Les tests couvrent l’association initiale, la navigation, les périphériques déconnectés, le tri des applications, les sessions audio sur plusieurs sorties et les confirmations des commandes PC. Les actions d’alimentation sont testées avec un exécuteur simulé. L’aperçu `node scripts/preview-mobile.mjs`, depuis `app/`, fournit des données simulées et refuse toutes les écritures sans charger les services matériels.
