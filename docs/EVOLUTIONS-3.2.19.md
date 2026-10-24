# Gear Control 3.2.19 — cohabitation avec le logiciel Elgato

## Le problème

Quand le logiciel Elgato Stream Deck et Gear Control envoyaient des images au Stream Deck Mini en même temps, leurs paquets USB se mélangeaient. Le firmware du Mini se bloquait et redémarrait en boucle (écrans qui clignotent). Il pouvait entraîner dans sa chute le hub USB sur lequel il est branché, et donc tous les périphériques branchés au même endroit (souris comprise).

Deux situations déclenchaient ce conflit :

- **Au démarrage de Windows**, Gear Control prenait la main sur le Stream Deck avant même de vérifier si le logiciel Elgato était ouvert.
- **Avec un script qui lance `StreamDeck.exe` au démarrage** alors qu’Elgato est déjà lancé par Windows : la seconde copie se ferme aussitôt. Gear Control croyait alors le logiciel Elgato fermé et renvoyait ses visuels pendant qu’Elgato continuait d’écrire.

## Ce qui change

- Tant que `StreamDeck.exe` tourne, Gear Control **lâche complètement le Stream Deck** : il ne garde aucune connexion ouverte et n’envoie plus rien. La fiche du périphérique l’indique.
- Gear Control ne reprend le Stream Deck qu’après avoir vérifié qu’**aucune** copie de `StreamDeck.exe` ne tourne encore. Il renvoie alors ses visuels, sa luminosité et réactive ses actions automatiquement.
- Si le logiciel Elgato démarre pendant un envoi de visuels, Gear Control s’arrête après la touche en cours et n’affiche pas d’erreur durable.
- Au démarrage, si le logiciel Elgato est installé, Gear Control attend 90 secondes avant de prendre le Stream Deck, le temps qu’un lancement automatique d’Elgato se fasse.
- **Appliquer** une configuration Stream Deck alors que le logiciel Elgato est ouvert est refusé avec un message clair, au lieu d’écrire en même temps que lui.

Vous pouvez donc ouvrir le logiciel Elgato quand vous voulez, y compris au démarrage de Windows, sans risque pour le Stream Deck ni pour les autres périphériques branchés sur le même hub.
