# Contrôle Logitech HID++ 2

## Portée réelle

`app/server/adapters/logitech-hidpp.mjs` communique directement avec les collections HID++ exposées par le pilote HID Windows. Il ne dépend pas de G HUB et n'installe/remplace pas de pilote. G FITS conserve son adaptateur spécifique ; son PID `046D:0A7B` est explicitement exclu de cet adaptateur.

Le protocole est auto-descriptif : chaque appareil doit répondre au ping HID++ 2, fournir ses identifiants matériel/unité et son nom, puis annoncer et répondre à chaque fonction. Un nom de marque ou une entrée du catalogue ne suffit jamais à autoriser une écriture.

Fonctions implémentées, uniquement lorsqu'elles sont confirmées par les réponses du périphérique :

| Fonction HID++ | Lecture | Contrôle explicite |
| --- | --- | --- |
| `0000` Root | Version avec challenge, indices de fonctions | Aucun |
| `0003`, `0005` | Identifiant d'unité, identifiant de modèle, nom et type | Aucun |
| `1000`, `1004` | Pourcentage annoncé et état de charge | Aucun |
| `2201` | DPI, valeur par défaut et liste/plage DPI autorisée | DPI du capteur 0 |
| `2202` | DPI X/Y, valeurs par défaut, plages et LOD si annoncée | DPI X/Y et LOD en conservant les champs non modifiés |
| `8060` | Liste et fréquence courante | Fréquence en millisecondes |
| `8061` | Liste et fréquence courante | Fréquence étendue, jusqu'à 8 000 Hz si annoncée |
| `8100` | Mode hôte/embarqué | Changement explicite du mode, sans réécriture des profils |

Les macros, le remappage des boutons, les effets RGB, les profils flash, la mise à jour du firmware et HID++ 1 ne sont pas implémentés par ce nouvel adaptateur. Les DPI/fréquences en mode embarqué sont bloqués jusqu'au choix explicite du mode hôte. Désactiver le mode hôte réactive le profil embarqué existant. G HUB ou un autre outil peut réécrire les paramètres : un succès nécessite une relecture conforme immédiatement après la commande.

Les récepteurs documentés `C52B/C531/C532/C534/C537/C539/C53A/C53D/C53F/C541/C547/C548/C54D` sont interrogés en lecture sur les emplacements 1 à 6. Les autres transports USB HID++ sont interrogés à l'adresse directe `FF`. Bluetooth `FF43`, les collections non exposées et les variantes nécessitant des particularités de protocole supplémentaires restent hors de cette implémentation.

## Transport et protections

- Collections Windows `046D`, page `FF00`, usages 1 et 2. Les chemins Windows sont regroupés uniquement lorsqu'ils partagent le même identifiant d'instance PnP et interface, en retirant les champs de collection ; les numéros de série réutilisés ne provoquent aucune fusion de transports.
- Les rapports longs sont privilégiés lorsqu'ils sont disponibles ; les réponses sont écoutées sur les deux collections ouvertes.
- Requêtes sérialisées, identifiant logiciel non nul, vérification de l'adresse, de l'indice et de la fonction ; les événements et réponses d'autres logiciels sont ignorés.
- Ouverture, réponse et fermeture bornées. Une commande expirée n'est pas réessayée. Une réponse USB ne termine pas la requête tant que l'écriture native n'est pas terminée ; si celle-ci dépasse le délai, le transport physique reste en quarantaine jusqu'à sa terminaison, sans ouverture d'un canal de remplacement. Un ping d'emplacement vide peut expirer sans empêcher la lecture des emplacements suivants.
- Aucune modification de DPI, fréquence ou mode pendant la découverte, l'ajout, l'actualisation ou le démarrage.
- Avant une commande : présence dans Gear Control, relecture des fonctions et de l'identité physique, relecture des plages, validation de **tout** le correctif avant la première mutation. Avant **chaque** mutation du lot, les indices de fonctions et l'identifiant d'unité/modèle sont relus ; l'appartenance est revérifiée immédiatement avant l'écriture.
- Après la commande : relecture des valeurs et nouvelle vérification de l'identité avant publication du résultat. Un changement d'appareil appairé interrompt le lot, sans réessai. Une déconnexion supprime les capacités et réglages périmés.
- Les alias de chemins permettent la migration des fiches HID génériques. Ils sont supprimés lorsque plusieurs périphériques utilisent le même récepteur pour éviter d'attribuer une fiche au mauvais appareil.

## Sources primaires consultées le 16 octobre 2026

1. [Documentation Logitech officielle : structure HID++ 2](https://github.com/Logitech/cpg-docs/blob/master/hidpp20/README.rst).
2. [Documentation Logitech officielle : IRoot](https://github.com/Logitech/cpg-docs/blob/master/hidpp20/features/0x0000-IRoot.rst).
3. [Implémentation originale libratbag, fonctions `2201`, `8060`, `8100`](https://github.com/libratbag/libratbag/blob/master/src/hidpp20.c). La commande standard `2201/getSensorDpiList` reçoit seulement l'indice du capteur, sans pagination inventée.
4. [Implémentation originale Solaar des réglages, DPI étendus et fréquences étendues](https://github.com/pwr-Solaar/Solaar/blob/master/lib/logitech_receiver/settings_templates.py).
5. [Implémentation originale Solaar des identités et batteries](https://github.com/pwr-Solaar/Solaar/blob/master/lib/logitech_receiver/hidpp20.py).
6. [Analyse originale des collections Windows et captures Onboard Memory Manager](https://github.com/Java-Idl/universal-omm-webhid).
7. [Suivi original des récepteurs Lightspeed OpenLogi](https://github.com/AprilNEA/OpenLogi/issues/512).

L'implémentation JavaScript est indépendante ; les trames et limites ont été reconstituées à partir des interfaces publiques et de leurs implémentations d'origine. Aucun code de SDK ni binaire propriétaire n'est incorporé.

## Validation

`node --test server/adapters/logitech-hidpp.test.mjs` : 19 tests de trames, découverte sans mutation, plages compressées, DPI standard et étendu, préservation Y/LOD, fréquences jusqu'à 8 kHz, mode hôte explicite, récepteur avec emplacement ultérieur occupé, changement d'unité appairée avant le lot, entre deux mutations ou pendant la relecture, retrait, déconnexion, erreurs et délais natifs, écriture native bloquée malgré une réponse reçue, et absence de réouverture pendant sa quarantaine. Ces tests utilisent un transport simulé complet ; aucun changement matériel n'a été effectué. Aucun firmware Logitech supplémentaire ne doit être annoncé comme testé physiquement avant une vérification sur l'appareil concerné.
