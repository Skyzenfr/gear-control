# Gear Control 3.0.3 — Ajustement automatique des visuels

Les visuels personnalisés sont ajustés comme les illustrations intégrées : les marges transparentes et les bordures blanches d’une photo produit sont recadrées, puis le produit est redimensionné et centré sans déformation. Une petite marge conserve ses contours. Le même traitement est utilisé dans les cartes PC et mobile, la fiche de l’appareil, les listes, l’aperçu et l’action épinglée.

Les personnalisations existantes bénéficient aussi de cet ajustement. Il n’est pas nécessaire de réimporter les fichiers. L’image originale ou son lien reste enregistré ; le traitement concerne uniquement l’affichage. Les grandes images sont analysées sur une version réduite puis affichées à une résolution adaptée, avec un cache partagé entre les emplacements.

Les PNG importés peuvent être recadrés localement. Pour un lien distant, le recadrage des marges dépend de l’autorisation de lecture de l’image par son site. Si elle est refusée, l’image reste affichée à sa taille adaptée, centrée, avec ses proportions et ses marges originales. Aucun téléchargement d’image par le serveur Gear Control n’est ajouté.

Les tests couvrent les produits décentrés, les formats verticaux et horizontaux, les bordures blanches, la transparence, les images vides, la réduction proportionnelle, les liens refusant le traitement et les réponses arrivant après un changement d’image ou la fermeture du panneau.
