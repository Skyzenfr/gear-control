# Gear Control 3.3.0 — Écrans AGON et nettoyage des builds

- **Écrans AOC / AGON** : réglages de couleur, gains RVB, netteté, sourdine et
  modes de jeu selon les capacités annoncées et lues.
- **AGON PRO AG276QZD2** : profil étendu pour Game Color, Shadow Control,
  gamma, HDR, latence, Dial Point, compteur FPS, Light FX, espace colorimétrique
  et protections OLED. Les options disponibles dépendent des réponses de l’écran.
- Les réglages étendus utilisent le composant signé compatible de **G-Menu
  installé, avec l’application fermée**. Les réglages standard restent autonomes.
  Les modifications sont relues, et les textes sont disponibles en français et en anglais.
- **Construction de l’installateur** : après une compilation réussie et la
  vérification SHA-256 du nouvel installateur, `Build-Installer.ps1` supprime les
  anciens installateurs et leurs fichiers `.sha256` dans `release/` et dans la
  copie de compilation. Les versions plus récentes et les autres fichiers sont
  conservés ; une construction échouée ne déclenche aucun nettoyage.

Le support AGON reste **partiel et non vérifié sur matériel avec Gear Control**.
Il ne couvre pas toute la gamme ni tous les outils G-Menu, notamment PIP/PBP.
Voir [le protocole et les limites](AGON-PROTOCOL.md).

Cette version conserve les réglages ASUS et l’interface PC en anglais. Les
nouvelles commandes ASUS restent à vérifier sur matériel, et la télécommande
du téléphone reste en français.

Installateur : `release/GearControl-Setup-3.3.0.exe`.
