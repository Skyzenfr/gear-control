# Razer DeathAdder V4 Pro — contrôle natif

L’adaptateur `app/server/adapters/razer-mouse.mjs` effectue réellement des lectures et des écritures HID. La compatibilité porte exclusivement sur la DeathAdder V4 Pro, VID `1532`, PID `00BE` en USB filaire et `00BF` via son récepteur. Une autre souris Razer n’est jamais ouverte pour essayer ce protocole.

## Sources et portée

Implémentation indépendante à partir des faits du protocole publiés par [OpenRazer, révision 477f4d6d3ab7e9deb6310d5bd3742560c5cbe80d](https://github.com/openrazer/openrazer/tree/477f4d6d3ab7e9deb6310d5bd3742560c5cbe80d) :

- [Identifiants exacts et délai USB](https://github.com/openrazer/openrazer/blob/477f4d6d3ab7e9deb6310d5bd3742560c5cbe80d/driver/razermouse_driver.h).
- [Sélection des commandes par modèle, transaction, fréquence, DPI, batterie et veille](https://github.com/openrazer/openrazer/blob/477f4d6d3ab7e9deb6310d5bd3742560c5cbe80d/driver/razermouse_driver.c).
- [Paramètres des commandes](https://github.com/openrazer/openrazer/blob/477f4d6d3ab7e9deb6310d5bd3742560c5cbe80d/driver/razerchromacommon.c).
- [Structure et somme de contrôle](https://github.com/openrazer/openrazer/blob/477f4d6d3ab7e9deb6310d5bd3742560c5cbe80d/driver/razercommon.h), [transport USB](https://github.com/openrazer/openrazer/blob/477f4d6d3ab7e9deb6310d5bd3742560c5cbe80d/driver/razercommon.c).
- [API asynchrone node-hid](https://github.com/node-hid/node-hid#async-api-usage).
- [Modèle, capteur 45 000 DPI et fonctionnalités officielles](https://www.razer.com/gaming-mice/razer-deathadder-v4-pro).

Aucun code OpenRazer n’est importé ou exécuté. Les tests utilisent des transports simulés ; aucun firmware de V4 Pro n’est déclaré validé sur du matériel réel.

## Échanges

Interface USB `0`, feature report ID `0`, charge utile de 90 octets. `node-hid` reçoit une trame de 91 octets incluant l’ID. Transaction `1F` ; somme XOR des octets 2 à 87 en position 88. Attente de 35 ms entre émission et réponse, supérieure au minimum de 31 ms du modèle.

| Fonction | Classe/lecture | Classe/écriture | Paramètres |
| --- | --- | --- | --- |
| Numéro de série | `00/82` | aucune | 22 octets |
| Batterie | `07/80` | aucune | octet 1, échelle 0–255 |
| Charge | `07/84` | aucune | octet 1, 0 ou 1 |
| Sensibilité | `04/85` | `04/05` | stockage, X et Y entiers big-endian |
| Fréquence | `00/C0` | `00/40` | code dans l’octet 1 |
| Veille | `07/83` | `07/03` | secondes, uint16 big-endian |
| Firmware | `00/81` | aucune | majeur, mineur |

La sensibilité règle les deux axes entre 100 et 45 000 DPI. La commande d’écriture utilise explicitement `VARSTORE=1` ; elle touche le réglage embarqué. La fréquence émet les deux commandes de stockage `0` puis `1` requises pour ce modèle. Les codes admis sont `40→125`, `10→500`, `08→1000`, `04→2000`, `02→4000`, `01→8000` Hz. La veille admet 60–900 secondes ; sa persistance après extinction n’est pas affirmée.

## Garanties

L’énumération ne configure rien : seules les commandes de lecture sont émises. Chaque fonction doit répondre correctement avant d’exposer son contrôle. Avant chaque écriture, le chemin, VID/PID/interface et numéro de série lu sont revérifiés. Une relecture du réglage doit correspondre à la valeur demandée. Les écritures ne sont jamais répétées automatiquement.

Réponse tronquée, mauvais checksum, transaction/commande incohérente, statut occupé ou refusé : aucune réussite annoncée. Une lecture peut être répétée une fois. Chaque appel HID a un délai maximal ; un appel natif encore actif est mis en quarantaine et son canal n’est jamais réutilisé concurremment. Un numéro de série inconnu ou identique sur deux récepteurs distincts bloque les écritures.

La connexion filaire et le récepteur ne sont regroupés que si leur modèle exact et leur numéro de série lu concordent. Les contrôles non couverts — boutons, macros, profils, calibration, mises à jour — restent accessibles dans [Razer Synapse](https://www.razer.com/synapse-4).

## Famille Razer (souris non vérifiées)

Depuis la 3.2.36, les autres souris Razer reçoivent les mêmes commandes communes, documentées pour toute la gamme par le projet openrazer (faits de protocole réimplémentés, aucun code copié). L’interface USB 0 doit porter une collection souris (page 1, usage 2) : claviers et casques Razer sont exclus.

- **Identifiant de transaction** : `0x1F` (modèles récents), `0x3F` ou `0xFF` (plus anciens). La lecture du numéro de série est tentée avec chacun, dans cet ordre ; l’identifiant qui répond est ensuite utilisé pour toutes les commandes de cette souris.
- **Fréquence** : `00/C0` est lue d’abord ; si la souris la refuse, l’ancienne commande `00/85` (lecture) / `00/05` (écriture) est utilisée, avec les codes `01→1000`, `02→500`, `08→125` Hz dans l’octet 0.
- Batterie, charge, sensibilité, veille et firmware : mêmes commandes que ci-dessus. Une fonction qui ne répond pas n’est pas proposée.

Ces modèles ne sont pas vérifiés sur matériel : chaque écriture est relue, et une différence est signalée sans nouvelle tentative.
