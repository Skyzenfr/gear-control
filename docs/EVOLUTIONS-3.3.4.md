# Version 3.3.4 — Détection et connexion Philips Hue

Cette version corrige un refus d’association constaté sur un pont Hue BSB002 :
le firmware rejetait `generateclientkey: false` avec l’erreur API 7. Gear Control
omet désormais ce paramètre facultatif pour demander sa clé d’application
standard, sans clé de synchronisation Entertainment. Un test de régression
reproduit ce refus.

Philips Hue devient une intégration facultative et repliée. À l’ouverture de
**Réglages généraux → Périphériques**, Gear Control recherche les ponts sur le
réseau local par mDNS et auprès du service de découverte Philips. Sans pont, aucun
formulaire ni avertissement « À connecter » ne s’impose. Le bouton **Configurer
Philips Hue** permet toujours de saisir une adresse manuellement.

La recherche locale fonctionne sans Internet et garde la priorité si Philips
connaît encore une ancienne adresse. Une source indisponible n’annule pas les
résultats de l’autre. Seules les annonces du service Hue sont recherchées ; aucun
balayage des appareils du réseau n’est effectué. L’identité du pont est vérifiée
par HTTPS lors de l’association.

Pour associer un pont détecté :

1. Choisissez **Configurer le pont**.
2. Appuyez sur le bouton physique du pont.
3. Choisissez **Associer le pont** dans les 30 secondes.

Si la recherche ne trouve rien, ouvrez **Configurer Philips Hue**, puis saisissez
l’adresse IPv4 indiquée dans les réglages de l’application Hue. Le PC et le pont
doivent être sur le même réseau local.

Une association existante peut être réparée avec **Reconnecter le pont**, notamment
après un changement d’adresse IP. L’ancienne association est conservée jusqu’à la
réussite de la nouvelle tentative. Les erreurs distinguent désormais l’absence de
réponse, l’identité du pont, le certificat HTTPS et l’absence d’API v2. Le délai
d’attente de l’interface laisse aussi le temps à Windows de protéger la clé.

Les textes du panneau et les diagnostics sont disponibles en français et en
anglais. Vérifications réussies : **966 tests, 965 réussis, 1 ignoré, aucun
échec**, `npm run check` sans erreur ni avertissement et `npm run smoke` vert sur
le port 3299 en simulateur. Les négociations HTTPS sont testées contre un serveur
local factice, y compris le refus d’un certificat ou d’une identité incorrecte
avant l’envoi d’identifiants. Le panneau a été vérifié dans Edge en français et
en anglais, à 1440 et 520 pixels, sans débordement ni erreur JavaScript.

Ces vérifications automatisées utilisent des réponses réseau factices et le mode
simulateur. Un diagnostic du pont réel a confirmé la découverte mDNS et Philips,
la connexion HTTPS et le refus du paramètre à l’origine du blocage. Après
installation locale de la version 3.3.4 et pression du bouton physique,
l’association a réussi : quatre lumières remontent dans l’inventaire, dont trois
connectées. Aucune commande de lumière n’a été envoyée ; les écritures restent
à vérifier sur matériel.

Voir [le protocole et les limites](HUE-PROTOCOL.md).
