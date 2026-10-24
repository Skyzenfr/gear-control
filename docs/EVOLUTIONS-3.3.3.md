# Version 3.3.3 — Lumières Philips Hue

Gear Control peut associer un pont Philips Hue sur le réseau local et afficher ses
lumières parmi les périphériques. Selon les capacités de chaque lampe, vous pouvez
l’allumer ou l’éteindre, régler sa luminosité, choisir sa couleur ou la température
du blanc. Ces réglages sont aussi disponibles dans les profils, les scènes, les
automatisations et les actions épinglées.

## Associer le pont

1. Ouvrez **Réglages généraux → Périphériques → Lumières Philips Hue**.
2. Choisissez **Chercher un pont Hue**, ou saisissez l’adresse IPv4 du pont indiquée
   dans l’application Hue si la découverte ne fonctionne pas.
3. Appuyez sur le bouton physique du pont, puis choisissez **Associer le pont**
   dans les 30 secondes.
4. Ajoutez les lumières détectées à votre espace dans la gestion des périphériques.

La liste des périphériques est actualisée après l’association ou la déconnexion du
pont, en conservant les formulaires et les saisies en cours.

Hue Sync installé sur le PC peut rester ouvert. Gear Control crée sa propre clé
d’association, protégée par Windows, et ne récupère pas celle de Hue Sync. Les
réglages sont suspendus pendant une synchronisation Hue active. La connexion au
pont utilise HTTPS avec vérification des autorités et de l’identité Hue.

## Portée et vérification

Cette version gère un pont compatible API v2 à la fois. Le Bluetooth direct, les
scènes et effets Hue natifs, les pièces/zones et la synchronisation d’écran restent
dans l’application Hue. La découverte automatique nécessite Internet ; la saisie
manuelle et les commandes utilisent le réseau local. Un changement d’adresse du
pont peut nécessiter une nouvelle association.

Une modification est relue avant d’être annoncée comme appliquée. Les profils
conservent le mode couleur ou blanc actif, sans enregistrer de valeur supposée pour
le mode inactif. Une lumière simulée permet de vérifier les contrôles et les scènes
sans matériel.

Les tests couvrent le transport HTTPS, l’association, la protection des clés,
les capacités, les conversions de couleur, les refus de commande, la confirmation
et l’intégration avec l’inventaire et les scènes : **952 tests, 951 réussis,
1 ignoré, aucun échec**. `npm run check` et `npm run smoke` sont verts, sans accès
au matériel. Le panneau a été vérifié dans Edge
en français et en anglais, à 1440 et 520 pixels. **Les écritures sur de vraies lampes
restent à vérifier après association du pont dans Gear Control.**

Voir [le protocole et les sources](HUE-PROTOCOL.md).
