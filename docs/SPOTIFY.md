# Favoris Spotify silencieux

Depuis la version 2.6.4, les favoris passent uniquement par l’API officielle Spotify. Gear Control n’ouvre pas Spotify, ne change pas la fenêtre active et ne manipule aucun bouton ni menu de son application. Les commandes de lecture et les métadonnées restent fournies par Windows.

## Connexion initiale

1. Dans [Spotify Developer Dashboard](https://developer.spotify.com/dashboard), sélectionnez votre application, activez **Web API**, puis enregistrez exactement cette adresse de redirection : `http://127.0.0.1:3210/spotify/callback`.
2. Dans Gear Control, ouvrez **Réglages généraux → Spotify**, renseignez son **Client ID** public et cliquez sur **Connecter Spotify**, puis **Autoriser Spotify**.
3. Autorisez votre compte dans l’onglet Spotify et revenez dans Gear Control. Aucun Client Secret n’est requis.

L’application demande uniquement `user-read-currently-playing`, `user-library-read` et `user-library-modify`. Spotify exige actuellement un compte Premium du propriétaire d’une application en mode développement ; les comptes utilisés doivent être autorisés dans cette application. Voir les [conditions du mode développement](https://developer.spotify.com/documentation/web-api/concepts/quota-modes).

Cet avertissement Premium est affiché dans le menu de connexion et concerne l’accès aux **Titres likés** par l’API Spotify. Les commandes de lecture, pause et volume continuent de passer par Windows et ne nécessitent pas cette association. Le menu propose également une copie de l’adresse de redirection à enregistrer dans Spotify Developer.

Les jetons sont conservés dans `spotify-connection.json` dans le dossier de données, chiffrés avec **Windows DPAPI / CurrentUser**. Ils ne sont jamais envoyés au navigateur ni à la télécommande. La configuration et l’association du compte sont accessibles uniquement depuis le PC. **Déconnecter ce PC** supprime ses jetons enregistrés.

## Lectures et ajouts

À l’arrivée d’un titre, le service demande une seule fois le titre courant du compte Spotify, vérifie son nom, son artiste, son album et sa durée lorsqu’ils sont disponibles, puis lit son appartenance aux **Titres likés**. Les sessions Windows dupliquées partageant le même titre utilisent cette même lecture. La progression, lecture/pause, le focus du navigateur et **Actualiser les médias** réutilisent le résultat mémorisé, y compris un résultat inconnu ou un échec. Aucune interrogation périodique des favoris n’est effectuée.

Le cœur ajoute uniquement le titre identifié par son URI Spotify. L’écriture est idempotente et ne retire jamais de favori. Une nouvelle lecture de l’appartenance aux Titres likés confirme cet ajout, puis met à jour le cœur dans le lecteur PC et la télécommande. Les changements de titre pendant la commande sont signalés.

Si le compte Spotify joue un autre titre que la session Windows, si Spotify ne renvoie pas de titre, si l’accès est refusé ou si le réseau échoue, l’état reste **inconnu**. Le service n’ouvre jamais l’application pour contourner cette absence d’information. Une nouvelle connexion ou l’arrivée d’un autre titre autorise une nouvelle lecture. Les modifications effectuées directement dans Spotify pendant le même titre ne déclenchent pas de vérification supplémentaire.

## Références de l’API

- [Autorisation avec PKCE](https://developer.spotify.com/documentation/web-api/tutorials/code-pkce-flow)
- [Titre courant du compte](https://developer.spotify.com/documentation/web-api/reference/get-the-users-currently-playing-track)
- [Vérifier les éléments enregistrés](https://developer.spotify.com/documentation/web-api/reference/check-library-contains)
- [Enregistrer des éléments](https://developer.spotify.com/documentation/web-api/reference/save-library-items)
- [Adresses de redirection](https://developer.spotify.com/documentation/web-api/concepts/redirect_uri)
