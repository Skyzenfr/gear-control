# Écrans AOC / AGON : protocole et prise en charge

## Périmètre

La version 3.2.42 ajoute les réglages AOC standard et un profil étendu pour
**AGON PRO AG276QZD2**. Les nouvelles commandes sont **non vérifiées sur matériel
avec Gear Control**. La famille entière ne peut pas être déclarée entièrement
compatible sur la seule base de ce profil.

Les réglages standard fonctionnent sans logiciel AOC : luminosité, contraste,
volume, entrées, couleurs RVB, température de couleur, netteté, sourdine et modes
de jeu, si annoncés et lisibles. L’EDID doit identifier le fabricant `AOC` pour
les nouveaux réglages propres à cette famille.

Les fonctions étendues utilisent des sélecteurs à **trois octets**, ce que l’API
Windows `SetVCPFeature` de notre adaptateur DDC standard ne transmet pas. Elles
nécessitent donc **le composant de communication signé livré avec G-Menu installé
sur le PC**, version de DLL `1.0.2.8` inspectée dans G-Menu 3.34.0. Gear Control
n’installe, ne démarre et ne redistribue pas G-Menu. L’application et son processus
`G_Menu` doivent être fermés. Un composant absent, différent ou non signé laisse
les réglages standard disponibles et explique cette dépendance dans la fiche.

## Sources

Inspection statique du téléchargement relié par la
[page officielle G-Menu](https://www.aoc.com/uk/gaming/gmenu), le 23 octobre 2026 :
[archive TPV](https://gz-dlsw.tpv-tech.com/MNT/AOC/Software/G-menu/G-Menu.zip),
installateur G-Menu 3.34.0. Les fichiers ont seulement été extraits ; aucun
programme, installateur ou composant téléchargé n’a été exécuté ou installé.

- `Zeasn.Equipment.Entity.Lib.dll` : faits des énumérations de commandes standard,
  `ExternVCPOpCode_E`, `F8E1_ExternVCPOpCode_E`, `E2A0_ExternVCPOpCode_E`, modes de
  jeu et préréglages de couleurs AOC.
- `Zeasn.DDC.Lib.dll` : disposition des structures et signatures de communication.
- `DDCHelperLib.dll` : DLL x86, version 1.0.2.8, signature Authenticode valide
  **TOP VICTORY ELECTRONICS (TAIWAN) CO., LTD.** SHA-256 :
  `84446A7AAA9B81E89495434E543C6BF5DF67CF9F5D4613CEBB65186400DFCD22`.
  Analyse statique de l’énumération : listes séparées par `;`, nom PnP suivi de
  `_`, sorties Windows associées. Aucun code constructeur n’est recopié dans le dépôt.

Le [manuel officiel AG276QZD2](https://www.aoc.com/api/asset/pi41012?ext=pdf&name=AG276QZD2_manual)
décrit les fonctions utilisateur. La
[recherche indépendante sur un AOC 22B2HM2B](https://github.com/zhzh2001/aoc-osd)
confirme le format étendu et le délai de validation des incrustations ; ce modèle
n’est pas utilisé pour autoriser des commandes sur un autre écran.

## Commandes et transport

| Groupe | Sélecteurs implémentés | Réglages |
|---|---|---|
| Standard | `14`, `16`, `18`, `1A`, `87`, `8D`, `DC` | Température, gains RVB, netteté, sourdine, jeu |
| `F8 E1` | `11`, `12`, `13`, `51`, `52`, `55`–`59` | Eco, gamma, DCR, overdrive, HDR, latence, recharge USB, Game Color, Shadow Control, LowBlue |
| `F8 E1` | `5A`, `5B` | Dial Point, compteur FPS |
| `F8 E1` | `60`–`68` | Intensité, effet, motif et composantes avant/arrière Light FX |
| `E2 A0` | `06`, `13`, `18`, `20` | Sniper Scope, USB en veille, atténuation locale, gamut |
| `E2 A0` | `34`, `35`, `40`, `43`, `45`, `46`, `4C`–`4F`, `61` | Déplacement, économiseur, Adaptive-Sync, rappels, Shadow Boost, LEA, logos, bordures, barre des tâches, protection thermique, entretien automatique |

Charge utile de lecture : `01 <groupe haut> <groupe bas> <sélecteur>`.
Charge utile d’écriture : `03 <groupe haut> <groupe bas> <sélecteur> <valeur haut> <valeur bas>`.
Le composant TPV assure le transport DDC/CI vers le pilote graphique.

ABI native : `Packet` de 1 064 octets (deux tableaux de 520 octets), liste de
sorties de 3 124 octets. Le helper est un processus PowerShell **x86**, caché,
borné à 10 secondes. La DLL est chargée par chemin absolu, depuis une installation
G-Menu reconnue, après contrôle de signature, d’éditeur et de version. Aucun
chemin de DLL ou index graphique fourni par l’interface n’est accepté.

## Conditions de sécurité

1. Les contrôles standard supplémentaires exigent un fabricant EDID `AOC`, une
   capacité annoncée et une valeur lue. Les capacités ASUS ne sont pas réutilisées.
2. Les lectures étendues exigent le modèle reconnu `AG276QZD2`. Certains
   firmwares omettent les extensions dans leur chaîne MCCS : une réponse valide
   à la lecture ciblée établit alors la disponibilité de la fonction. Les seuls
   sélecteurs demandés appartiennent à la liste blanche documentée.
3. Le helper retrouve lui-même l’identité Windows du moniteur, sa sortie GDI et
   son nom WMI. Il recherche la même sortie dans l’énumération TPV, refuse une
   correspondance multiple, puis vérifie à nouveau fabricant et modèle par EDID.
4. Seuls les sélecteurs ayant répondu sont affichés. Plages invalides, domaines
   trop grands ou valeurs actuelles inconnues sont exclus. Les choix standard
   sont filtrés par la liste MCCS ; les domaines entiers étendus sont bornés par
   la réponse du moniteur. Les options dont le nom n’est pas établi restent
   « Option N ».
5. Les lectures sont mises en file en arrière-plan, par lots de huit fonctions
   au maximum à chaque sondage du hub, et ne chevauchent pas les écritures. Les
   fonctions apparaissent progressivement pendant la première minute environ.
   Un nouvel état matériel est demandé après chaque changement.
6. Une écriture doit être relue à la valeur exacte. Attente initiale d’une seconde
   pour les incrustations, puis deux essais supplémentaires. Un échec ne devient
   jamais une réussite affichée.

Le nettoyage immédiat des pixels (`E2 A0 36`), les remises à zéro, l’extinction,
les modes usine et les mises à jour du firmware ne sont pas des réglages de
profil et ne sont pas exposés. Les fonctions PIP/PBP, certaines commandes OSD,
les protections ou effets sans encodage établi et les autres modèles AGON
restent à documenter. **Ce lot ne prétend donc pas couvrir toutes les fonctions
de G-Menu ni tous les modèles AGON.**

## Vérification sans matériel

Tests Node avec transport simulé : identification, filtres de capacités,
disponibilité du composant, sélection x86, encodage intact des sélecteurs,
validation des valeurs et relecture. Tests C# : compilation, tailles ABI,
construction des paquets et rejet des sorties ambiguës, sans chargement de DLL
constructeur ni accès aux moniteurs. Les données d’AG276QZD2 utilisées dans les
tests sont **synthétiques**, pas une capture matérielle.
