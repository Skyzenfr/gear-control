# Écrans ASUS : DDC/CI et DisplayWidget Center

## Sources et méthode

Les faits de protocole ont été relevés le 23 octobre 2026 dans **ASUS DisplayWidget
Center 1.5.2.8**, déjà installé sur le PC : inspection statique de `VCPAPI`,
`OLEDSettingsPage` et des tables de modes, sans exécuter l’application ni ses DLL.
Les correspondances de registres et de champs ont été réimplémentées ; aucun code
du fabricant n’est embarqué et DisplayWidget Center n’est pas une dépendance.

La chaîne de capacités du **XG27AQWMG, firmware 107**, provient du cache
`UserConfig/CapabilityData.json` du logiciel ASUS. La fixture
`app/server/fixtures/asus-xg27aqwmg-capabilities.txt` contient uniquement cette
chaîne : aucun numéro de série ou autre identifiant personnel.

Références publiques complémentaires :

- [ASUS : présentation de DisplayWidget Lite](https://www.asus.com/support/faq/1045142/)
  et [manuel DisplayWidget](https://dlcdnet.asus.com/pub/ASUS/LCD%20Monitors/Utility/DisplayWidget/E11420_ASUS_DisplayWidget_UM_English.pdf) :
  les réglages disponibles dépendent du modèle.
- [Recherche directe sur le PA329CV](https://github.com/wachtermar/a-ok/blob/main/Docs/Research.md) :
  les commandes des différentes familles ne sont pas interchangeables.
- [Analyse du firmware PG42UQ](https://github.com/schlarpc/asus-pg42uq-firmware-re/blob/main/ddc_surface.md) :
  ce modèle ancien utilise d’autres registres pour GamePlus. Aucun code de ce
  projet n’a été repris et ces registres ne sont pas généralisés aux autres ROG.

## Reconnaissance et règles d’exposition

Transport : API Windows `dxva2`, sur le câble DisplayPort, HDMI, USB-C ou DVI.
L’identification ASUS utilise les fabricants EDID `AUS` et `ACI`.

1. Les quatre réglages existants restent disponibles comme avant : luminosité,
   contraste, volume et entrée.
2. Les capacités MCCS sont lues une fois en arrière-plan, en série avec les autres
   opérations de l’adaptateur. Le hub conserve son dernier instantané pendant cette lecture.
3. Seuls les codes connus **et annoncés** par cet écran sont demandés en plus.
   Une lecture valide est également nécessaire pour afficher leur contrôle.
4. Pour les fonctions du protocole ASUS récent, l’octet haut du maximum de
   `EF` doit être **2**. Il ne s’agit pas de la version MCCS de `DF`. Les
   correspondances de jeu sont limitées aux familles identifiées `XG`, `PG`, `VG`.
5. Une liste ne propose que ses valeurs annoncées et reconnues. Une valeur actuelle
   inconnue ou combinée sans encodage établi ne donne pas lieu à un contrôle inventé.
6. Chaque écriture exige une relecture conforme. Pour les champs partagés,
   lecture, fusion et écriture sont effectuées dans la même opération native.

## Registres utilisés

| Code | Réglage | Portée |
|---|---|---|
| `14` | Température de couleur | ASUS, valeurs nommées connues |
| `16`, `18`, `1A` | Gains rouge, vert, bleu | ASUS ; valeurs courantes dans le mot bas |
| `59`–`5E` | Saturation des six couleurs | ASUS ; valeur OSD dans le mot bas |
| `6C`, `6E`, `70` | Décalages RVB | ASUS si annoncés |
| `72` | Gamma | Valeurs annoncées `5000` à `A000` |
| `87`, `8A`, `8D` | Netteté/VividPixel, saturation, sourdine | ASUS |
| `DC` | Mode/préréglage d’image | GameVisual nommé en protocole 2 jeu ; valeurs brutes annoncées ailleurs |
| `E0`, `E6` | Overdrive/Trace Free, filtre de lumière bleue | ASUS si annoncés et lisibles |
| `E2`–`E5` | HDR, réticule, minuterie, Shadow Boost | Protocole 2 jeu |
| `E7`, `E8`, `EA`, `EE` | Alignement, position GamePlus, FPS, ELMB | Protocole 2 jeu |
| `ED` | Capteur Neo : distance et délai d’absence | Protocole 2 jeu ; deux octets indépendants |
| `F8`, `F9` | Rappel de nettoyage, déplacement des pixels | Protocole 2 jeu |
| `FD` | Interrupteurs regroupés | Protocole 2 jeu ; un seul bit modifié |

Bits `FD` exposés si présents dans le masque de capacités : `0001` VRR,
`0002` ASCR, `0008` économiseur OLED, `0020` logos, `0040` luminosité uniforme,
`0080` Clear Pixel Edge, `0800` barre des tâches, `1000` bordures,
`2000` atténuation extérieure, `4000` atténuation globale. Les autres bits
sont préservés, y compris ceux qui ne sont pas connus de Gear Control.

`ED` : distance dans l’octet bas (`0` désactivé, `3` 60 cm, `2` 90 cm,
`1` 120 cm, `FF` personnalisé) ; délai en minutes dans l’octet haut.
Chaque liste est filtrée par les valeurs annoncées par l’écran.

## Limites et validation

**Les nouvelles écritures ne sont pas vérifiées sur matériel avec Gear Control.**
La fixture réelle prouve les capacités annoncées, pas le fonctionnement de chaque
écriture. Les essais automatisés utilisent un faux transport ; le test C# compile
le helper et vérifie les fusions de bits et d’octets sans énumérer les écrans.

Cette extension n’est donc pas une équivalence complète avec DisplayWidget Center.
Restent notamment les fonctions propres à des modèles non documentés, certains
registres combinés (HDR, espace colorimétrique, raccourcis, Aura, KVM/PIP/PBP selon
la famille), AI Visual/App Tweaker, MultiScreen et les mises à jour de firmware.
Les fonctions non établies restent absentes, même si leur code est annoncé.

Le lancement immédiat du nettoyage des pixels (`FD`, bit `0010`) n’est pas
transformé en interrupteur de profil : c’est une action ponctuelle qui peut
interrompre l’affichage. Seul son **rappel** est réglable ici. Les codes de remise
à zéro, d’alimentation, de service et de navigation OSD sans relecture sont exclus.

Le HDR ou un autre mode d’image peut verrouiller des réglages. Une commande
ignorée ou arrondie est signalée comme non confirmée. Les libellés d’options dont
seule la valeur est établie restent « Option N », sans inventer de nom de mode.
