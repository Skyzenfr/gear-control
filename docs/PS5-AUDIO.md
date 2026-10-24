# Son PS5 via l’entrée ligne Realtek

Le bouton Son PS5 pilote la case Windows **Écouter ce périphérique** de l’entrée ligne Realtek. La PS5 arrive sur cette entrée analogique. Le contrôle conserve la destination d’écoute déjà choisie dans Windows.

Le backend expose `WindowsAudio.ps5Audio()` et `WindowsAudio.setPs5Audio({ enabled: boolean })`. Les deux retournent `{ available, enabled, endpointId?, name?, reason? }`. Un état inconnu utilise `enabled: null`. Une entrée absente, un type de propriété inconnu ou plusieurs entrées ligne Realtek actives rendent le contrôle indisponible. Aucune entrée n’est choisie par défaut.

## Identification

L’endpoint doit être actif, de capture, et avoir un parent PnP `HDAUDIO\FUNC_01&VEN_10EC&DEV_…`. Il doit aussi exposer le form factor `LineLevel` (2) et le sous-type de connecteur `{DFF21FE3-F70F-11D0-B917-00A0C9223196}`. Son nom affiché n’autorise pas la sélection. Ainsi, le SC440, le microphone de la manette et les autres microphones restent exclus, même renommés.

La sélection s’appuie sur les API Microsoft [CM_Get_Parent](https://learn.microsoft.com/en-us/windows/win32/api/cfgmgr32/nf-cfgmgr32-cm_get_parent), [CM_Get_Device_IDW](https://learn.microsoft.com/en-us/windows/win32/api/cfgmgr32/nf-cfgmgr32-cm_get_device_idw), [EndpointFormFactor](https://learn.microsoft.com/en-us/windows/win32/api/mmdeviceapi/ne-mmdeviceapi-endpointformfactor) et [PKEY_AudioEndpoint_JackSubType](https://learn.microsoft.com/en-us/windows/win32/coreaudio/pkey-audioendpoint-jacksubtype). Le GUID du connecteur est défini par `KSNODETYPE_LINE_CONNECTOR` dans le [header Microsoft ksmedia.h](https://github.com/microsoft/win32metadata/blob/main/generation/WinSDK/RecompiledIdlHeaders/shared/ksmedia.h).

## Propriété d’écoute observée

Windows ne documente pas de PKEY publique pour la case d’écoute. L’inspection locale du property store de **Entrée ligne (Realtek(R) Audio)** a confirmé la clé `{24DBB0FC-9311-4B3D-9CF0-18FF155639D4},1` de type `VT_BOOL` (11), initialement `false`. Le backend n’écrit que cette propriété existante et refuse son absence ou un type différent. Il n’écrit ni le registre directement ni la propriété 0 du même GUID, qui détermine la destination d’écoute.

L’accès utilise les API COM [IMMDevice::OpenPropertyStore](https://learn.microsoft.com/en-us/windows/win32/api/mmdeviceapi/nf-mmdeviceapi-immdevice-openpropertystore), [IPropertyStore::SetValue](https://learn.microsoft.com/en-us/windows/win32/api/propsys/nf-propsys-ipropertystore-setvalue) et `Commit`. La propriété étant une implémentation Windows non documentée, sa présence et son type sont vérifiés à chaque opération. Un refus d’accès remonte comme une erreur, sans remplacement par un autre réglage audio.

## Confirmation et vérification

Chaque écriture réidentifie l’entrée, conserve son état antérieur, puis relit le résultat depuis un nouveau property store. Si la confirmation échoue, le backend tente de rétablir l’état précédent sur cette même entrée. Il ne change ni le microphone SC440, ni les périphériques par défaut, ni le volume ou la sourdine des sorties Windows.

Les tests utilisent des fixtures pour la sélection physique, l’ambiguïté, les échecs partiels et la restauration. La première vérification native a relu et réécrit l’état `false` en contrôlant que les défauts Windows, les volumes et le SC440 restaient inchangés. La vérification d’intégration HTTP a ensuite activé réellement l’écoute (`true` confirmé), puis restauré l’état initial (`false` confirmé) sur la même entrée. L’accès réseau a été vérifié : réponse 200 pour le téléphone associé, 401 sans association.
