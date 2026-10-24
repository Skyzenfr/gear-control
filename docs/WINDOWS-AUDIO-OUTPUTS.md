# Sélection de la sortie audio Windows

`GET /api/audio/outputs` liste les endpoints **de lecture actifs**, y compris les sorties USB, Bluetooth, HDMI et virtuelles. Les microphones et périphériques déconnectés ne sont pas proposés. L’API retourne :

```json
{
  "outputs": [{"id":"endpoint Windows", "name":"nom Windows", "default":true, "multimediaDefault":true, "communicationsDefault":true, "deviceId":"sony-…", "volume":72, "muted":false}],
  "defaults": {"console":"endpoint", "multimedia":"endpoint", "communications":"endpoint"}
}
```

`POST /api/audio/default`, avec le corps JSON strict `{"endpointId":"ID proposé"}`, affecte cette sortie aux **trois rôles Console, Multimédia et Communications**, pour la lecture normale, les appels et le chat vocal. La réponse utilise le même format et ajoute `changedRoles:["console","multimedia","communications"]`. Les volumes, les états muets et le microphone ne sont pas modifiés.

`output.deviceId` associe les sorties Logitech G Fits et Sony INZONE Buds à leur identifiant GearControl. À chaque GET et POST, les collections HID de contrôle exactes sont énumérées ; leur instance PnP est associée au même `ContainerId` physique que l’endpoint Windows. Le nom du périphérique, les alias et l’ordre d’énumération n’interviennent pas pour ces récepteurs. Les Creative Pebble Pro sont découvertes par leur nom de modèle Windows puis associées à une identité dérivée de leur endpoint exact. Leur adaptateur conserve cette identité si le nom Windows change pendant la session. Les autres sorties sans association ou les associations ambiguës renvoient `deviceId:null`. Les chemins HID, les instances PnP et les `ContainerId` restent privés au backend.

`POST /api/audio/volume` accepte `{"endpointId":"ID proposé", "volume":72, "muted":false}` avec au moins l’un des champs `volume` (nombre de 0 à 100) ou `muted` (booléen). Le champ omis garde sa valeur. Il ne change aucun rôle de sortie et renvoie le snapshot complet, avec des niveaux indisponibles à `null`. L’option booléenne `requireDefault:true`, utilisée par le menu audio et la télécommande, refuse une commande lorsque la sortie principale a changé depuis la préparation du réglage. La sortie doit encore être active ; volume et sourdine sont confirmés après écriture, avec tentative de restauration en cas d’échec.

Les Pebble Pro apparaissent dans le hub avec leur volume Windows et leur sourdine. Leurs réglages ciblent leur endpoint précis même si une autre sortie est principale. Le hub ne prétend pas piloter leur molette physique ni leur éclairage RGB.

Le choix utilise l’identifiant exact Windows, avec une nouvelle énumération des sorties actives juste avant chaque écriture. Le helper refuse une identité arbitraire, un microphone ou un endpoint qui vient de disparaître. Les trois rôles sont relus après les changements ; si Windows ne confirme pas l’état demandé, une restauration des rôles précédents est tentée et vérifiée. Les protections locales Host/Origin, la taille maximale du JSON et le type de contenu du serveur s’appliquent aussi à ces routes.

La lecture repose sur [Windows Core Audio / GetDefaultAudioEndpoint](https://learn.microsoft.com/en-us/windows/win32/api/mmdeviceapi/nf-mmdeviceapi-immdeviceenumerator-getdefaultaudioendpoint). Le changement utilise l’interface COM Windows non documentée `IPolicyConfig`, également utilisée par [EarTrumpet](https://github.com/File-New-Project/EarTrumpet/blob/master/EarTrumpet/Interop/MMDeviceAPI/IPolicyConfig.cs). Son fonctionnement a été vérifié sur ce Windows ; un changement futur de cette interface peut demander une adaptation. Aucun programme fabricant ou outil de commande externe n’est lancé. Le helper Windows Audio existant est réutilisé.

Preuve matérielle du 11 octobre 2026 : **INZONE Buds Game → G Fits blanc → INZONE Buds Game pour les trois rôles**. Les trois rôles ont été restaurés exactement et les volumes/mutes des quatre sorties actives sont restés identiques. Aucun échantillon sonore n’a été joué. Capture : `research/sony/default-output-three-roles-verified.json`. La capture antérieure `default-output-verified.json` concerne la première version qui ne changeait que Console et Multimédia.

Le rollback a également été vérifié sur un échec intermédiaire volontaire : Console passe aux G Fits blancs, la sélection d’un endpoint inexistant pour Multimédia échoue, puis les trois rôles Sony initiaux sont restaurés automatiquement et relus. Capture : `research/sony/default-output-rollback-verified.json`.

Tests : `node --test server/windows-audio.test.mjs` depuis `app`. Ils couvrent l’identité des récepteurs, les entrées invalides, le refus d’un microphone, le fonctionnement sans récepteur Logitech, les trois rôles retournés, la plateforme, les erreurs natives, les identités audio/HID indépendantes des noms, le débranchement et l’absence de chemins privés dans l’API.
