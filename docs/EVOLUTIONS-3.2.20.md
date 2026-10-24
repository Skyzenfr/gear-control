# Gear Control 3.2.20 — priorité au logiciel du fabricant

## Le principe

Quand le logiciel officiel d’un fabricant est ouvert, c’est lui qui pilote le périphérique. Gear Control se met en retrait :

- il ne lit plus rien et n’envoie plus aucune commande au périphérique ;
- il ferme ses connexions USB avec lui, sans rien y écrire (pas même la restitution d’un éclairage) ;
- dès que le logiciel est fermé, Gear Control reprend la main automatiquement, sans redémarrage.

Deux programmes qui échangent en même temps avec le même périphérique USB se perturbent. Dans le pire des cas, le périphérique se bloque et peut entraîner le hub USB sur lequel il est branché.

## Logiciels reconnus

| Logiciel | Périphériques concernés | Processus surveillés |
| --- | --- | --- |
| Elgato Stream Deck | Stream Deck Mini | `StreamDeck.exe` |
| Logitech G HUB | G FITS et périphériques Logitech HID++ | `lghub.exe`, `lghub_agent.exe` |
| Razer Synapse | Souris Razer | `Razer Synapse 3.exe`, `Razer Synapse.exe`, `RazerAppEngine.exe` |
| Wootility | Claviers Wooting | `Wootility.exe`, `Wootility Lekker.exe` |
| Sony INZONE Hub | INZONE Buds | `INZONEHub.exe` |

Les services d’arrière-plan installés avec ces logiciels (mises à jour, services Windows) ne sont pas pris en compte : seul le logiciel qui pilote réellement le matériel l’est. Le réglage ATK/FGG se fait dans un navigateur, Gear Control ne peut donc pas le détecter.

La détection est immédiate à l’ouverture ou à la fermeture du logiciel, et revérifiée à chaque actualisation. Si plusieurs copies du logiciel sont lancées, Gear Control attend que la dernière soit fermée.

## Dans l’interface

- Le périphérique passe en **orange**, dans la barre latérale comme dans la grille, avec le nom du logiciel ouvert.
- Un bandeau orange explique que le logiciel du fabricant a la priorité et qu’il faut le fermer, y compris depuis la zone de notification, pour modifier les réglages.
- Le badge de la fiche affiche **LOGICIEL FABRICANT OUVERT**.
- Tous les réglages restent visibles avec leurs dernières valeurs connues, mais **grisés et inactifs** : réglages rapides, réglages détaillés, configurateur Stream Deck, égaliseur, contrôle du bruit.
- La batterie n’est plus affichée pendant ce temps, puisqu’elle n’est plus lue.
- Le nom du périphérique reste modifiable : il ne concerne que Gear Control.

Si le logiciel du fabricant était déjà ouvert au démarrage de Gear Control, le périphérique apparaît directement verrouillé, avec ses derniers réglages enregistrés. Il doit pour cela avoir été vu au moins une fois par Gear Control 3.2.20.

Toute commande envoyée malgré tout (depuis la télécommande, une action épinglée ou un profil automatique) est refusée par le service avec le même message.
