# Gear Control 3.0 — Une installation personnelle et partageable

La version 3.0 généralise l’inventaire et les fonctions Windows. La détection d’un appareil ne rend pas son protocole constructeur disponible : les fonctions propriétaires restent limitées aux adaptateurs réellement développés. Le catalogue intégré décrit cette distinction.

## Première installation et découverte

Au premier lancement, l’assistant présente les appareils détectés et permet de choisir ceux à ajouter. Reporter conserve un espace vide utilisable ; la gestion reste accessible par **Ajouter un périphérique** et **Réglages généraux → Périphériques**. Le branchement ultérieur d’un appareil le rend disponible dans **À découvrir**, sans l’ajouter automatiquement à l’accueil.

La découverte regroupe les interfaces HID à partir des conteneurs Windows lorsque l’association est précise. Les appareils identiques ne sont pas fusionnés à partir de leur nom ou de leur numéro de série. Les interfaces sans association fiable restent séparées. Les rôles audio Windows peuvent apparaître comme des fiches distinctes : cela conserve un ciblage explicite de chaque entrée et sortie.

Aucun rapport propriétaire n’est envoyé aux appareils inconnus. Les adaptateurs existants peuvent lire les réglages de leurs modèles reconnus ; rechercher et ajouter un appareil n’applique pas de profil. Les modifications d’inventaire ne sont pas traitées comme des branchements matériels par les automatisations.

## Ajout manuel et gestion

L’utilisateur peut ajouter un appareil détecté, l’ignorer, le retirer de son espace, le renommer et personnaliser sa catégorie, sa description de modèle et sa couleur. Retirer une fiche de Gear Control ne désinstalle aucun périphérique Windows. Les appareils déconnectés restent disponibles.

Une fiche préparée manuellement reste hors connexion et sans commandes. **Associer un appareil** permet de choisir ensuite un appareil réellement détecté et de confirmer l’association. Les contrôles proviennent toujours de l’appareil réel ; nommer une fiche « MAD68 » ou choisir une catégorie ne lui attribue pas un protocole.

Le catalogue se consulte depuis **Compatibilité et diagnostic**. Il présente les modèles exacts, fonctions, transports et limitations. **Préparer une fiche** préremplit uniquement des informations de présentation. Aucune version de firmware n’est annoncée testée sans preuve correspondante.

## Fonctions standard et réglages

Les sorties audio Windows exposent leur volume, sourdine et sélection comme sortie si disponibles. Les entrées actives exposent leur niveau et sourdine. L’écoute d’une entrée est proposée seulement lorsque Windows expose cette propriété ; elle utilise la destination d’écoute déjà configurée dans Windows.

Les réglages Windows ne sont pas présentés comme un égaliseur matériel, un gain analogique ou une modification interne au casque. Les inconnues restent inconnues. Les photos des modèles connus sont conservées ; les autres appareils utilisent une illustration de leur catégorie. Les claviers et manettes génériques ne reçoivent pas le configurateur MAD68 ou DualSense.

La description des capacités distingue source, disponibilité, lecture, écriture, valeur courante et dernière valeur sauvegardée. Les contrôles simples utilisent les descriptions fournies par les adaptateurs. Les contrôles complexes gardent leur interface spécialisée.

Les niveaux et sourdines sont validés puis confirmés par Windows. Les commandes de manette ne garantissent pas une mesure physique de l’éclairage ou de la vibration : leur confirmation dépend du transport et de l’adaptateur. Les délais limitent le blocage de l’interface ; une commande déjà remise au matériel peut être impossible à annuler. Une erreur ne déclenche aucun nouvel essai automatique d’écriture.

L’écoute d’entrée est appliquée séparément et immédiatement. Elle n’est pas capturée automatiquement dans un profil de jeu. L’import de profils vérifie les capacités et les plages décrites de l’appareil destinataire, ainsi que les fréquences de l’égaliseur.

## Action épinglée

La première commande de la barre supérieure est configurable. Ouvrir son panneau ne déclenche rien. Choisissez un appareil ajouté, une action proposée, un nom et une icône, puis enregistrez. Les actions proposées comprennent les bascules booléennes, les modes à valeurs définies, la sélection d’une sortie et l’écoute d’une entrée. Les paramètres numériques arbitraires et les commandes brutes ne sont pas proposés.

La commande rapide applique un état souhaité explicite et attend sa confirmation. Un appareil déconnecté ou une valeur inconnue désactive l’action. Le choix reste enregistré et peut être remplacé ou retiré. Sur mobile, la commande peut être exécutée si elle est autorisée, mais sa configuration se fait sur le PC.

La migration d’une installation antérieure peut conserver **Son PS5** avec l’icône console, uniquement sur l’entrée exacte précédemment identifiée. Elle ne change pas son état d’écoute. Les nouvelles installations n’utilisent aucune entrée Realtek par défaut.

## Données et distribution

`device-inventory.json` conserve l’inventaire, les préférences de présentation et l’ordre des cartes. Une sauvegarde `.bak` sert à récupérer un inventaire valide ; un fichier corrompu est conservé séparément. Les noms et couleurs existants sont repris. L’ancien ordre du navigateur est migré au premier chargement du PC ; l’ordre enregistré par le service est partagé avec la télécommande.

`pinned-action.json` contient l’action choisie. Le paquet d’installation ne contient ni ces fichiers, ni le dossier de données, ni les secrets Spotify ou le jeton de télécommande d’un autre utilisateur. Chaque nouvelle installation crée son association mobile.

Le diagnostic peut être consulté avant export. Son contenu exclut notamment noms personnalisés, chemins matériels, identifiants de conteneur, numéros de série, jetons et identifiants de compte. L’export est local ; aucun envoi n’est automatique.

## Vérifier la livraison

- Installation neuve : assistant, choix vide ou sélectif, ajout ultérieur et absence de raccourci PS5 automatique.
- Mise à jour : conservation des appareils, noms, couleurs, profils, ordre et état audio.
- Gestion : appareils identiques, ignorés, déconnectés et association d’une fiche préparée.
- Réglages : aucune commande sur un modèle inconnu, contrôle du seul point audio choisi, confirmation des modifications.
- Action épinglée : configuration sans exécution, bascule explicite, remplacement, retrait et droits mobiles.
- Résilience : découverte en échec, adaptateur bloqué, sauvegarde en échec, inventaire corrompu et réponses tardives.
- Distribution : paquet sans données utilisateur, modules nécessaires inclus, mise à jour et redémarrage du service installé.

Les tests automatisés utilisent des fixtures pour les mutations. Les essais de découverte natifs sur le PC de développement sont en lecture seule. Une validation sur les PC et modèles des amis reste nécessaire avant de déclarer leurs réglages propriétaires pris en charge. Une signature commerciale, un canal de mise à jour automatique, de nouveaux adaptateurs, les mises à jour de firmware et les autres systèmes d’exploitation nécessitent des ressources ou travaux supplémentaires.
