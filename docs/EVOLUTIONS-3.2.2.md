# Gear Control 3.2.2 — Stream Deck et MAD68 HE RGB

## Stream Deck Mini

**Reprise de la configuration Elgato.** Quand Gear Control n’a encore aucune configuration pour un Stream Deck, il reprend automatiquement dans le brouillon le profil Elgato lié à cet appareil, sur la page qu’Elgato affiche. Le profil est reconnu grâce au numéro de série enregistré par Elgato (`Device.UUID`), qui n’est jamais transmis à l’interface. Le brouillon reste désactivé : cochez **Utiliser avec Gear Control**, puis **Appliquer**.

**Pages.** Chaque page apparaît sous forme de vignette avec ses six touches. Des flèches entourent l’appareil, et des points indiquent la page affichée. Les pages peuvent être ajoutées (16 au maximum), renommées et supprimées. Une touche qui pointait vers une page supprimée repasse sur « Aucune action ».

**Animations.** Au changement de page, les touches entrent depuis le côté du déplacement, l’une après l’autre. Elles se soulèvent au survol, s’enfoncent au clic, et la touche sélectionnée porte un halo. Un appui confirmé sur le Stream Deck physique fait briller la touche correspondante. Un badge indique le type d’action de chaque touche. Les animations sont coupées lorsque Windows demande de réduire les animations.

**Ouvrir un logiciel.** Cette action est disponible pour toutes les touches. **Parcourir…** ouvre le sélecteur Windows, qui remplit le chemin complet ainsi que le nom et l’icône de la touche s’ils sont vides. Un chemin `.exe` peut aussi être collé à la main. Les scripts, consoles, interpréteurs et arguments restent refusés. Le sélecteur n’est disponible que sur le PC, pas depuis la télécommande.

Les avertissements génériques de l’import Elgato (pages vides, luminosité, multi-actions) ne sont plus affichés. Les touches impossibles à convertir restent signalées dans l’aperçu d’import.

## MAD68 HE RGB, Ultra et Limit

Seul le MAD68 HE Lightless (`373B:1057`) était détecté. Les variantes RGB (`1058`), Ultra (`1059`) et Limit (`105A`, `105C`) utilisent le même contrôleur, la même matrice et la même version de touches avancées. Elles reçoivent maintenant les mêmes réglages : activation par touche, Rapid Trigger, verrouillage Windows et inversion WASD / flèches. Ces variantes n’ont pas été vérifiées sur du matériel ; chaque écriture est relue avant confirmation. L’éclairage RGB reste réglable dans l’outil officiel (voir `INPUT-PROTOCOL.md`).

## Vérification

Les tests couvrent la reprise automatique sur la page affichée par Elgato, l’association par numéro de série, la navigation, l’ajout, le renommage et la suppression de pages, le sélecteur d’application, ainsi que la détection de la variante RGB et l’exclusion des contrôleurs non validés. Le rendu a été vérifié par capture Edge avec un profil Elgato réel ; les animations, elles, ne sont pas vérifiées pendant leur déroulement.
