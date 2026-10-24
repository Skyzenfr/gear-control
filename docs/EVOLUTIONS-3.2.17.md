# Gear Control 3.2.17 — autonomie manuelle et barre latérale épurée

## Indiquer l’autonomie annoncée par le fabricant

Certains périphériques (une souris qui ne perd que quelques pour cent par heure, par exemple) pouvaient rester longtemps sur « Batterie restante · Estimation en cours… ». Il fallait attendre une baisse mesurable de la batterie.

Le nouvel onglet **Réglages généraux → Autonomie** liste chaque périphérique qui a une batterie. Pour chacun, vous pouvez saisir l’autonomie annoncée par le fabricant, batterie pleine, en heures (par exemple `70` ou `95,5`).

- L’estimation apparaît aussitôt : niveau de batterie × autonomie indiquée.
- La valeur réelle reste prioritaire. Gear Control passe automatiquement à la mesure dès qu’il observe la décharge pendant la session, ou qu’il la connaît grâce aux sessions précédentes. L’ordre est : mesure de la session, historique des sessions, votre valeur, puis la valeur intégrée pour quelques modèles.
- Chaque ligne indique la source utilisée (« Mesure réelle utilisée », « Votre valeur est utilisée »…), le temps restant et le niveau de batterie.
- La croix efface la valeur saisie. Valeurs acceptées : 0,5 à 1 000 h, arrondies à la demi-heure.
- Les valeurs sont enregistrées sur ce PC (`battery-runtimes.json`) et se règlent uniquement depuis le PC.
- L’onglet est aussi accessible depuis la recherche (Ctrl K) : « Autonomie des périphériques ».

## Barre latérale

- Les mentions « Indépendant des logiciels fabricant » et « Réglages de ce PC » laissent place à des crédits discrets : **Créé par Skyzen**, avec la version installée.

## Vue d’ensemble

- La phrase « Glissez les poignées pour réorganiser » a été retirée. Les poignées restent visibles sur chaque carte.
- Dans le menu **Affichage**, les indicateurs « N autres plus bas / plus haut » sont remplacés par une petite pastille « +N », comme dans la liste des périphériques de la barre latérale, avec un fondu sur le bord de la liste.
