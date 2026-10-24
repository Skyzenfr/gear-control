# Gear Control 3.1 — Stream Deck Mini

## Configurer les six touches

Le Stream Deck Mini original, USB **0FD9:0063**, possède désormais un adaptateur natif. Une fiche auparavant identifiée par Windows est migrée en conservant son nom, son visuel, son appartenance à Gear Control, sa visibilité et son ordre. Les autres modèles Stream Deck restent limités à leur détection ; le protocole Mini est différent du protocole des modèles récents.

1. Installez la version 3.1.0 et rechargez la page avec **Ctrl + F5**.
2. Sélectionnez **Stream Deck Mini**, puis **Configurer**.
3. Fermez le logiciel Elgato pour cet appareil et cochez **Utiliser avec Gear Control**.
4. Cliquez sur chacune des six touches pour choisir son nom, sa couleur, son image et son action.
5. Cliquez sur **Appliquer**. Seule cette validation transmet les écrans et la luminosité au périphérique. La modification d’un brouillon n’exécute aucune action.

Les touches proposent les commandes compatibles déjà exposées par les adaptateurs Gear Control, les scènes et profils enregistrés, lecture/pause/piste précédente/piste suivante du lecteur actif, et une combinaison clavier. Les commandes booléennes basculent depuis leur état actuel ; les choix à plusieurs valeurs passent au mode suivant. Chaque action vérifie la cible au moment de l’exécution. Une action devenue indisponible signale son erreur et n’est pas réessayée automatiquement.

Les raccourcis utilisent **Ctrl, Alt, Shift et Win** avec une lettre, un chiffre, **F1–F24** ou une touche de navigation, par exemple `Ctrl + Shift + M` ou `Win + E`. Une pression correspond à une combinaison, sans script ni séquence de texte. Ils agissent sur l’application au premier plan. Windows peut refuser l’injection vers une application disposant de droits plus élevés.

Les images sont préparées en **80 × 80 pixels** avec conservation des proportions et centrage. Le texte est intégré au visuel transmis. Pour les liens refusant l’export de leurs pixels, importez le fichier PNG. La dernière touche exécutée et son résultat sont affichés dans le configurateur.

## Sauvegarde et coexistence

`stream-deck-mini.json` conserve la configuration par identité. Après une activation explicite, elle est restaurée à la reconnexion ou au redémarrage du service tant que l’appareil reste ajouté. Retirer l’appareil empêche ses touches d’exécuter des actions. Une touche déjà maintenue à la prise de contrôle n’est pas exécutée automatiquement ; relâchez-la avant d’appuyer à nouveau.

Décochez **Utiliser avec Gear Control**, puis **Appliquer**, pour libérer le périphérique. Le logiciel Elgato peut ensuite reprendre ses propres écrans et profils. Les profils et plugins Elgato ne sont pas importés ni exécutés par ce configurateur. La configuration des touches et des raccourcis reste locale au PC ; la télécommande mobile ne peut pas la modifier.

Le protocole accuse réception des transferts USB, sans permettre de relire les pixels affichés ou la luminosité effective. L’interface distingue cet envoi d’une vérification physique des écrans. En cas d’échec partiel, les touches sont désarmées et un nouvel **Appliquer** est nécessaire. Un fichier de configuration illisible n’active pas l’appareil et est conservé séparément s’il est remplacé explicitement.

## Vérification et références

Les tests couvrent les rapports BMP, la rotation et les couleurs, les limites de données, la validation des raccourcis, la migration d’inventaire, la sauvegarde, les fronts d’appui, la désactivation, les erreurs USB et l’éditeur. La couche clavier Windows est compilée sans envoyer de touches. L’identification native du Mini de développement a confirmé son numéro de série et le firmware **3.00.000** en lecture seule. Les essais d’écriture des écrans et d’exécution des raccourcis utilisent des fixtures ; aucune configuration personnelle n’est appliquée automatiquement pendant la livraison.

Références primaires : [protocole HID Mini d’Elgato](https://docs.elgato.com/streamdeck/hid/mini/) et [implémentation de référence Gen1 de node-elgato-stream-deck](https://github.com/Julusian/node-elgato-stream-deck/tree/main/packages/core/src/services).
