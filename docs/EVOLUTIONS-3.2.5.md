# Gear Control 3.2.5 — médias, Deezer / Apple Music et Stream Deck plus réactif

## Lecteur affiché en double

**Symptôme :** le sélecteur de la carte média proposait deux fois « Spotify · Tu connais ».

**Cause :** Windows peut renvoyer, pour la session média actuelle, un objet différent de celui de la liste des sessions. Gear Control comparait ces objets par identité et les considérait comme deux lecteurs. Certains lecteurs publient aussi réellement la même lecture deux fois.

**Correction :**

- La session actuelle est rapprochée de la session listée de la même application, au lieu d’être ajoutée une seconde fois.
- Quand Windows remplace l’objet d’une session, son identifiant et son morceau sont conservés. La sélection et le déplacement dans le morceau ne sont plus perdus.
- Deux sessions de la même application qui affichent exactement le même morceau sont fusionnées. Gear Control garde la session actuelle de Windows, sinon celle en lecture.
- Le premier choix du sélecteur indique l’application active, par exemple « Lecteur actif · Spotify ».

## Deezer et Apple Music

- Deezer, Apple Music et iTunes ont un nom propre et leur logo dans la carte média, la barre supérieure et le mélangeur audio. Les navigateurs reconnus (Chrome, Edge, Firefox…) affichent aussi leur logo.
- Le contrôle du PC propose **Ouvrir Deezer** et **Ouvrir Apple Music** quand l’application est installée (version classique ou Microsoft Store).
- L’onglet média de la télécommande s’appelle désormais **Musique**.

**Limite :** l’ajout aux favoris reste réservé à Spotify. Deezer n’accepte plus de nouvelles applications sur son API, et Apple Music exige un jeton développeur payant. La lecture, la pause, les pistes et la position fonctionnent avec tous les lecteurs compatibles avec les contrôles multimédias de Windows.

## Fond des touches du Stream Deck

Le simple sélecteur de couleur est remplacé par :

- un aperçu de la touche avec la couleur du texte réellement utilisée sur l’écran ;
- un champ de code hexadécimal (`#1DB954`, ou la forme courte `#1D5`) ;
- des teintes classées par groupe : Neutres, Couleurs et Marques (Spotify, Deezer, Apple Music, Discord, Twitch, YouTube) ;
- la couleur personnalisée et les couleurs déjà utilisées dans le profil ;
- **Appliquer ce fond aux 6 touches de la page**.

## Délai à l’appui sur une touche

- **Lecture multimédia :** l’état des lecteurs envoyé par Windows est réutilisé. Gear Control ne relit plus toutes les sessions et leurs pochettes avant chaque appui. Lecture / pause est décidé par Windows au moment de l’appui, à partir de l’état réel du lecteur.
- **Raccourcis clavier :** le module clavier démarre dès qu’une touche configurée en a besoin. Avant, le premier appui attendait la compilation de ce module, soit une à trois secondes.
- **Commandes Gear Control :** chaque appui déclenchait trois relectures complètes de tous les périphériques. La relecture finale, en double avec celle de la commande, est supprimée.
- **Changement de page :** seules les touches dont l’image change sont renvoyées. La liste des périphériques USB n’est plus relue, et la luminosité n’est renvoyée que si elle change.
- Les raccourcis, ouvertures de logiciel et commandes multimédias ne déclenchent plus de relecture de tous les périphériques après l’appui.
