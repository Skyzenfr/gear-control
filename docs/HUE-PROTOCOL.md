# Philips Hue — API locale du pont

Gear Control utilise le pont Philips Hue par son API locale CLIP v2 en HTTPS.
Le logiciel Hue Sync installé sur le PC peut continuer à fonctionner : Gear Control
crée sa propre association et ne récupère pas les clés d'une autre application.
Cette intégration ne pilote ni le Bluetooth direct des ampoules, ni les fonctions
de synchronisation audio/vidéo de Hue Sync.

## Sources et portée

Sources officielles consultées le 23 octobre 2026 :

- [Get Started](https://developers.meethue.com/develop/get-started-2/) : découverte du
  pont, création d'un utilisateur par `POST /api`, validation physique par le bouton.
- [New Hue API](https://developers.meethue.com/new-hue-api/) : CLIP v2, HTTPS obligatoire,
  découverte mDNS ou `discovery.meethue.com`, abandon d'UPnP, API Entertainment
  distincte pour les mises à jour rapides et continues.
- [Actualités du programme développeur](https://developers.meethue.com/) : abandon
  d'HTTP annoncé le 4 juin 2025 ; état des lampes modifiable lorsqu'elles sont
  éteintes, annoncé le 10 juin 2025 ; nouveau champ `effects_v2` annoncé le
  7 février 2025.
- [Using HTTPS](https://developers.meethue.com/develop/application-design-guidance/using-https/),
  [conversion RGB/xy](https://developers.meethue.com/develop/application-design-guidance/color-conversion-formulas-rgb-to-xy-and-back/)
  et [référence CLIP v2](https://developers.meethue.com/develop/hue-api-v2/api-reference/) :
  pages officielles nécessitant un compte développeur, non accessibles dans cette
  session. Leur contenu complet n'est pas présenté comme vérifié.

Les faits de schéma non visibles sur les pages publiques ont été recoupés dans
les modèles de la bibliothèque Home Assistant
[aiohue, révision 894d8226](https://github.com/home-assistant-libs/aiohue/tree/894d8226f8650724676b1368147e8151da5f231b/aiohue/v2/models),
notamment `light`, `entertainment`, `entertainment_configuration` et
`zigbee_connectivity`. Les certificats publics des autorités Hue sont publiés
également dans le client
[hb-hue-tools, révision 2e877470](https://github.com/ebaauw/hb-hue-tools/blob/2e877470bfd5339ee9e7e75a0e17cd13a53e1dfa/lib/HueClient.js),
qui les attribue au guide officiel HTTPS. Aucun code de ces projets n'est importé.

Le PC de développement possède Hue Sync 1.13.1.83. Son fichier de configuration
indique un pont BSB002, API 1.78.0, firmware 1978418030. Ce constat provient d'une
lecture locale de métadonnées, sans requête au pont. Il ne constitue pas une
validation du protocole sur matériel. Aucun identifiant, nom de groupe, adresse
locale ou secret de Hue Sync n'est inclus dans le dépôt.

## Association et transport

La découverte se lance à l’ouverture du panneau Périphériques, sans association
automatique. Le panneau reste facultatif et replié en l’absence de pont. Elle
interroge en parallèle le service DNS-SD local `_hue._tcp.local` par mDNS et
`https://discovery.meethue.com/`. Une saisie manuelle de l'adresse permet
l'association si la découverte échoue. Les réponses mDNS sont bornées en durée,
nombre et taille ; seuls les enregistrements Hue PTR/SRV/TXT/A cohérents, avec
identifiant de pont et port HTTPS 443, produisent des candidats. Aucun balayage
du sous-réseau ni aucune requête HTTP aux appareils annoncés n’est effectué à
cette étape. Les candidats deviennent des ponts vérifiés pendant l’association
HTTPS. Les résultats locaux ont priorité sur une adresse connue du service
Internet pour le même identifiant. Une source indisponible n’empêche pas
l’utilisation de l’autre.

Le format DNS et les requêtes mDNS suivent les
[règles mDNS (RFC 6762)](https://www.rfc-editor.org/rfc/rfc6762.html),
notamment les réponses unicast à un port source éphémère (§ 6.7).

Le transport
n'accepte qu'une adresse IPv4 privée littérale RFC 1918, sur le port 443 ; ni nom
DNS arbitraire, ni URL, ni redirection, ni pont virtuel ou appareil inconnu.

L'utilisateur appuie sur le bouton du pont puis lance l'association dans Gear
Control. La seule écriture de cette étape est `POST /api`, avec un nom d'application
Gear Control et sans demander de clé Entertainment. Le nom d'utilisateur émis par
le pont devient l'en-tête `hue-application-key` des requêtes CLIP v2. Une lecture
des ressources du pont valide l'association avant son enregistrement.

Le champ facultatif `generateclientkey` est omis. Le 23 octobre 2026, le pont
BSB002 de test (API 1.78.0, firmware 1978418030) a refusé explicitement la valeur
`false` avec l’erreur v1 de type 7. L’omettre demande uniquement la clé
d’application standard ; Gear Control ne demande pas de clé Entertainment.

La chaîne TLS est validée avec les autorités Hue fournies à ce transport,
indépendamment des autorités publiques du système. Le nom commun du certificat
doit être un identifiant de pont à 16 chiffres hexadécimaux et correspondre au pont
attendu. L'identité est contrôlée avant l'envoi d'un secret. Il n'existe pas de
repli HTTP ni de désactivation de la validation TLS.

| Autorité publique | Empreinte SHA-256 du certificat DER |
| --- | --- |
| `CN=root-bridge, O=Philips Hue, C=NL` | `F0BD8E6509E82F774D63BC009D5388C969FE3DCF7D6D541D6351B72B898D8ACF` |
| `CN=Hue Root CA 01, O=Signify Hue, C=NL` | `D8B89448B2AF8E1676185AC07219EE9DCBC8F01C122A026A2A4B7B5CFE0328B8` |

Les réponses sont bornées en durée et en taille. Les erreurs de transport,
d'authentification et le tableau `errors` de CLIP v2 sont contrôlés ; une réponse
HTTP 200 ne suffit pas à confirmer une écriture.

La clé de Gear Control est protégée par Windows DPAPI dans `hue-connection.json`. Elle ne
figure pas dans les réponses HTTP, les diagnostics ou les sauvegardes générales.
La déconnexion locale oublie cette association ; les autres applications restent
associées au pont.

## Ressources et réglages

`GET /clip/v2/resource` fournit les services `light`, leurs appareils propriétaires
et les informations de connexion Zigbee. Les contrôles proposés dépendent des
champs effectivement annoncés par chaque lumière.

| Réglage | Écriture sur `/clip/v2/resource/light/{id}` |
| --- | --- |
| Allumage | `on.on`, booléen |
| Luminosité | `dimming.brightness`, de 0 à 100 % |
| Couleur | `color.xy`, coordonnées CIE xy ; conversion depuis la couleur RGB de l'interface |
| Température du blanc | `color_temperature.mirek`, bornée par `mirek_schema` ; conversion depuis les kelvins |

Une couleur et une température de blanc ne sont pas envoyées ensemble. Une
relecture après chaque écriture doit confirmer l'état annoncé ; un refus, une
lampe déconnectée ou une valeur différente produit une erreur. Les écritures ne
sont pas répétées automatiquement.

La capture d'un profil et sa restauration conservent seulement le mode de couleur
actif : couleur xy ou température de blanc. La valeur initiale éventuellement
proposée par l'éditeur pour un mode inactif permet de choisir ce mode ; elle ne
représente pas une mesure et n'entre pas dans une capture de l'état courant.

La présence d'un service `light` en mode `streaming` ou d'une configuration
Entertainment active bloque les écritures du pont. Gear Control n'arrête jamais
une synchronisation pour prendre le contrôle. Dans une configuration
Entertainment, les membres des canaux désignent des services `entertainment` ;
`renderer_reference` relie ces services à une lumière. Ces identifiants ne doivent
pas être assimilés directement aux identifiants `light`.

## Limites et validation

- Un pont associé à la fois, IPv4 privée uniquement. Le multicast mDNS peut être
  bloqué par le réseau, le pare-feu ou un VPN ; la découverte Philips et la saisie
  manuelle restent disponibles. Un changement d'adresse DHCP peut nécessiter une
  nouvelle association via **Reconnecter le pont** ; l’association précédente
  reste enregistrée tant que la tentative n’a pas abouti.
- Réglages individuels des lumières ; pas de création de pièces/zones, gestion des
  accessoires, scènes Hue natives, effets, gradients segmentés ou mises à jour de
  firmware.
- Pas de streaming Entertainment ou de synchronisation d'écran. Les automatisations
  de Gear Control utilisent les réglages usuels ; elles ne doivent pas produire
  une animation continue à haute fréquence via l'API REST.
- Le simulateur et les tests automatisés n'utilisent aucun secret ni aucun pont
  réel. Les écritures d’allumage et d’extinction ont été validées séparément sur
  matériel avec l’association propre à Gear Control. La luminosité, la couleur et
  la température du blanc restent **non vérifiées sur matériel**.
- Les actions à l’arrêt disposent d’un budget global de dix secondes avant la
  fermeture des adaptateurs. Les actions restantes et les écritures Hue différées
  sont annulées à l’expiration ; une commande déjà envoyée ne peut pas être retirée.
  Depuis la version 3.3.7, le lanceur déclenche aussi cette fermeture gracieuse
  lorsque Windows confirme un arrêt, un redémarrage ou une fermeture de session.
  Une demande Windows annulée ne déclenche aucune action. L’exécution n’est pas
  garantie lors d’une coupure de courant ou d’un arrêt forcé du processus.

Validation réelle du 23 octobre 2026, version 3.3.4 : découverte mDNS et Philips,
validation HTTPS, association par bouton physique avec clé Gear Control protégée
par DPAPI, lecture CLIP v2 et remontée de quatre lumières, dont trois connectées.
Aucune commande de lumière n’a été envoyée pendant ce diagnostic.

Validation réelle du 23 octobre 2026, version 3.3.6 installée : allumage et
extinction confirmés par lecture sur le pont. L’essai d’arrêt a exécuté la règle
`gearControlStopping` déjà enregistrée, sans commande manuelle de lumière : lumière allumée
avant l’arrêt, puis éteinte selon une lecture Hue indépendante. La commande
`--shutdown` a terminé avec le code 0 en 1 406 ms, puis le service a été relancé
et confirmé prêt en version 3.3.6. Une seconde lecture Hue indépendante a confirmé
l’allumage par la règle de démarrage existante, sans commande manuelle de lumière.
Cet essai ne valide pas la luminosité, la couleur ou la température du blanc.
Preuve locale : `.build/shutdown-release-verification.json`.
