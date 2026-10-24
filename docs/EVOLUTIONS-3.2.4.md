# Gear Control 3.2.4 — service bloqué avec le Stream Deck actif

## Symptôme

Après activation de **Utiliser avec Gear Control** sur le Stream Deck Mini, le hub restait inaccessible (`ERR_CONNECTION_REFUSED`) et le menu indiquait « Service en cours de connexion… ». Le lanceur relançait le service toutes les deux à trois minutes, sans succès.

## Cause

L’adaptateur Stream Deck utilisait les appels HID **synchrones** de `node-hid` : ouverture, lecture des rapports de fonction, écriture des 120 paquets d’image et luminosité. Ces appels s’exécutent sur le fil principal de Node. Pendant que les adaptateurs de la manette et du clavier/souris utilisaient en parallèle l’API HID asynchrone, ce fil principal restait bloqué dans le code natif. Tout le service était alors figé : le port 3210 ne s’ouvrait pas et l’inspecteur Node ne pouvait même pas suspendre le processus.

Le diagnostic a été fait sur une copie des données : le blocage se produisait toujours dans le premier rafraîchissement des périphériques avec le Stream Deck actif, et jamais avec le Stream Deck désactivé.

## Correction

- L’adaptateur Stream Deck n’utilise plus que l’API asynchrone : `devicesAsync`, `HIDAsync.open` en mode non exclusif, puis des rapports et écritures attendus. Le service démarre en environ 9 secondes, Stream Deck actif, et reste réactif.
- Le numéro de série reste vérifié avant chaque touche, mais la liste complète des périphériques HID n’est plus relue que pour la première, ce qui réduit le temps d’envoi des visuels.
- Le menu de la zone de notification affiche désormais la version installée : la version du lanceur était figée à 3.2.1.

## Limite connue

Les adaptateurs G FITS et INZONE ainsi que la détection audio Windows utilisent encore des appels HID synchrones. Aucun blocage n’a été observé avec eux, mais leur passage à l’API asynchrone reste à faire.
