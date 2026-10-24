# Audio Windows, télécommande et automatisations

Le mélangeur affiche les véritables sessions audio Windows de chaque sortie active. Discord, un navigateur ou un jeu peuvent avoir plusieurs sessions : elles restent distinctes, même lorsqu’elles partagent le même processus. Le volume et le mute ne changent que le paramètre demandé. Une session fermée ou remplacée est refusée avant toute écriture ; les identifiants publics sont opaques et ne contiennent pas les chemins des exécutables.

Le déplacement d’une application vers une autre sortie n’est pas exposé par l’API Core Audio documentée utilisée ici. Le lien **Changer les sorties dans Windows** ouvre le mélangeur natif. Gear Control n’affiche pas une modification de routage qu’il ne peut pas confirmer.

La balance est disponible uniquement pour les sorties Windows comportant exactement deux canaux. Le réglage atténue le canal gauche ou droit, conserve le niveau du canal le plus fort et confirme les deux niveaux. Une sortie silencieuse doit d’abord avoir un volume supérieur à zéro. Le test audio du navigateur utilise sa propre sortie sélectionnée : l’identifiant d’une sortie Windows n’est pas interchangeable avec un `deviceId` du navigateur.

## Scènes et règles

Ces fonctions sont regroupées dans **Réglages généraux → Profils & automatisations**. Le mélangeur se trouve dans **Réglages généraux → Audio Windows**.

Un profil contient les réglages choisis pour un ou plusieurs périphériques et, si nécessaire, une sortie audio. Depuis la version 3.3.1, les valeurs se modifient directement dans l’éditeur : seuls les réglages sélectionnés sont appliqués. Modifier un profil ne modifie pas immédiatement le matériel. Pour un clavier, les touches et fonctions proposées proviennent des capacités annoncées ; aucune lecture complète implicite n’est nécessaire. Les profils et suites d’actions se sauvegardent dans `automation.json` du répertoire de données. Le moteur limite le nombre de scènes à 32, de règles à 64 et d’actions par scène à 8.

### CS2 et retour au Bureau

1. Créez un profil de jeu **CS2** et choisissez le jeu installé ou saisissez `cs2.exe`. Activez son application automatique.
2. Sélectionnez votre clavier, cochez les réglages voulus et donnez-leur les valeurs souhaitées pour CS2. Enregistrez le profil.
3. Créez un profil **Bureau** avec vos valeurs habituelles, puis choisissez-le comme profil de retour.
4. Choisissez le retour dès que le jeu quitte le premier plan pour rétablir ces valeurs lors d’un Alt-Tab. Désactivez cette option pour attendre la fermeture du jeu.

Au retour dans CS2, les réglages du jeu sont réappliqués. Les réglages non inclus dans le profil Bureau ne sont pas réinitialisés automatiquement : incluez les mêmes paramètres dans les deux profils si vous voulez deux valeurs distinctes. Un périphérique hors ligne d’un profil attend sa reconnexion. Si plusieurs jeux sont ouverts, l’option de priorité au premier plan détermine lequel prend la main.

### Connexions d’appareils, applications et raccourcis

Le panneau d’automatisations permet de créer une règle sans devoir préparer une scène ailleurs. Choisissez un événement, sa cible et une ou plusieurs actions : sortie audio Windows, application locale `.exe`, raccourci clavier ou réglages d’un profil existant. Une suite manuelle n’a pas de déclencheur et peut se lancer depuis ce panneau ou depuis le menu des profils dans la barre supérieure.

- **INZONE → son** : choisissez la connexion des INZONE, puis l’action de sortie audio avec leur sortie Windows. Branchez les écouteurs au moins une fois pour pouvoir sélectionner cette sortie. Il s’agit du périphérique de sortie par défaut de Windows ; les applications utilisant une sortie fixée dans leurs propres réglages peuvent la conserver.
- **DualSense → Steam** : choisissez la connexion de votre manette, puis l’ouverture d’application. Utilisez le sélecteur pour choisir le fichier `steam.exe` de votre installation.
- **Autres possibilités** : changer de sortie à la déconnexion d’un casque, appliquer un profil lors du lancement d’une autre application, envoyer une combinaison comme `Ctrl+Shift+M`, ou enchaîner plusieurs de ces actions.

Le bouton d’exécution manuelle applique réellement les actions. L’historique permet de voir leur résultat. Un lancement d’application ou un raccourci n’est pas répété automatiquement à chaque retour dans un profil de jeu.

Les actions s’exécutent en série et les cibles connues comme déconnectées sont vérifiées avant la première écriture. Lors d’une connexion, la sortie audio peut apparaître légèrement après l’appareil : le moteur l’attend pendant un délai borné. Si un périphérique disparaît pendant l’exécution ou si une commande échoue, la suite s’arrête. L’historique indique le nombre d’actions déjà confirmées : ces actions ne sont pas automatiquement annulées sur les autres périphériques.

Les appareils déjà présents au démarrage du service ne sont pas traités comme de nouvelles connexions. Une règle de connexion s’applique au prochain changement détecté. Pour les INZONE, la liaison des écouteurs est vérifiée même si leur récepteur USB est resté branché ; le délai dépend du prochain rafraîchissement du périphérique. Les profils importés contenant des applications ou raccourcis conservent leurs actions, avec activation automatique désactivée.

Une règle de processus utilise son nom exact, par exemple `cs2.exe`. Le jeu peut être choisi dans la liste des jeux installés ou par son exécutable. La liste des processus utilise `Get-Process` à la demande dans un helper caché et limité dans le temps ; elle ne collecte pas leurs lignes de commande. Le lancement d’une application utilise uniquement le chemin local `.exe` explicitement choisi, sans arguments ni commande shell.

