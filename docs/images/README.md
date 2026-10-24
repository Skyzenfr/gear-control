# Visuels du README

- `logo.png` : copie de `app/public/brand/gear-control-512.png`, identité actuelle.
- `accueil.png` : vue d’ensemble et réglages épinglés actuels.
- `egaliseur.png` : égaliseur actuel et ses profils.
- `automatisations.png` : profils de jeu et règles actuels.
- `dualsense.png` : panneau interactif actuel, éclairage et configuration.
- `telecommande.png` : accueil actuel sur téléphone, en 430 × 1100.
- `telecommande-appareil.png` : commandes actuelles des INZONE Buds sur téléphone.

Les six captures ont été réalisées le 24 octobre 2026 après `npm run build`,
avec le code actuel de `app/src`, dans un navigateur Edge sans interface visible.
Le serveur temporaire sert le vrai `app/dist` et des API de démonstration isolées.
Les anciens fichiers de capture ne sont pas réutilisés. Les panneaux égaliseur
et DualSense sont capturés directement dans la page ; aucun visuel n’est retouché.

Le navigateur de capture est limité au serveur local de démonstration. Aucune
commande n’est envoyée au service installé ni aux périphériques. Aucun QR
d’association, jeton ou adresse du réseau privé n’est inclus.

Le script de rendu et les preuves locales (date, empreintes SHA-256 du build
et de chaque image, requêtes, erreurs) se trouvent dans
`.build/ui-qa/capture-readme-current.mjs` et
`.build/presentation/current/report.json`. Le rapport confirme six captures,
aucune erreur JavaScript et aucun endpoint de démonstration manquant.
