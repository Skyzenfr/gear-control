# Gear Control 2.7

## Profils de jeu

Dans **Réglages généraux → Profils de jeu**, créez un profil, choisissez un jeu, ses périphériques et sa sortie audio. L’option **Activer au lancement** associe le profil à l’exécutable du jeu. Il n’est pas nécessaire que le jeu soit lancé pendant l’enregistrement. Un exécutable personnalisé peut être saisi dans **Autre jeu ou application**.

Les cartes remplacent les formulaires de scènes et de règles séparés. Les profils existants et leurs déclencheurs sont conservés. **Profils** dans la barre supérieure et dans la télécommande permet une activation manuelle.

## Périphériques déconnectés

Le hub conserve les derniers réglages confirmés dans `hub-devices.json`, à côté des autres données du service. Les touches du clavier sont regroupées au fil de leurs lectures. Ces réglages sont distingués de l’état matériel actuel : un appareil déconnecté conserve des commandes désactivées et aucune batterie fictive.

Une carte **Derniers réglages** peut être ajoutée à un profil même hors connexion. Un appareil jamais lu n’a pas de réglages à enregistrer. Lorsqu’un profil est activé, ses appareils absents restent en attente et reçoivent leurs réglages à la reconnexion, tant que ce profil reste actif. Changer de profil annule l’attente du précédent.

Les profils enregistrent les réglages pris en charge, y compris l’égaliseur et les gestes tactiles lorsqu’ils sont disponibles.

## Clavier visuel

Les touches affichent l’activation en rose, la réactivation en bleu et le relâchement en vert. L’éclair indique Rapid Trigger. Les valeurs en attente apparaissent immédiatement sur le clavier et dans l’aperçu du switch. **Lire toutes les touches** actualise les valeurs sans changer leur configuration.

L’aperçu du switch est une simulation : le curseur et **Appuyer** servent à tester le comportement des valeurs choisies. Il ne mesure pas la course physique du clavier. La modification d’un curseur prépare les réglages ; **Appliquer** les transmet au périphérique.