Les notifications de processus utilisent `Win32_ProcessStartTrace` et `Win32_ProcessStopTrace` lorsque Windows les autorise. Sur une session sans ces droits, Gear Control utilise `SetWinEventHook` et la notification `Process.Exited`. Dans ce mode, le démarrage correspond à la première fenêtre visible de l’application ; un processus exclusivement en arrière-plan ne déclenche pas cette règle. Le panneau affiche le mode et les sources effectivement disponibles. Aucun balayage périodique des processus n’est utilisé.

Les changements matériels proviennent de `Win32_DeviceChangeEvent` et les changements de sortie de `IMMNotificationClient`. Le hub regroupe les rafales de notifications avant de relire les périphériques. Une lecture USB périodique plus espacée reste nécessaire pour les batteries et les appareils dont le protocole ne fournit pas de notifications. Le navigateur reçoit les mises à jour par un flux d’événements authentifié au lieu d’interroger l’état toutes les quelques secondes.

### Sélection multiple et déclencheurs (3.3.2)

Dans **Raccourcis et automatisations → Créer**, le mode **Sur un ou plusieurs
événements** permet d’ajouter jusqu’à huit déclencheurs. Ils sont reliés par
**OU** : chaque déclencheur peut lancer la même séquence.

- Pour un événement de périphérique, cochez un ou plusieurs appareils. La
  recherche et les boutons de sélection globale permettent de préparer un
  groupe. **Chacun des périphériques sélectionnés** réagit à chaque appareil ;
  **Quand tous sont connectés / déconnectés** attend le groupe complet. Ce
  groupe ne se relance pas tant que son état n’a pas changé.
- **Batterie sous un seuil** et **La batterie atteint un seuil** réagissent au
  franchissement descendant ou montant du pourcentage choisi (5 à 100 %).
  **La charge commence / s’arrête** nécessite un état de charge effectivement
  transmis par l’appareil. La première lecture et les reconnexions ne
  déclenchent aucune de ces actions.
- **Une application passe au premier plan** utilise son nom exact, par exemple
  `discord.exe`. Les profils automatiques des jeux continuent de fonctionner
  indépendamment de ces règles.
- **À une heure précise** utilise l’heure locale du PC et les jours cochés.
  Le service doit être actif à cette heure : les horaires manqués ne sont pas
  rattrapés après une veille ou un arrêt. Les horaires déjà traités sont
  mémorisés pour éviter une répétition au redémarrage dans la même minute.

**Ajouter plusieurs périphériques** crée une action indépendante par appareil,
avec ses propres valeurs mémorisées. Cochez les réglages à inclure et modifiez
leurs valeurs. Les réglages structurés copiés d’un profil (égaliseur, touches,
gâchettes…) restent conservés et peuvent être retirés ; leurs éditeurs détaillés
restent dans les profils ou la fiche du périphérique. Un appareil sans aucune
valeur applicable empêche l’enregistrement tant que son action n’a pas été
complétée ou retirée. La limite reste de huit actions au total.

L’action **Attendre entre deux actions** ajoute une pause de 100 à 30 000 ms.
Les flèches permettent de changer l’ordre. Le **délai minimal entre deux
lancements** est partagé par tous les déclencheurs de la règle ; un lancement
déjà en cours n’est pas dupliqué. Les événements reçus pendant ce délai sont
ignorés, pas mis en attente. Une désactivation ou modification de règle annule
les actions restantes de son exécution au prochain contrôle, sans annuler les
actions déjà confirmées.

La duplication ouvre une copie à vérifier avant enregistrement. Une règle
dupliquée est désactivée par défaut. Le panneau **Historique des exécutions**
affiche les 60 dernières exécutions de la session et le nombre d’actions
terminées ; il se met à jour avec les événements du service.

## Raccourcis et iPhone

Avec le service Gear Control actif, **Ctrl + Alt + F9** sélectionne la sortie précédente et **Ctrl + Alt + F10** la suivante, même lorsque le site est fermé. Les raccourcis sont enregistrés auprès de Windows avec `RegisterHotKey` et sans répétition ; le panneau signale un conflit avec une autre application. Le thread de raccourcis attend les messages Windows et ne fait pas de polling.

L’appairage mobile conserve le QR code et le jeton du réseau local. La télécommande affiche toutes les batteries disponibles, les scènes enregistrées, les commandes du micro et les sessions du mélangeur par application. Les requêtes et le flux d’événements envoient le jeton dans l’en-tête `Authorization`, pas dans l’URL du flux. La création de scènes/règles reste disponible sur le PC ; le mobile peut exécuter une scène déjà enregistrée.

## Références

- [IAudioSessionManager2::GetSessionEnumerator](https://learn.microsoft.com/en-us/windows/win32/api/audiopolicy/nf-audiopolicy-iaudiosessionmanager2-getsessionenumerator)
- [IAudioSessionControl2::GetSessionInstanceIdentifier](https://learn.microsoft.com/en-us/windows/win32/api/audiopolicy/nf-audiopolicy-iaudiosessioncontrol2-getsessioninstanceidentifier)
- [IAudioEndpointVolume::SetChannelVolumeLevelScalar](https://learn.microsoft.com/en-us/windows/win32/api/endpointvolume/nf-endpointvolume-iaudioendpointvolume-setchannelvolumelevelscalar)
- [SetWinEventHook](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setwineventhook)
- [RegisterHotKey](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-registerhotkey)
