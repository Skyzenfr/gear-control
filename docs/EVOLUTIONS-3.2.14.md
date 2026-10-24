# Gear Control 3.2.14 — correctif du démarrage du service

## Le service ne démarrait plus après la mise à jour 3.2.13

Après l’installation de la 3.2.13, le menu de la zone de notification restait sur « Service en cours de connexion… » et « Profil indisponible », et le hub ne s’ouvrait pas. Le journal du service montrait un plantage immédiat à chaque relance :

```
Error [ERR_MODULE_NOT_FOUND]: Cannot find module '…\app\server\battery-estimator.mjs' imported from …\app\server\hub.mjs
```

Le module d’estimation de l’autonomie, ajouté en 3.2.13, n’était pas copié dans l’installateur. Le script de construction utilisait une liste de fichiers tenue à la main, et ce nouveau fichier n’y avait pas été ajouté.

## Ce qui change

- L’installateur embarque désormais tous les fichiers du service (`app/server` et `app/server/adapters`), sauf les tests et les fixtures. Un nouveau module ne peut plus être oublié.
- La construction vérifie que chaque import relatif du service existe dans le paquet. Si un module manque, la construction échoue et affiche son nom, au lieu de produire un installateur qui plante au démarrage.
