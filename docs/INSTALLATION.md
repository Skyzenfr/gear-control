# Installation et maintenance de Gear Control

Le [guide utilisateur](../README.md) décrit les parcours batterie, ANC, égaliseur, clavier et souris. Cette page précise l’installation Windows, les données locales et le travail depuis les sources.

## Installation Windows

L’installateur `release/GearControl-Setup-<version>.exe` cible **Windows 10/11 x64**. Par défaut, il installe l’application pour l’utilisateur courant dans le dossier ci-dessous. L’étape **Emplacement** permet de choisir un autre dossier, y compris lors d’une mise à jour :

```text
%LOCALAPPDATA%\Programs\GearControl
```

Choisissez un dossier où votre compte peut écrire, sans droits administrateur. Si vous déplacez une installation existante, l’ancien dossier n’est pas supprimé automatiquement.

Il inclut Node.js portable, l’interface compilée, `node-hid` et les helpers audio Windows. Il ne nécessite pas de Node.js global ni de droits administrateur. Aucune DLL fabricant n’est distribuée avec le service. L’installateur n’a pas de signature commerciale ; un avertissement Windows SmartScreen est possible.

Les fonctions USB et audio utilisent les interfaces Windows existantes. L’EQ des Sony INZONE Buds nécessite **Windows 11** et le pilote APO Sony : sur un Windows neuf, installez INZONE Hub une fois, puis fermez son application et son agent. Le hub utilise ensuite ce pilote pour la sortie **INZONE Buds Game**, sans lancer INZONE Hub. Les batteries et les commandes ANC ne nécessitent pas ce pilote EQ. La vérification du traitement utilise [IAudioEffectsManager](https://learn.microsoft.com/en-us/windows/win32/api/audioclient/nn-audioclient-iaudioeffectsmanager), disponible à partir du build Windows 22000.

## Lancement, arrêt et démarrage automatique

**GearControl.exe** apparaît dans la zone de notification de Windows. Il lance le service Node portable et ses helpers en arrière-plan, sans fenêtre de console, puis surveille le service pour le relancer après un arrêt inattendu.

Son menu propose :

- **Ouvrir le hub** : ouvre `http://127.0.0.1:3210` dans le navigateur.
- **Relancer le service** : redémarre le service local et ses helpers.
- **Ouvrir les journaux** : ouvre le dossier de diagnostic.
- **Démarrer avec Windows** : active ou désactive le lancement à l’ouverture de la session.
- **Quitter** : arrête le lanceur, le service et ses helpers. Un démarrage automatique encore activé s’appliquera à la prochaine ouverture de session.

Le démarrage automatique, activé par défaut, est une entrée du compte utilisateur dans `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`. Il ne crée pas de service Windows nécessitant des droits administrateur. Fermer le navigateur n’arrête pas le service. Gear Control ne lance pas les applications fabricant.

## Données, profils et mises à jour

Depuis la version 3.0, l’assistant du premier lancement configure un inventaire propre à l’utilisateur. **Réglages généraux → Périphériques** permet ensuite de rechercher et d’ajouter de nouveaux appareils, de préparer des fiches, de consulter le catalogue et d’exporter un diagnostic. Les appareils inconnus restent limités à leur identification et aux fonctions Windows disponibles.

Les données du service et les journaux sont enregistrés dans :

```text
%LOCALAPPDATA%\GearControl\data
```

**Ouvrir les journaux** ouvre ce dossier dans l’Explorateur Windows. `service.log` contient les journaux du service ; une copie `.previous` est conservée lors de la rotation à 1 Mo. `supervisor.json` fournit le diagnostic du lanceur.

`mouse-connections.json` conserve les associations radio réellement lues pour reconnaître le récepteur quand il ne répond plus pendant une connexion filaire. L’identité de la souris est dérivée de son identifiant radio et reste identique entre les deux modes. Les noms enregistrés avant ce regroupement sont repris.

Avant une opération de maintenance, choisissez **Quitter**, puis copiez le dossier de données si vous voulez en conserver une sauvegarde. Ce dossier contient les données du service, pas les profils EQ du navigateur.

Les noms personnalisés sont associés à l’identité du périphérique. Les profils EQ personnels sont conservés dans le stockage local du navigateur, par appareil et fréquences. Ils restent disponibles sur ce PC tant que vous utilisez le même profil de navigateur, l’adresse exacte `http://127.0.0.1:3210` et que vous ne supprimez pas les données du site. Une fenêtre privée, un autre navigateur ou `localhost` à la place de `127.0.0.1` utilise un autre stockage.

L’ordre des cartes est conservé par identité dans `device-inventory.json`, avec un cache de secours dans le navigateur sous `gear-control:device-order`. L’ancien ordre du navigateur est migré au premier chargement de la version 3.0. Les reconnexions et actualisations ne le changent pas ; les nouveaux appareils ajoutés suivent les précédents. `pinned-action.json` conserve l’action épinglée configurable. Ces fichiers restent dans les données de l’utilisateur et ne sont pas intégrés à l’installateur à partager.

La couleur d’accentuation est également mémorisée dans ce navigateur ; l’interface utilise un thème sombre OLED unique. Le PC et le téléphone peuvent chacun avoir leur couleur. Si le stockage est bloqué, le choix reste appliqué pendant la session et l’interface indique qu’il n’a pas pu être enregistré.

Depuis la version 3.2.9, Gear Control propose lui-même les nouvelles versions : voir **Réglages généraux › Mises à jour**. `updates.json` mémorise vos choix (recherche automatique, version ignorée), et l’installateur téléchargé est placé temporairement dans le sous-dossier `updates`, supprimé au démarrage suivant. Le journal de la dernière installation automatique est `update-install.log`.

Pour installer une nouvelle version à la main, lancez le nouvel installateur pour le même compte Windows. Il arrête la version précédente avant de la remplacer, puis relance le hub. Une désactivation du démarrage depuis l’icône est conservée lors d’une mise à jour ; vous pouvez la modifier dans l’assistant. Le port et l’adresse restent identiques. La désinstallation se fait depuis **Paramètres Windows → Applications → Gear Control** ; elle conserve le dossier de données. Supprimez-le manuellement si vous souhaitez également effacer les données du service.

### Télécommande sur le réseau local

Depuis la version 3.2.26, la télécommande s’active dans **Réglages généraux → Télécommande mobile**. Désactivée, le service n’écoute que `127.0.0.1` : Windows n’affiche plus d’alerte de pare-feu au premier lancement. Une nouvelle installation reste désactivée, sauf si l’option **Activer la télécommande** de l’installateur a réussi. Une installation existante garde la télécommande active. Le choix est enregistré dans `remote-access.json`. Le bouton **Révoquer les téléphones** crée une nouvelle clé : tous les téléphones associés doivent scanner le nouveau QR code.

Une fois activée, le service écoute aussi sur les adresses IPv4 du PC, au port 3210. Les requêtes du réseau doivent provenir d’une adresse privée, utiliser une adresse locale du PC et présenter le jeton personnel fourni par **Réglages → Télécommande mobile**. Le jeton est conservé dans `remote-token` dans le dossier de données du service. Le lien d’association le transporte dans un fragment `#token=…`, que le navigateur mémorise puis retire de la barre d’adresse. La télécommande permet les réglages audio essentiels, les DPI et la fréquence USB de la souris, les options globales du clavier et les commandes du PC. Les réglages par touche, l’égaliseur et les autres fonctions avancées restent accessibles depuis le PC. Les commandes PC utilisent une liste fixe d’actions Windows, avec confirmation explicite pour celles qui interrompent la session ; aucune commande shell ni chemin fourni par le téléphone n’est accepté.

Le script `Enable-RemoteAccess.ps1` installé à côté de `GearControl.exe` configure le pare-feu Windows avec les droits administrateur. Il autorise uniquement le moteur Node installé, en TCP sur le port 3210, depuis le sous-réseau local et sur les interfaces actives reliées à une passerelle. Il conserve la catégorie du réseau Windows. Il supprime aussi les règles « Query User » créées pour ce moteur par l’alerte du pare-feu Windows : une règle de blocage annulerait l’autorisation, une règle d’autorisation ouvrirait le port au-delà du sous-réseau local. Relancez ce script si vous changez d’interface réseau. Aucun port de la box n’est à ouvrir ni à rediriger. La désinstallation retire la règle, avec une confirmation administrateur.

## Depuis les sources

Le projet utilise **Svelte 5, TypeScript et Vite** pour l’interface, **Node.js** pour l’API et **USB HID** pour les périphériques. Le petit service local est nécessaire pour accéder aux interfaces matérielles et audio Windows depuis la page web. Aucun hébergement distant n’est requis.

Pour développer, utilisez une version Node compatible avec le projet, par exemple Node 24, puis depuis `app/` :

```powershell
npm.cmd ci
npm.cmd run dev
```

Pour vérifier et compiler :

```powershell
npm.cmd run check
npm.cmd test
npm.cmd run build
```

`dev` et `start` servent l’interface et l’API sur `127.0.0.1:3210`. Pour lancer la version compilée depuis les sources, utilisez **Start-Hub.cmd** à la racine du projet ; **Stop-Hub.cmd** arrête ce service et ses helpers. Les scripts PowerShell correspondants sont `Start-Hub.ps1` et `Stop-Hub.ps1`. Ces lanceurs de sources sont distincts du lanceur Windows installé.

Évitez de lancer simultanément le service de développement et la version installée : ils utilisent le même port et les mêmes interfaces USB. L’API vérifie l’hôte, l’origine et l’identité du périphérique avant une écriture ; elle n’expose pas d’exécution arbitraire de commandes shell. Le paramètre `GFITS_PORT` permet un autre port hors du lanceur installé, mais change également l’origine du stockage des profils dans le navigateur.

## Reconstruire l’installateur

Le plus simple est de double-cliquer sur **Build-Installer.cmd** à la racine du projet. Depuis un terminal :

```powershell
.\Build-Installer.ps1            # construit la version actuelle de app/package.json
.\Build-Installer.ps1 -Bump      # 3.2.8 -> 3.2.9, puis construit
.\Build-Installer.ps1 -Version 3.3.0 -Open
```

| Option | Effet |
| --- | --- |
| `-Bump` | Incrémente le numéro de correctif dans `package.json` et `package-lock.json`. |
| `-Version x.y.z` | Fixe un numéro de version précis. |
| `-Force` | Remplace sans confirmation un installateur déjà présent pour cette version. |
| `-Open` | Sélectionne l’installateur dans l’Explorateur à la fin (toujours actif avec le `.cmd`). |
| `-CompilerPath` | Utilise un `ISCC.exe` déjà installé. |

`Build-Installer.ps1` copie le projet dans `%LOCALAPPDATA%\GearControl-Build\workspace` et y lance `Build-HubInstaller.ps1` (installation propre des dépendances, `svelte-check`, tests, compilation de l’interface puis de l’installateur). L’installateur et son fichier `.sha256` sont ensuite copiés dans `release/`, puis le chemin, la taille et l’empreinte SHA-256 sont affichés. Comme `npm ci` s’exécute dans cette copie, la construction fonctionne même si VS Code garde des fichiers de `app/node_modules` ouverts, et le dossier `node_modules` du projet n’est jamais modifié. La copie est conservée entre deux constructions pour accélérer les suivantes ; vous pouvez supprimer ce dossier à tout moment.

Depuis la version 3.3.0, après vérification de l’empreinte SHA-256 du nouvel installateur, le script supprime les installateurs des versions antérieures et leurs fichiers `.sha256` dans `release/` et dans la copie de compilation. Une compilation échouée conserve les anciens fichiers. Les versions plus récentes, les autres fichiers, les outils téléchargés et les dépendances sont conservés.

Utilisez toujours `Build-Installer.ps1` : il appelle `Build-HubInstaller.ps1` dans la copie isolée. Ne lancez pas ce dernier directement dans le dépôt.

Le script télécharge les composants de construction nécessaires, notamment Node portable et Inno Setup, avec vérification des téléchargements. L’installateur résultant est placé dans `release/`. La construction nécessite Internet ; le fonctionnement quotidien du hub reste local, à l’exception des outils officiels intégrés pour la configuration avancée et de la recherche de mises à jour, qui lit un seul fichier sur GitHub et peut être désactivée dans **Réglages généraux › Mises à jour**.

Les visuels de l’assistant (fonds de pages, boutons, interrupteurs) sont décrits dans `packaging/installer-art/art.html` et enregistrés en PNG dans `packaging/assets/installer/`. Après une modification de ce fichier, régénérez-les avec Microsoft Edge :

```powershell
node packaging/render-installer-art.mjs
```

Les positions des contrôles placés par `GearControl.iss` correspondent aux coordonnées de ce fichier (zone de 745 × 490 pixels). Une version d’essai qui n’enregistre rien et n’arrête pas le service peut être compilée avec `ISCC.exe /DPreview`.

## Publier une mise à jour

Les logiciels installés lisent `https://github.com/<owner>/<repo>/releases/latest/download/latest.json`. Le dépôt et la clé publique sont définis dans `app/server/update-source.json`.

**Une seule fois :**

1. Créez le dépôt sur GitHub, en **public**, avec un README. Il peut ne contenir que les releases ; le code source peut rester privé ailleurs.
2. Créez la clé de signature : `node packaging/release-signing.mjs keygen`. La clé privée est enregistrée dans `%APPDATA%\GearControl-Release\update-signing-key.pem` et la clé publique est ajoutée à `update-source.json`. **Sauvegardez la clé privée** (gestionnaire de mots de passe, clé USB) : sans elle, les versions déjà installées ne pourront plus recevoir de mise à jour automatique. Il faudrait alors les réinstaller à la main.
3. Créez un jeton GitHub sur <https://github.com/settings/personal-access-tokens/new> : accès au dépôt uniquement, permission **Contents : Read and write**. Il est demandé à la première publication puis enregistré chiffré pour votre session Windows. `Publish-Update.ps1 -ForgetToken` l’efface.

**À chaque version :**

```powershell
.\Publish-Update.ps1 -Build -Bump   # nouvelle version, construction puis publication
.\Publish-Update.ps1                # publie l’installateur déjà construit de la version actuelle
.\Publish-Update.ps1 -Draft         # release en brouillon, rien n’est proposé aux utilisateurs
```

Le script vérifie que la version est plus récente que la dernière publiée, signe `latest.json` et crée la release `v<version>` en brouillon. Il y envoie l’installateur, son `.sha256` et `latest.json`, publie la release, puis vérifie le flux public. Les notes affichées dans le logiciel viennent de `docs/EVOLUTIONS-<version>.md` (ou de `-NotesFile`).

Pour tester le flux sans publier, la variable `GEAR_CONTROL_UPDATE_FEED` remplace l’adresse de `latest.json` lue par le service.

## Vérification et documentation technique

Les tests automatiques vérifient les trames, les valeurs inconnues, l’identité des appareils et les modifications confirmées, avec des fixtures ou des adaptateurs simulés. Ils n’écrivent pas sur vos périphériques.

Avec le service démarré, les scripts de diagnostic en lecture peuvent être lancés depuis `app/` :

```powershell
node scripts/verify-http.mjs
node scripts/verify-hardware.mjs
```

Les autres scripts de vérification des contrôles et de l’audio peuvent modifier temporairement le matériel avant restauration ; lisez leur fonctionnement avant de les exécuter. Les échanges et essais réels sont décrits dans les documents suivants, avec leurs limites. Un test de relance du service ne remplace pas un essai de redémarrage complet du PC.

Le paquet 2.1.0 a été installé et mis à jour sur ce PC. La désinstallation puis la réinstallation ont été vérifiées : les données restent présentes, les processus sont arrêtés et l’entrée de démarrage Windows est retirée puis réenregistrée. Une préférence de démarrage désactivée est conservée lors d’une mise à jour.

Le moteur Node embarqué, l’arrêt des helpers, la récupération après crash du service, le nettoyage après crash du lanceur et l’absence de double instance ont été vérifiés. Le script suivant reproduit ces arrêts temporaires et laisse le hub démarré ; il ne redémarre pas Windows :

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\packaging\Verify-Installation.ps1 -Exercise
```

- [G FITS : accès Centurion direct](GFITS-PROTOCOL.md)
- [Sony INZONE Buds : USB et EQ](SONY-PROTOCOL.md)
- [MAD68 HE et ATK A9 Ultra](INPUT-PROTOCOL.md)
- [Volumes et microphones Windows](WINDOWS-AUDIO.md)
- [Choix de la sortie audio Windows](WINDOWS-AUDIO-OUTPUTS.md)
- [Provenance des PNG officiels](DEVICE-IMAGES.md)
- [Ancienne API G HUB](PROTOCOL.md), conservée comme historique de recherche ; le pont n’est plus chargé par le serveur.

[REA](https://github.com/morluto/rea) a servi à analyser les applications et transports installés. Aucun code décompilé ni DLL fabricant n’est intégré au serveur. Les recherches contenant des identifiants matériels restent dans `research/`, exclu de Git ; les fixtures distribuées sont limitées aux données utiles aux tests.
