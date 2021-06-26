# Salty Chat for [FiveM](https://fivem.net/)

An example implementation of Salty Chat for [FiveM](https://fivem.net/) OneSync and OneSync Infinity.  

You can report bugs or make sugguestions via issues, or contribute via pull requests - we appreciate any contribution.  
Join our [Discord](https://discord.gg/MBCnqSf) and start with [Salty Chat](https://www.saltmine.de/)!

# Setup Steps
Before starting with the setup, make sure you have OneSync enabled and your server artifacts are up to date.

1. Copy the folder `saltychat` into your resources
2. [Build the solution](https://github.com/saltminede/saltychat-docs/blob/master/installing-vs.md#installing-visual-studio) (`saltychat\SaltyChat-FiveM.sln`) with Visual Studio 2019, so the `*.net.dll` files get build
3. Add `start saltychat` into your `server.cfg`
4. Open `config.json` and adjust the [variables](https://github.com/saltminede/saltychat-docs/blob/master/setup.md#config-variables)
```
  "VoiceEnabled": true,
  "ServerUniqueIdentifier": "NMjxHW5psWaLNmFh0+kjnQik7Qc=",
  "MinimumPluginVersion": "",
  "SoundPack": "default",
  "IngameChannelId" : 25,
  "IngameChannelPassword": "5V88FWWME615",
  "SwissChannelIds": [ 61, 62 ],
```
5. (Optional) Change keybinds in `config.json`, see [default values](https://github.com/saltminede/saltychat-fivem#keybinds) below

**Attantion**: CFX team implemented a NUI blacklist and blocked local (`127.0.0.1` and `localhost`) WebSocket connections.
If the clientside can't connect to the WebSocket, make sure that you can resolve `lh.saltmine.de`:
1. Open `Windows Command Prompt` by searching `cmd`
2. Execute `nslookup lh.saltmine.de`

If it resolved to `127.0.0.1` then your issue is probably somewhere else, if not then you can use e.g. [Google DNS servers](https://developers.google.com/speed/public-dns/docs/using#addresses).

# Config
Variable | Type | Description
------------- | ------------- | -------------
VoiceRanges | `float[]` | Array of possible voice ranges
EnableVoiceRangeNotification | `bool` | Enables/disables a notification when chaning the voice range
VoiceRangeNotification | `string` | Text of the notification when changing the voice range, `{voicerange}` will be replaced by the voice range
RadioType | `int` | Radio type which will be used for radio communication - [see possible values](https://github.com/saltminede/saltychat-docs/blob/master/enums.md#radio-type)
EnableRadioHardcoreMode | `bool` | Limits some radio functions like using the radio while swimming/diving and allows only one sender at a time
UltraShortRangeDistance | `float` | Maximum range of USR radio mode
ShortRangeDistance | `float` | Maximum range of SR radio mode
LongRangeDistace | `float` | Maximum range of LR radio mode
MegaphoneRange | `float` | Range of the megaphone (only available while driving a police car)
NamePattern | `string` | Naming schema of TeamSpeak clients, `{serverid}` will be replaced by the FiveM server ID of the client and `{guid}` will be replaced by a generated GUID
RequestTalkStates | `bool` | Enables/disables [TalkState's](https://github.com/saltminede/saltychat-docs/blob/master/commands.md#11--talkstate)
RequestRadioTrafficStates | `bool` | Enables/disables [RadioTrafficState's](https://github.com/saltminede/saltychat-docs/blob/master/commands.md#33--radiotrafficstate)

# Keybinds
Below are the default keybinds which will be written to your client config (`%appdata%\CitizenFX\fivem.cfg`).  
Changing the default values wont change the values saved to your config.  
Keybinds can be changed in game through the keybinding options of GTA V (`ESC` > `Settings` > `Key Bindings` > `FiveM`).

Variable | Description | Default
:---: | :---: | :---:
ToggleRange | Toggles voice range | F1
TalkPrimary | Talk on primary radio | N
TalkSecondary | Talk on secondary radio | Caps
TalkMegaphone | Use the Megaphone (only in police vehicles) | B

# Events
## Client
### SaltyChat_PluginStateChanged
Parameter | Type | Description
------------ | ------------- | -------------
pluginState | `int` | Current state of the plugin (e.g. client is in a swiss channel), see [GameInstanceState](https://github.com/saltminede/saltychat-docs/blob/master/enums.md#game-instance-state) for possible values

### SaltyChat_TalkStateChanged
Parameter | Type | Description
------------ | ------------- | -------------
isTalking | `bool` | `true` when player starts talking, `false` when the player stops talking

### SaltyChat_VoiceRangeChanged
Parameter | Type | Description
------------ | ------------- | -------------
voiceRange | `float` | current voice range
index | `int` | index of the current voice range (starts at `0`)
availableVoiceRanges | `int` | count of available voice ranges

### SaltyChat_MicStateChanged
Parameter | Type | Description
------------ | ------------- | -------------
isMicrophoneMuted | `bool` | `true` when player mutes mic, `false` when the player unmutes mic

### SaltyChat_MicEnabledChanged
Parameter | Type | Description
------------ | ------------- | -------------
isMicrophoneEnabled | `bool` | `false` when player disabled mic, `true` when the player enabled mic

### SaltyChat_SoundStateChanged
Parameter | Type | Description
------------ | ------------- | -------------
isSoundMuted | `bool` | `true` when player mutes sound, `false` when the player unmutes sound

### SaltyChat_SoundEnabledChanged
Parameter | Type | Description
------------ | ------------- | -------------
isSoundEnabled | `bool` | `false` when player disabled sound, `true` when the player enabled sound

### SaltyChat_RadioTrafficStateChanged
Parameter | Type | Description
------------ | ------------- | -------------
name | `string` | TeamSpeak name of the player
isSending | `bool` | `true` when radio traffic is received, `false` when radio traffic breaks or ends
isPrimaryChannel | `bool` | `true` radio traffic is received on primary channel, `false` when radio traffic is received on secondary channel
activeRelay | `string` | TeamSpeak name of the active relay (only if someone near you has the speaker enabled)

# Exports
## Client
### GetVoiceRange
Returns the current voice range as float.

### GetRadioChannel
Get the current radio channel.

Parameter | Type | Description
------------ | ------------- | -------------
primary | `bool` | Whether to get the primary or secondary channel

### GetRadioVolume
Returns the current radio volume as float (0.0f - 1.6f).

### GetRadioSpeaker
Returns the current state of the radio speaker as bool (`true` speaker on, `false` speaker off).

### SetRadioChannel
Set the current radio channel.

Parameter | Type | Description
------------ | ------------- | -------------
radioChannelName | `string` | Name of the radio channel
primary | `bool` | Whether to set the primary or secondary channel

### SetRadioVolume
Adjust the radio's volume

Parameter | Type | Description
------------ | ------------- | -------------
volumeLevel | `float` | Overrides the volume in percent (0f - 1.6f / 0 - 160%)

### SetRadioSpeaker
Turn the radio speaker on (`true`) or off (`false`).

Parameter | Type | Description
------------ | ------------- | -------------
isRadioSpeakEnabled | `bool` | `true` to enable speaker, `false` to disable speaker

## Server
### SetPlayerAlive
Sets player `IsAlive` flag.

Parameter | Type | Description
------------ | ------------- | -------------
netId | `int` | Server ID of the player
isAlive | `bool` | `true` if player is alive, otherwise `false`

### EstablishCall
Starts a call between two players.

Parameter | Type | Description
------------ | ------------- | -------------
callerNetId | `int` | Server ID of the caller
partnerNetId | `int` | Server ID of the call partner

### EndCall
Ends a call between two players.

Parameter | Type | Description
------------ | ------------- | -------------
callerNetId | `int` | Server ID of the caller
partnerNetId | `int` | Server ID of the call partner

### SetPlayerRadioSpeaker
Turns radio speaker of an player on/off.

Parameter | Type | Description
------------ | ------------- | -------------
netId | `int` | Server ID of the player
toggle | `bool` | `true` to turn on speaker, `false` to turn it off

### SetPlayerRadioChannel
Sets a player's radio channel.

Parameter | Type | Description
------------ | ------------- | -------------
netId | `int` | Server ID of the player
radioChannelName | `string` | Name of the radio channel
isPrimary | `bool` | `true` to set the channel as primary, `false` to set it as secondary

### RemovePlayerRadioChannel
Removes a player from the radio channel.

Parameter | Type | Description
------------ | ------------- | -------------
netId | `int` | Server ID of the player
radioChannelName | `string` | Name of the radio channel

### SetRadioTowers
Sets the radio towers.

Parameter | Type | Description
------------ | ------------- | -------------
towers | `float[][]` | Array with radio tower positions and ranges (X, Y, Z, range)

# Addons
Here are some Scripts provided by external developers for extending SaltyChat

- [SaltyHUD](https://github.com/saltminede/saltychat-fivem/tree/master/saltyhud) Adds a HUD displaying your SaltyChat range, if your muted & co.
- [SaltyCircle](https://github.com/EgoPvP/erp_saltycircle) Adds a circle displaying you current voice range when changing it
- [SaltyNUI](https://github.com/EgoPvP/SaltyNUI) Adds a Blackscreen, if users dont have Teamspeak started up or are not in the correct Channel
- [ls-radio](https://github.com/BlackFlash5/ls-radio) Forked version of the Original, made to be compatible with SaltyChat

