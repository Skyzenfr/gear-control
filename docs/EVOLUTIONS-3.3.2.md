# Gear Control 3.3.2 — Sélection multiple et déclencheurs

- **Plusieurs périphériques déclencheurs** : recherche, sélection de plusieurs
  appareils, tout sélectionner ou tout désélectionner. Une connexion ou
  déconnexion peut agir pour chaque appareil, ou seulement lorsque tous les
  appareils sélectionnés sont dans l’état demandé.
- **Plusieurs événements pour une automatisation** : jusqu’à huit déclencheurs
  reliés par **OU** lancent la même séquence. Les événements existants restent
  disponibles : connexion, déconnexion, batterie faible, ouverture et fermeture
  d’application.
- **Nouveaux déclencheurs** : batterie qui atteint un seuil en remontant, début
  et arrêt de charge, application qui passe au premier plan, horaire avec choix
  des jours de la semaine. Le délai minimal entre deux lancements est réglable.
- **Actions sur plusieurs appareils** : ajoutez plusieurs périphériques d’un
  coup, puis choisissez les réglages à inclure et leurs valeurs pour chacun.
  Les réglages structurés déjà mémorisés restent conservés ou peuvent être
  retirés ; leur configuration détaillée reste disponible dans les profils.
- **Séquences** : pauses de 100 ms à 30 s entre actions, réorganisation,
  duplication d’une automatisation ou d’un raccourci. Une automatisation
  dupliquée est désactivée jusqu’à sa vérification.
- **Historique** : les 60 dernières exécutions de la session affichent leur
  origine, leur date, leur état, le nombre d’actions terminées et les erreurs.

Les règles et raccourcis existants restent compatibles. Les nouveaux textes
sont disponibles en français et en anglais sur PC. La sauvegarde complète de
Gear Control conserve les automatisations ; les pauses sont aussi conservées
dans les exports et imports de profils.

Les horaires utilisent l’heure locale du PC. Gear Control et le PC doivent
être actifs ; les horaires manqués pendant un arrêt ou une veille ne sont pas
rejoués. Un horaire déjà traité n’est pas relancé par un redémarrage dans la
même minute. Les événements de batterie et de charge nécessitent deux
lectures valides successives : une connexion ou une valeur inconnue ne compte
pas comme un franchissement de seuil.

Les essais utilisent le service en mode simulateur et des adaptateurs et
applications factices. Aucune nouvelle commande matérielle n’est ajoutée.

Installateur : `release/GearControl-Setup-3.3.2.exe`.
