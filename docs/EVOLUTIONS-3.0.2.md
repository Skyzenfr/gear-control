# Gear Control 3.0.2 — Détection et cohérence de l’interface

- **INZONE Buds** : une fiche ajoutée quand le récepteur est présent mais les écouteurs endormis est associée à leur identité vérifiée au réveil. Une identité déjà vérifiée est conservée pendant une indisponibilité sur ce même récepteur. Un numéro de série différent reste une identité distincte. Aucune commande n’est autorisée lorsque les écouteurs sont indisponibles.
- **Découverte Windows** : les interfaces clavier avec suffixe `KBD` ou souris `MOU` sont rattachées à leur conteneur Windows. Les interfaces HID déjà représentées par un appareil audio du même conteneur n’apparaissent plus comme une seconde fiche. Les anciennes fiches correspondantes sont migrées. Le regroupement utilise les associations physiques Windows ; deux appareils portant le même nom restent distincts.
- **Accueil PC et mobile** : le bandeau de prise en charge est retiré. Les capacités et limites restent consultables dans l’écran d’ajout et de gestion.
- **Visuels** : espaces dédiés dans les formulaires de périphérique et de raccourci. Dans un panneau étroit, le lien et son bouton passent sur deux lignes.
- **Affichage** : un indicateur cliquable signale les appareils restant plus haut ou plus bas dans la liste. La barre de défilement suit le thème.
- **Menus de sélection** : les formulaires de périphérique, d’action épinglée et de profil clavier utilisent le composant du site. Le menu passe au-dessus des panneaux, conserve la navigation clavier, désactive les options indisponibles et rend le focus au bouton avec Échap.

Les vérifications automatisées couvrent la veille et la reconnexion Sony, les doublons physiques, la migration des préférences, la navigation des menus et les indicateurs de défilement. Les essais natifs de découverte restent en lecture seule.
