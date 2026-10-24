# Gear Control 3.2.26 — fluidité, fiabilité et télécommande maîtrisée

## Un service qui ne se fige plus

- La recherche des périphériques USB est désormais faite une seule fois par cycle, en arrière-plan, puis partagée par tous les adaptateurs. Avant, elle était faite une dizaine de fois par cycle, dont trois fois en bloquant le service : celui-ci se figeait jusqu’à 1,2 s toutes les 12 s, retardant les touches du Stream Deck, les pages et la télécommande.
- Les G FITS et les INZONE Buds utilisent la même communication USB asynchrone que les autres périphériques.
- La liste des logiciels fabricants (G HUB, Synapse, Elgato…) est tenue à jour en arrière-plan. Un rafraîchissement n’attend plus `tasklist` (environ 300 ms), et l’ouverture d’un logiciel fabricant reste détectée en quelques secondes.
- Après une commande (réglage, action épinglée, touche du Stream Deck, scène), seul le périphérique concerné est relu, au lieu de tous les adaptateurs et de la détection Windows. L’action épinglée ne déclenche plus trois relectures complètes.
- Les G FITS relisent leurs gestes et leur retour vocal à la connexion, après une modification, puis une fois par minute. Les batteries restent relues à chaque cycle. La radio des écouteurs est moins sollicitée.
- L’état de l’égaliseur Sony est relu toutes les 30 s, et tout de suite après une modification.
- La manette DualSense n’est plus écoutée en continu : la lecture est suspendue entre deux relevés de la batterie.
- La détection des applications installées (écran PC de la télécommande) est conservée 30 s.

## Données sur le disque

- L’inventaire des périphériques n’est plus réécrit toutes les 12 s. Un horodatage du clavier et de la souris suffisait à le réécrire, avec sa copie de secours, à chaque cycle : environ 2 Go d’écritures par jour. Seules la présentation, l’appartenance à votre espace et les dernières valeurs connues sont enregistrées, et seulement quand elles changent.
- Si Windows refuse une écriture (fichier verrouillé par un antivirus, disque plein), la mise à jour des périphériques continue, et l’enregistrement est retenté au cycle suivant. Les remplacements de fichiers sont retentés pendant environ une seconde.
- Chaque écriture utilise son propre fichier temporaire.
- Le lanceur ne réécrit plus `supervisor.json` chaque seconde : il l’écrit quand l’état du service change, et au plus une fois par minute sinon.

## États plus justes

- Une commande en cours ne fait plus passer les périphériques de son adaptateur en « Indisponible ». Ce faux état effaçait vos modifications non appliquées et déclenchait des automatisations de déconnexion puis de reconnexion.
- Les périphériques ne passent plus tous en « données périmées » quand un cycle est simplement lent. Seule une boucle de lecture vraiment bloquée les marque ainsi, et les commandes restent refusées dans ce cas.
- Un récepteur G FITS lu avant son numéro de série ne crée plus de seconde fiche. La fiche existante est reprise, comme pour les INZONE Buds. Deux fiches d’un même périphérique n’apparaissent plus en double dans les listes.
- Le lanceur ne redémarre plus le service à cause d’un cycle USB lent ou d’une erreur lors du transfert d’une mise à jour ou d’un sélecteur de fichier.
- Le service média Windows redémarre seul après un arrêt inattendu.
- L’arrêt du service libère toujours le matériel, même si une étape échoue avant.

## Télécommande

- **Réglages généraux → Télécommande mobile** permet d’activer ou de désactiver la télécommande. Désactivée, le service n’écoute que ce PC : Windows n’affiche plus son alerte de pare-feu au premier lancement. Une nouvelle installation commence désactivée, sauf si l’option de l’installateur a réussi. Une installation existante garde la télécommande active.
- **Révoquer les téléphones** crée une nouvelle clé : un téléphone perdu ou un lien partagé ne donne plus accès au PC. Chaque téléphone doit alors scanner le nouveau QR code.
- `Enable-RemoteAccess.ps1` supprime les règles créées par l’alerte du pare-feu Windows pour le moteur de Gear Control. Une règle de blocage annulait sinon l’autorisation, et une règle d’autorisation ouvrait le port au-delà du réseau local. L’installateur attend la fin de ce script avant de démarrer le service. La désinstallation retire la règle.
- Après un réglage, le téléphone ne demande plus une relecture complète des périphériques.

## Interface plus légère

- Les pages reçoivent l’état complet seulement quand il change. Sinon, un court signal indique que les données sont à jour.
- Les notifications audio Windows arrivant en rafale sont regroupées : les pages relisent les sorties une seule fois par rafale.
- Les visuels personnalisés et les pochettes d’album ne sont plus envoyés à chaque mise à jour. Ils sont servis une seule fois à une adresse propre à leur contenu, que le navigateur conserve.
- L’interface est compressée (Brotli ou gzip) : le script principal passe par exemple de 586 Ko à 170 Ko. Les fichiers versionnés sont conservés un an par le navigateur.
- Le PC et le téléphone chargent chacun leur propre interface. Le téléphone télécharge environ huit fois moins de données.
- Pendant un téléchargement de mise à jour, la progression est envoyée une fois par seconde, et seulement aux pages ouvertes sur le PC.

## Corrections

- La capture d’écran à distance n’est plus tronquée quand la mise à l’échelle de Windows dépasse 100 %.
- Une réponse illisible du service affiche un message clair au lieu de « Unexpected token… ».
- Une coupure du flux temps réel n’empêche plus d’utiliser les commandes du PC. Un simple avertissement s’affiche pendant la reconnexion.
- Une erreur d’affichage propose désormais de réessayer ou de recharger la page, au lieu de figer l’interface.
- Mises à jour : un double clic ne peut plus préparer deux installations, et **Annuler** retire un installateur que le lanceur n’a pas encore démarré.
- Les profils dont toutes les actions ont été dissociées sont ignorés et signalés à l’import, au lieu de faire échouer tout l’import.
- Lorsqu’un profil en attente ne peut pas s’appliquer à la reconnexion d’un périphérique, la raison s’affiche dans son historique.
- L’action Stream Deck **Ouvrir un logiciel** refuse davantage d’outils capables d’exécuter du code (WSL, msiexec, dotnet, certutil…).
- Publication : un brouillon de version interrompu est supprimé au lieu de s’accumuler. Un build raté remet le numéro de version d’origine.
