# Gear Control 3.2.23 — inventaire illisible après une coupure

- Le service ne s’arrête plus en boucle quand `device-inventory.json` et sa sauvegarde sont tous les deux illisibles (par exemple remplis de zéros après une coupure de courant ou un arrêt forcé). Il essaie dans l’ordre le fichier, l’écriture en cours puis la sauvegarde. Si aucun n’est lisible, il reconstruit l’inventaire à partir des appareils déjà connus.
- Les copies illisibles restent dans le dossier de données sous le nom `device-inventory.corrupt-<horodatage>.json`. Un avertissement est affiché dans la gestion des périphériques.
- L’inventaire et sa sauvegarde sont maintenant écrits sur le disque avant d’être remplacés. La sauvegarde est elle aussi remplacée d’un seul coup, au lieu d’être réécrite sur place.
