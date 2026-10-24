# G FITS volume and microphone through Windows

`app/server/windows-audio.mjs` runs a small, persistent PowerShell helper with
locally compiled C# declarations for the Windows APIs. It uses neither G HUB,
Logitech DLLs, an installed audio library, nor an additional driver.

The HID receiver is enumerated as VID `046D`, PID `0A7B`, usage page `FFA0`,
usage `1`. Its exact HID path is converted to a Windows PnP instance ID.
`CM_Locate_DevNodeW` and `CM_Get_DevNode_PropertyW` read
`DEVPKEY_Device_ContainerId`. Active audio endpoints are enumerated by
`IMMDeviceEnumerator::EnumAudioEndpoints`; their PnP container IDs must match
the receiver's container. `IMMEndpoint::GetDataFlow` distinguishes output and
capture. More than one endpoint of either flow in a container disables that
control. Friendly names and enumeration order are never used for association.
The same lowercase-path SHA-256 prefix as the G FITS HID adapter identifies the
public record. Endpoint IDs and container IDs remain private.

`WindowsAudio.refresh()` returns an array with `id`, `volume`, `microphone`,
`volumeMuted`, `microphoneMuted`, `capabilities: {volume, microphone}`, and
`source: 'windows-core-audio'`. Levels are percentages of the Windows endpoint
master scalar. They are Windows audio controls, independent of the earbuds'
HID EQ or touch settings. `settings(id, {volume?, microphone?})` only accepts
finite numeric percentages from 0 through 100. The receiver and container
mapping are checked again immediately before each write. No default endpoint,
audio playback, or mute setting is modified.

`IAudioEndpointVolume::SetMasterVolumeLevelScalar` writes the level;
`GetMasterVolumeLevelScalar` reads it back. A mismatch greater than half a
percentage point or a changed mute state causes the command to fail and the
levels already written by that command to be restored. This does not install
anything or route applications to a different audio device. Endpoint mute is
reported separately, because changing the microphone level does not unmute it.

Hardware validation on 2026-10-11, with the two actual receivers:

| Pair | Volume test | Microphone test | Mute retained |
| --- | --- | --- | --- |
| White | 100 → 99 → 100 | 90 → 89 → 90 | Output off, microphone on |
| Black | 100 → 99 → 100 | 50 → 49 → 50 | Both off |

Each pair's change left the other pair's values unchanged. Final readings matched
the initial snapshot. The helper started in approximately 0.7 seconds; subsequent
reads, including HID enumeration, took approximately 0.3 seconds on this PC.
The helper stays alive for the local bridge and is terminated when it stops.

Microsoft API references:

- [Device ContainerId](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/devpkey-device-containerid)
- [CM_Locate_DevNodeW](https://learn.microsoft.com/en-us/windows/win32/api/cfgmgr32/nf-cfgmgr32-cm_locate_devnodew)
- [CM_Get_DevNode_PropertyW](https://learn.microsoft.com/en-us/windows/win32/api/cfgmgr32/nf-cfgmgr32-cm_get_devnode_propertyw)
- [EnumAudioEndpoints](https://learn.microsoft.com/en-us/windows/win32/api/mmdeviceapi/nf-mmdeviceapi-immdeviceenumerator-enumaudioendpoints)
- [IAudioEndpointVolume](https://learn.microsoft.com/en-us/windows/win32/api/endpointvolume/nn-endpointvolume-iaudioendpointvolume)
