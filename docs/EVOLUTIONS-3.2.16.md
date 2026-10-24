# Gear Control 3.2.16 — confirmations, actions destructives et OLED partout

## Les actions destructives passent au rouge

Au survol et au focus clavier, les actions qui suppriment, retirent, ferment, déconnectent ou réinitialisent passent au rouge (contour, texte et halo) :

- les croix de fermeture (éditeurs, formulaires, capture du PC, recherche, action épinglée, carte de mise à jour…) ;
- **Supprimer** (profil de jeu, profil audio, page Stream Deck, visuel) et **Retirer de Gear Control**, y compris dans le menu « ⋯ » de la liste des périphériques ;
- **Déconnecter ce PC** (Spotify), **Fermer la caméra**, **Réinitialiser les touches**, **Remettre à zéro** l’égaliseur.

Une règle commune du thème s’en charge. Les boutons dont le libellé commence par « Supprimer », « Retirer », « Fermer » ou « Déconnecter » la suivent automatiquement.

## Une fenêtre de confirmation avant chaque suppression

Les suppressions ouvrent maintenant une fenêtre de confirmation, au lieu de s’exécuter directement ou d’afficher un encadré dans la page. Le bouton **Annuler** a le focus par défaut. Échap ou un clic à côté referment la fenêtre sans rien changer. Sont concernés :

- retirer ou ignorer un périphérique ;
- supprimer un profil de jeu ;
- supprimer un profil audio de l’égaliseur, ou réinitialiser les profils locaux ;
- retirer l’action épinglée ;
- supprimer une page du Stream Deck ;
- supprimer un visuel personnalisé ;
- déconnecter Spotify de ce PC.

## Test audio et balance stéréo

Le module a été redessiné :

- trois grandes touches **Gauche**, **Les deux** et **Droite**. Le canal testé s’allume et pulse pendant le bip. Un second appui arrête le test ;
- la sortie des tests et le bouton **Lister les sorties** sont sur une même ligne ;
- la balance utilise un curseur centré : la partie remplie part du centre vers le côté choisi, avec un repère central. La valeur s’affiche en clair (« 30 % à gauche ») et les boutons sont **Centrer** et **Appliquer**.

## Plus de fonds gris

- Les surfaces secondaires (cartes internes, champs, boutons) sont désormais en noir pur. Elles restaient en gris très sombre dans plusieurs modules : configuration Elgato, sélecteur de couleur du Stream Deck, test audio…
- La télécommande mobile et le scanner de QR code utilisent maintenant le même thème OLED que l’application. Ils avaient leur propre palette grise.
- Les messages d’alerte et d’erreur (fonds marron) passent sur fond noir avec un contour et un texte ambrés.

## Stream Deck allégé

- L’en-tête « SIX TOUCHES · USB / Stream Deck Mini » a été retiré : il répétait le nom de l’appareil affiché juste au-dessus.
- L’action de la touche n’est plus répétée sous « Touche N ». Elle reste visible dans les champs juste en dessous.
- Les points de pagination sous l’appareil ont été retirés : la bande de pages et les flèches indiquent déjà la page affichée.
