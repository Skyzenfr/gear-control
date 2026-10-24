# Version 3.3.5 — Lumières Hue sur Stream Deck et au démarrage

Les lumières Philips Hue associées peuvent être allumées ou éteintes depuis une
touche Stream Deck et par une automatisation au démarrage de Gear Control.
Ajoutez d’abord les lumières souhaitées à votre espace dans **Réglages généraux →
Périphériques** si elles ne figurent pas encore parmi les appareils disponibles.

## Sur le Stream Deck

1. Ouvrez les réglages de votre Stream Deck dans Gear Control.
2. Choisissez une touche, puis **Action → Éclairage**.
3. Sélectionnez la lumière et l’une des trois commandes : **Allumer la lumière**,
   **Éteindre la lumière** ou **Allumer / éteindre la lumière**.
4. Activez **Utiliser avec Gear Control** si nécessaire, puis **Appliquer**.

Les deux premières commandes imposent un état précis. La troisième inverse
l’état de la lumière à chaque appui. Elles restent disponibles dans **Commande
Gear Control** et peuvent aussi être épinglées. Les touches existantes conservent
leur configuration. Le Stream Deck doit être piloté par Gear Control pour
exécuter ses actions.

## Au démarrage

Dans **Réglages généraux → Profils & automatisations**, choisissez le préréglage
**Lumière au démarrage → allumer ou éteindre**. Choisissez la lumière et son état,
puis enregistrez la règle activée. Le réglage de puissance peut être activé pour
allumer ou désactivé pour éteindre ; la couleur et la luminosité sont conservées.

L’événement **Au démarrage de Gear Control** est aussi disponible dans les
automatisations personnalisées. Il se déclenche une seule fois au démarrage du
service, après la détection initiale. Ouvrir la page ou enregistrer, activer ou
modifier une règle ne l’exécute pas. Le service continue à fonctionner lorsque
la fenêtre est fermée : rouvrir cette fenêtre ne constitue pas un démarrage.

Le moteur attend les appareils pendant au plus 30 secondes. Une lumière toujours
indisponible produit un échec dans l’historique ; sa reconnexion ultérieure ne
rejoue pas l’action. Les commandes ne sont pas répétées automatiquement. Une
modification ou suppression de la règle ou de sa scène annule l’ancienne action
en attente.

La configuration d’une touche ou d’une automatisation n’envoie aucune commande à
une lumière. Les écritures passent par l’adaptateur Hue existant, avec contrôle
de l’identité du pont, respect d’une synchronisation active et relecture de l’état.

Vérifications : **991 tests, 990 réussis, 1 ignoré, aucun échec** ; contrôle Svelte
sans erreur ni avertissement. Le test HTTP en simulateur sur le port 3299 vérifie
les trois choix Stream Deck et redémarre le service deux fois pour confirmer
l’extinction puis l’allumage, sans exécution à l’enregistrement ni au rafraîchissement.
Les tests n’utilisent aucun matériel réel.

L’interface a également été vérifiée dans Edge en français et en anglais, à
1440 et 520 pixels : trois commandes de touche, enregistrement et réouverture,
automatisation d’allumage puis d’extinction et persistance sur disque. Aucune
commande de lumière n’a été envoyée pendant ces parcours de configuration.
