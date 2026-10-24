# Gear Control 3.2.28 — Examiner mon PC

## Demander la prise en charge d’un périphérique

**Réglages généraux → Périphériques → Examiner mon PC** crée un rapport du matériel de ce PC. On l’enregistre, puis on l’envoie au développeur pour demander qu’un périphérique soit ajouté.

Après l’examen, la page classe les appareils trouvés :

- **Pas encore pris en charge** : aucun adaptateur Gear Control ne correspond à ses identifiants USB ou Bluetooth (VID:PID).
- **Pris en charge** : le modèle exact figure dans le catalogue de compatibilité.
- **Audio Windows standard** : Gear Control règle seulement le volume et la sourdine Windows.

**Enregistrer le rapport** télécharge `GearControl-examen-PC-<date>.json`. **Vérifier le contenu du rapport** affiche le fichier complet avant l’envoi.

Le périphérique doit être branché et allumé pendant l’examen. Un appareil Bluetooth éteint reste visible, mais Windows ne fournit pas sa description HID.

## Contenu du rapport

- **Système** : édition et build de Windows, carte mère, processeur, mémoire, cartes graphiques et version de leur pilote.
- **Périphériques**, regroupés par appareil physique (conteneur PnP de Windows) : bus (USB, Bluetooth, Bluetooth LE, audio intégré), VID:PID, nœuds Windows avec leur classe, leur pilote et leurs identifiants matériels, et sorties ou entrées audio.
- **Interfaces HID** de chaque appareil : page et usage, longueur des rapports d’entrée, de sortie et de fonctionnalité, et détail de chaque rapport (identifiant, page, usages, taille, plage). Ces informations suffisent pour préparer un adaptateur.
- **Logiciels fabricants** installés ou ouverts (G HUB, Synapse, iCUE, Wootility, INZONE Hub, Stream Deck…) : ils expliquent un conflit d’accès au matériel.
- **La vue de Gear Control** : adaptateurs actifs et fonctions détectées sur chaque appareil.

## Confidentialité

Le rapport ne contient ni le nom du PC, ni le nom d’utilisateur, ni les numéros de série, ni les adresses Bluetooth, ni les chemins système. Gear Control retire la partie des identifiants Windows propre à un appareil précis, ainsi que les identifiants de conteneur. Rien n’est envoyé automatiquement : l’utilisateur transmet lui-même le fichier.

L’examen est en lecture seule et ne communique pas avec les périphériques. Windows fournit la description HID qu’il a déjà lue au branchement. L’examen dure environ une seconde. Il est disponible uniquement sur le PC, pas depuis la télécommande.

Le **Diagnostic à partager** des versions précédentes est remplacé par ce rapport, qui reprend son contenu dans la section `gearControl`.
