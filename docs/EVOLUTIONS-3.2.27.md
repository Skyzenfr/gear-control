# Gear Control 3.2.27 — sons de confirmation

## Un retour sonore discret

L’interface du PC joue un son court et doux quand le PC confirme une commande que vous avez lancée :

- **Confirmation** (deux notes montantes) : des réglages appliqués, l’égaliseur enregistré ou un profil activé.
- **Bascule** (une note brève) : une sortie audio Windows choisie, une action épinglée exécutée, un réglage rapide appliqué. Après un changement de sortie, le son est joué sur la nouvelle sortie, ce qui confirme que le son y passe bien.
- **Échec** (deux notes graves descendantes) : l’une de ces commandes est refusée ou n’est pas confirmée par le périphérique.

## Ce qui reste silencieux

- La navigation, le survol, les curseurs (volume, égaliseur) et la préécoute de l’égaliseur.
- Le contrôle du bruit des écouteurs : ils annoncent déjà eux-mêmes le changement de mode.
- Les changements arrivés d’ailleurs : raccourcis clavier, télécommande, automatisations.
- La télécommande mobile.
- Un onglet en arrière-plan.

## Réglage

**Réglages généraux → Apparence → Sons** active ou coupe les sons, et l’activation fait entendre le son de confirmation. Les sons sont activés par défaut. Le choix est enregistré dans le navigateur.

Les sons sont synthétisés par le navigateur : aucun fichier audio n’est ajouté. Leur niveau reste bas (environ −26 dBFS), sous la musique ou un jeu. La session audio du navigateur est libérée environ 1,5 s après chaque son. Gear Control ne reste donc pas dans le mélangeur de volume de Windows.
