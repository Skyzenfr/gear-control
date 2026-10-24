# Version 3.3.6 — Actions à l’arrêt et automatisations sans doublon

L’événement **À l’arrêt de Gear Control** permet d’éteindre une lumière Hue ou
d’exécuter une autre action avant la déconnexion des appareils. Il se lance une
seule fois lorsque le service se ferme proprement, notamment avec **Quitter**
depuis l’icône Gear Control près de l’horloge, lors d’un redémarrage du service
ou d’une mise à jour.

Dans **Réglages généraux → Profils & automatisations**, le préréglage
**Lumière à l’arrêt → éteindre ou allumer** prépare une extinction. Vous pouvez
choisir la lumière, modifier l’état souhaité et enregistrer la règle activée.
Les autres actions restent disponibles dans une automatisation personnalisée.

Les anciennes règles qui attendaient exactement la fermeture de `gearcontrol.exe`
sont reprises avec le nouvel événement d’arrêt. Attendre cette fermeture était
trop tardif : le moteur chargé d’exécuter les actions était déjà arrêté.

Les actions disposent d’un délai global de dix secondes avant la fermeture des
adaptateurs. Le lanceur attend leur fin et conserve une limite d’arrêt. Une
commande déjà envoyée ne peut pas être annulée ; aucune nouvelle écriture Hue
différée n’est lancée après l’expiration du délai. Les commandes ne sont pas
répétées automatiquement.

Fermer la page ou la fenêtre de Gear Control laisse le service actif et ne
déclenche pas cet événement. L’arrêt et la fermeture de session Windows ne
déclenchent pas automatiquement cette fermeture gracieuse : utilisez **Quitter
Gear Control** avant si vous souhaitez exécuter ces actions. Une coupure de
courant ou un arrêt forcé du processus ne permettent pas de garantir leur exécution.

Les scènes internes des règles ne sont plus présentées une seconde fois comme
profils manuels. Les profils identifiés, le profil Bureau et les anciens profils
de jeu restent disponibles ; les raccourcis manuels conservent leur place et
aucune scène n’est supprimée. L’export de profils applique le même classement.

Lorsque le service est arrêté, **Failed to fetch** est remplacé par un message
en français avec **Réessayer**. Le retour dans la fenêtre permet aussi de
recharger les données après le redémarrage du service.

Vérifications : **1 016 tests, 1 015 réussis, 1 ignoré, aucun échec**, contrôle
Svelte sans erreur ni avertissement et test HTTP du vrai service en simulateur
sur le port 3299. Ce dernier utilise le même canal d’arrêt que le lanceur et
confirme l’extinction de la lumière et un autre réglage avant la fermeture des
adaptateurs. Un superviseur Windows compilé a aussi été vérifié avec un enfant
factice, dont une action de six secondes et une fermeture forcée bornée.

L’interface a été vérifiée dans Edge en français et en anglais, à 1440 et 520
pixels : deux cartes au total pour les deux règles de la capture, migration,
enregistrement et réouverture de l’état d’arrêt, panne réseau et reprise. Ces
24 captures n’ont produit ni débordement ni erreur JavaScript et aucune commande
de lumière n’a été envoyée pendant les parcours de configuration.

La version 3.3.6 a été construite et installée localement. L’essai sur le pont
réel a utilisé la règle d’arrêt existante : la lumière était allumée avant
**Quitter**, puis son extinction a été confirmée par une lecture Hue indépendante,
sans commande manuelle de lumière. La commande d’arrêt du lanceur a réussi en
1 406 ms ; Gear Control a ensuite été relancé et confirmé prêt en version 3.3.6,
avec un seul lanceur actif. Une nouvelle lecture Hue indépendante a confirmé
l’allumage par la règle de démarrage existante. Les données d’automatisation
ont été préservées ; seul le déclencheur de fermeture a été migré.
L’allumage et l’extinction sont désormais validés sur matériel ; luminosité,
couleur et température du blanc restent à vérifier. Aucune publication effectuée.
