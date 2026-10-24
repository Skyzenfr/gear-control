# Gear Control 3.2 — images et contrôle des périphériques

## Visuels et raccourcis

Dans **Réglages des périphériques**, sélectionnez l’appareil puis **Modifier le visuel**, directement dans le panneau de commandes. Choisissez un PNG ou un lien, puis **Enregistrer le visuel**. **Supprimer le visuel** suivi de l’enregistrement rétablit l’image automatique du modèle. **Annuler** conserve l’image précédente. Cette présentation se modifie même si l’appareil est débranché ; elle ne transmet aucune commande au matériel. Le cadrage, le redimensionnement proportionnel et le centrage automatiques restent appliqués partout.

La même suppression est disponible dans la gestion des appareils et dans le configurateur du raccourci épinglé. Une erreur d’enregistrement conserve le brouillon et le visuel publié précédent.

L’action épinglée utilise maintenant l’état confirmé de sa fonction : l’écoute d’entrée désactivée affiche un haut-parleur barré ; une sourdine active affiche le son ou le microphone barré. L’icône suit aussi les changements externes après actualisation. Un état indisponible affiche une information, sans inventer un état actif. Le visuel personnalisé reste indépendant de cet indicateur.

Le **Stream Deck Mini original** affiche son configurateur à six touches dans **Réglages**, ainsi que dans la vue d’ensemble. Les commandes et raccourcis clavier continuent à nécessiter l’activation explicite de sa configuration.

## Communication matérielle

Les nouveaux adaptateurs utilisent les interfaces HID du pilote Windows via `node-hid`. Ils ne remplacent pas les pilotes installés et ne s’appuient pas sur les seules fiches Windows. Ils échangent des commandes propriétaires sur les modèles ou protocoles reconnus. La présence d’un pilote ne donne pas automatiquement accès à tous ses réglages : les commandes non implémentées restent dans l’outil du fabricant.

| Matériel | Lectures et commandes disponibles | Limites |
| --- | --- | --- |
| **Razer DeathAdder V4 Pro**, `1532:00BE` filaire et `1532:00BF` récepteur | Batterie, charge, firmware ; DPI 100–45 000 sur les deux axes ; fréquence 125/500/1 000/2 000/4 000/8 000 Hz ; veille 60–900 s. Chaque réglage doit répondre avant d’être proposé. | Boutons, macros, profils et mises à jour restent dans Synapse. Les DPI et la fréquence concernent le réglage embarqué. |
| **Logitech HID++ 2**, USB direct ou récepteur pris en charge | Batterie ; DPI standard ou X/Y indépendants et LOD si annoncés ; fréquences USB annoncées ; choix explicite du mode hôte/embarqué si exposé. | Pas de compatibilité accordée à toute la marque. HID++ 1, Bluetooth et variantes non exposées ne sont pas couverts. Les profils flash, macros et boutons ne sont pas réécrits. |
| **Wooting RGB**, modèles et PID modernes explicitement reconnus dans le catalogue | Couleur uniforme et restitution du profil lumineux Wootility. L’appareil n’est ouvert qu’après une commande explicite. | Éclairage uniquement : activation, Rapid Trigger, remappage et profils analogiques restent dans Wootility. Les couleurs envoyées ne peuvent pas être relues. |
| **ATK A9 Ultra / MAD68 HE** | Les commandes natives existantes sont conservées : DPI, fréquence et fonctions du capteur compatibles ; activation par touche et Rapid Trigger du MAD68. | Les autres modèles ATK ne reçoivent pas automatiquement le protocole A9 Ultra. |
| **Sony, G FITS, DualSense, audio Windows, Stream Deck Mini** | Adaptateurs existants conservés. | Leurs limites et dépendances restent celles de leur documentation. |

Les pas DPI suivent désormais les limites de chaque adaptateur. La règle ATK de 50 DPI au-delà de 10 000 ne s’applique plus à une Razer ou à une Logitech. Le diagnostic n’affiche pas de fonctions spécifiques ATK sur les autres marques.

