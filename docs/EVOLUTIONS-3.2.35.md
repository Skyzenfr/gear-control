# Gear Control 3.2.35 — Découverte automatique, Bluetooth et manettes XInput

- **Nouveau périphérique détecté** : quand vous branchez ou connectez un appareil, une carte apparaît en bas de l’écran du PC. **Ajouter** le place dans vos périphériques, **Ignorer** le range dans les appareils ignorés, la croix le laisse pour plus tard. Si plusieurs appareils arrivent ensemble, **Tout voir** ouvre la gestion des périphériques.
- **Appareils Bluetooth** : les écouteurs, casques, souris, claviers et enceintes appairés à Windows apparaissent avec leur état de connexion et, s’ils le transmettent à Windows, leur niveau de batterie. Pour un casque ou des écouteurs, la fiche reprend aussi le volume et la sourdine Windows de leur sortie audio : il n’y a plus deux cartes pour le même appareil. Les téléphones et ordinateurs appairés ne sont pas listés.
- **Manettes Xbox et compatibles XInput** : elles sont reconnues avec leur connexion (USB ou sans fil) et leur batterie. XInput ne donne que quatre paliers (vide, faible, moyen, plein) : le pourcentage affiché est donc approximatif.
- **Reconnaissance plus précise** : la gestion des périphériques indique comment un appareil non pris en charge a été reconnu : **Famille reconnue** (marque dont le protocole est connu, modèle non vérifié), **Protocole standard** (éclairage dynamique Windows, QMK/VIA, commandes de casque, manette standard) ou simple identification Windows. Aucune commande n’est envoyée à ces appareils.
- Le type d’un appareil inconnu est mieux deviné : le nom du produit est pris en compte, et une souris qui expose aussi des touches de macro n’est plus prise pour un clavier.
- **Logi Options+** a désormais la priorité sur les périphériques Logitech HID++ tant qu’il est ouvert, comme G HUB.

Téléchargez **GearControl-Setup-3.2.35.exe** pour installer ou mettre à jour Gear Control sur Windows 10 ou 11, 64 bits.

La publication inclut l’empreinte SHA-256 de l’installateur et le manifeste signé `latest.json` pour les mises à jour automatiques.
