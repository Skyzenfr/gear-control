# Gear Control 2.4

## Clavier

Le configurateur démarre sans touche sélectionnée. Cliquez pour sélectionner une touche ; Ctrl/Cmd ou Maj + clic ajoutent ou retirent une touche. Tracez un rectangle sur le clavier pour sélectionner une zone ; utilisez Ctrl/Cmd ou Maj pour ajouter cette zone à la sélection.

Les champs affichent les valeurs différentes entre les touches. Les boutons −/+ décalent chaque valeur individuelle d’un pas. Saisir une valeur commune modifie uniquement ce champ : les autres paramètres de chaque touche restent conservés. Vous pouvez continuer à sélectionner des touches avec un brouillon en cours ; **Appliquer** envoie toutes les modifications en attente, puis les relit.

**Ctrl+Z** annule une modification et l’historique décrit les opérations du configurateur. Copier nécessite une seule touche ; coller copie ses paramètres sur la sélection. La restauration reprend les réglages lus au début de cette session de configuration, sans effectuer une réinitialisation complète du clavier.

Les profils **CS2**, **FPS** et **Bureautique** proposent un point de départ et peuvent enregistrer les réglages de touches sous ces noms, dans ce navigateur. Charger prépare un brouillon ; **Appliquer** reste nécessaire. La carte thermique lit explicitement toutes les touches ; une lecture inconnue reste sans couleur. Les lectures suivantes ne rescannent pas toute la matrice en permanence.

Le panneau distingue les fonctions vérifiées des fonctions disponibles uniquement dans l’outil officiel. Les automatisations d’entrées opposées peuvent être interdites dans les jeux compétitifs : consultez notamment les règles officielles de CS2 avant d’activer SOCD ou un mode similaire. Le hub ne simule pas une mesure analogique de course avec un simple événement de frappe.

## Souris ATK

Les modes **Base**, **Compétitif ATK Shard** et **ATK Shard Max** sont des modes de performance du capteur ; cette commande ne flashe pas le firmware. Les réglages directement lus comprennent la synchronisation de mouvement, la correction de ligne droite, le LOD de 0,7 à 1,7 mm, la veille et la rotation du capteur. Le mode longue distance et l’éclairage sont accessibles sur la connexion récepteur compatible. Les champs non reconnus restent indisponibles.

Le diagnostic distingue la fréquence configurée de la fréquence mesurée et affiche la batterie, la connexion, le récepteur et la version du firmware. La fréquence USB mesurée et la qualité radio ne sont pas transmises par le protocole validé ; un tiret ou « Non transmise » l’indique.

## Audio et égaliseur

Chaque bande EQ dispose de boutons −/+, d’une aide sur son rôle et d’un historique avec **Ctrl+Z** et **Ctrl+Maj+Z**. Les préréglages JSON s’importent et s’exportent après validation des fréquences et des limites de l’appareil.

**B · Préécouter le brouillon** applique réellement l’EQ Sony à la sortie Game. **A · Avant** permet d’écouter le réglage initial ; **Restaurer A** termine la préécoute en restaurant ce réglage. Changer de page conserve cette comparaison pendant la session ouverte. **Appliquer** conserve le brouillon ; **Annuler le brouillon** restaure d’abord A. Fermer complètement la page pendant une préécoute laisse le dernier EQ appliqué sur l’appareil.

L’analyseur affiche les mesures d’une source explicitement autorisée au navigateur : audio d’un écran/onglet partagé, ou microphone. Il n’enregistre ni ne transmet le son. Le spectre décrit le signal reçu par le navigateur et ne confirme pas le traitement appliqué par l’appareil. Sans source autorisée, aucun spectre artificiel n’est affiché. Les API de capture nécessitent localhost ou HTTPS ; l’accès mobile HTTP ne dispose pas des mêmes permissions.

Le test gauche/droite/stéréo joue une tonalité brève à volume modéré sur la sortie du navigateur. Le choix explicite de cette sortie dépend de sa prise en charge par le navigateur. La balance utilise la sortie Windows associée aux écouteurs et ne devient disponible que lorsque deux canaux sont confirmés.

Le mélangeur montre les vraies sessions audio ouvertes sous Windows, avec leur sortie, volume et sourdine. Une application silencieuse ou fermée peut ne pas avoir de session. Le déplacement d’une application vers une autre sortie utilise actuellement **Paramètres Windows → Système → Son → Mélangeur de volume** ; le hub ne prétend pas exécuter un routage non pris en charge.

## Interface et télécommande

**Vos réglages**, dans la vue d’ensemble, devient éditable. Les brouillons sont communs à cette vue et à la page Réglages. Les états distinguent **Modifié**, **Application en cours**, **Appliqué** et **Échec**. **Ctrl+K** recherche un appareil, une page ou un réglage disponible. Les visuels partagent une zone normalisée, avec centrage et conservation des proportions.

Le QR code existant conserve l’appairage sur le réseau local. La télécommande enrichie affiche les batteries, les scènes enregistrées sur le PC et le mélangeur par application, en plus de la sortie, du volume et des commandes micro. Le téléphone peut appliquer une scène enregistrée ; la création des scènes et des règles reste sur le PC.

## Automatisations locales

Une scène enregistre une sortie audio et/ou des paramètres actuellement lus sur les appareils sélectionnés. Les paramètres clavier capturés concernent les touches déjà lues. Une règle déclenche cette scène lorsqu’une application démarre/se ferme ou qu’un périphérique se connecte/se déconnecte. Les scènes et règles sont enregistrées dans le dossier de données du service, indépendamment du navigateur.

Les notifications Windows pilotent ces règles et les changements de sortie. Le service pousse les états aux pages PC/mobile, au lieu de leur faire interroger le hub toutes les trois secondes. Les lectures USB de batterie restent espacées de douze secondes, car tous les appareils ne transmettent pas spontanément leurs changements.

**Ctrl+Alt+F9** et **Ctrl+Alt+F10** changent la sortie audio Windows précédente/suivante, même lorsque la page est fermée, tant que le service tourne. Le panneau signale un raccourci occupé par un autre logiciel.

Si Windows refuse les notifications de processus sans élévation, le service utilise les événements de fenêtres et les notifications de fin de processus. Cette solution peut détecter l’ouverture de CS2 ou Discord, mais pas tous les démarrages de processus sans fenêtre. Le panneau affiche le mode réellement disponible. Si une scène échoue, les actions suivantes s’arrêtent et l’historique indique les actions déjà confirmées.