## Utilisation sur un autre PC

1. Installez **3.2.0**, branchez l’appareil et lancez **Ajouter un périphérique → Actualiser**.
2. Ajoutez sa fiche. Une ancienne fiche HID générique est migrée lorsque l’association physique est prouvée, en conservant nom, image, visibilité et ordre. Un récepteur partagé entre plusieurs appareils n’est pas attribué arbitrairement à l’un d’eux.
3. Ouvrez **Réglages des périphériques**. Seules les fonctions compatibles sont proposées. Réveillez une souris sans fil si sa fiche ne répond pas. Un autre logiciel constructeur peut monopoliser le transport ou réappliquer ses propres paramètres.
4. Modifiez un réglage puis **Appliquer**. Razer et Logitech doivent confirmer leur réglage par relecture. En mode embarqué Logitech, activez d’abord le contrôle logiciel si le périphérique le nécessite ; le changement n’est jamais effectué implicitement.
5. Pour Wooting, choisissez la couleur et appliquez-la. L’interrupteur représente la prise en charge par la session Gear Control, pas une mesure des couleurs affichées. Désactivez-le puis appliquez pour rendre l’éclairage à Wootility. Le retrait de l’appareil et l’arrêt du service tentent également de restituer uniquement une session précédemment prise en charge. Une perte USB peut empêcher cette restitution.

La découverte, l’ajout et le démarrage ne changent aucun DPI, fréquence ou éclairage. Le RGB Wooting n’est pas réappliqué au redémarrage. Les profils et automatisations déjà activés par l’utilisateur conservent leur fonctionnement.

## Vérification

Les tests couvrent les trames et limites, les réponses incohérentes, la validation du correctif complet, les identités remplacées, les récepteurs Logitech, la migration d’inventaire, le retrait, les échecs et la relecture après écriture. Une opération native expirée reste bloquée tant que son appel sous-jacent n’est pas terminé ; aucune écriture n’est répétée automatiquement.

Les parcours UI vérifient les brouillons d’images, l’annulation, l’enregistrement, la suppression hors connexion, les erreurs, la propagation dans les cartes et la barre latérale, les icônes d’état et l’accès au configurateur Stream Deck. Le RGB indique une confirmation de transport, jamais une relecture matérielle des pixels.

**Les nouveaux protocoles sont validés sur transports simulés. Aucun exemplaire DeathAdder V4 Pro, Logitech HID++ ou Wooting n’est branché sur le PC de développement. Aucun firmware de ces modèles n’est déclaré testé physiquement.** La validation sur les périphériques de vos amis reste nécessaire, en particulier pour les révisions, modes de connexion et coexistences avec les outils constructeurs.

### Parcours à vérifier sur le matériel

- Personnaliser une image, annuler, enregistrer, supprimer, puis vérifier le même appareil hors connexion.
- Activer/désactiver l’action PS5 et modifier son état dans Windows : texte et icône doivent rester cohérents.
- Sur la DeathAdder V4 Pro, comparer batterie/DPI/fréquence avec Synapse, appliquer une valeur, puis vérifier sa valeur dans Synapse. Essayer filaire, récepteur et veille/reconnexion.
- Sur Logitech, vérifier les fonctions annoncées, le mode embarqué/hôte et les axes indépendants si proposés ; confirmer les modifications dans l’outil constructeur.
- Sur Wooting, appliquer une couleur, rendre la main à Wootility, tester retrait et arrêt ; vérifier qu’un redémarrage ne reprend pas l’éclairage automatiquement.
- Vérifier que les anciens appareils gardent leur nom, leur image et leur ordre, et que leurs commandes existantes fonctionnent encore.

Références et détails techniques : [Razer](RAZER-PROTOCOL.md), [Logitech](LOGITECH-PROTOCOL.md), [Wooting](WOOTING-PROTOCOL.md), [Stream Deck Mini](EVOLUTIONS-3.1.md).
