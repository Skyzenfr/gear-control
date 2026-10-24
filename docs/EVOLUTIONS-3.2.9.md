# Gear Control 3.2.9 — mises à jour automatiques

## Mises à jour intégrées

Gear Control recherche lui-même les nouvelles versions et propose de les installer.

- **Détection :** au démarrage, puis toutes les 6 heures. Le bouton **Rechercher** de **Réglages généraux › Mises à jour** lance une vérification immédiate. La recherche automatique peut être désactivée au même endroit.
- **Annonce :** une carte « Version x.y.z » apparaît en bas de la barre latérale, et une notification Windows s’affiche une fois par version. Un clic ouvre l’onglet **Mises à jour** avec les nouveautés de la version.
- **Installation :** **Installer la mise à jour** télécharge l’installateur (avec la progression et un bouton **Annuler**), le vérifie, puis lance l’installation. Gear Control se ferme, se met à jour et redémarre seul ; la page se recharge sur la nouvelle version, qui confirme « Mis à jour en x.y.z ».
- **Ignorer cette version :** la version n’est plus annoncée. Elle peut être proposée à nouveau depuis le même onglet, et une version plus récente sera annoncée normalement.

Rien n’est téléchargé ni installé sans votre accord. Les réglages de l’installation sont conservés : dossier, démarrage avec Windows tel que réglé dans la zone de notification, raccourcis. La mise à jour ne demande pas de droits administrateur. Si l’installation échoue, la version précédente redémarre.

## Hébergement et sécurité

Les versions sont publiées sur GitHub Releases. Gear Control lit `latest.json` sur la dernière release, sans passer par l’API GitHub ni par un autre service.

`latest.json` est signé avec une clé Ed25519 qui ne quitte pas le PC de publication. Gear Control ne contient que la clé publique : il refuse tout fichier qu’elle n’a pas signé, ainsi que toute version égale ou plus ancienne que la version installée. L’installateur téléchargé doit avoir exactement la taille et l’empreinte SHA-256 annoncées dans le fichier signé ; il est vérifié de nouveau juste avant son lancement. Un dépôt ou un compte compromis ne permet donc pas de diffuser un faux installateur.

La télécommande mobile ne peut ni vérifier ni installer de mise à jour.

## Publication

`Publish-Update.ps1` (ou `Publish-Update.cmd`) publie une version : signature de `latest.json`, création de la release, envoi des fichiers, puis vérification du flux public. Voir [Publier une mise à jour](INSTALLATION.md#publier-une-mise-à-jour).

## Tests plus fiables

Deux tests échouaient parfois quand toute la suite tournait en parallèle : la découverte USB de la DualSense et le renvoi des écrans du Stream Deck Mini après la fermeture du logiciel Elgato. Leurs délais d’attente étaient trop courts pour une machine chargée. Ils attendent désormais l’événement attendu, sans ralentir la suite. Cela évite aussi qu’une construction de l’installateur échoue au hasard.

**Première installation :** la 3.2.9 est la première version capable de se mettre à jour seule. Elle s’installe manuellement une fois ; les versions suivantes seront proposées automatiquement.
