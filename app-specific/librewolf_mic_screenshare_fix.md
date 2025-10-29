# Fixing issues with microphone access & screenshares on LibreWolf

For me, communication platforms (e.g.: Discord) couldn't access my microphone or allow screensharing.

## Microphone issues fix

### Testing if even the system can access the microphone

You can install **alsa-utils** and record a test file to make sure your microphone is set up correctly:

```shell
sudo pacman -S alsa-utils
arecord -f cd -d 5 test.wav
aplay test.wav
```

### Configuring microphone on system-level

If you haven't already, install the `pipewire-pulse` package for compatibility:

```shell
sudo pacman -S pipewire-pulse
```

Check available devices with the following command:

```shell
wpctl status
```

The output should look something like this:

```
PipeWire 'pipewire-0' [1.4.9, vb2007@vbDesktop, cookie:3368655778]
 └─ Clients:
        32. xdg-desktop-portal-hyprland         [1.4.9, vb2007@vbDesktop, pid:926]
        33. WirePlumber                         [1.4.9, vb2007@vbDesktop, pid:929]
        41. WirePlumber [export]                [1.4.9, vb2007@vbDesktop, pid:929]
        42. waybar                              [1.4.9, vb2007@vbDesktop, pid:859]
        43. waybar                              [1.4.9, vb2007@vbDesktop, pid:859]
        44. waybar                              [1.4.9, vb2007@vbDesktop, pid:859]
        74. wpctl                               [1.4.9, vb2007@vbDesktop, pid:15080]

Audio
 ├─ Devices:
 │      45. AD106M High Definition Audio Controller [alsa]
 │      46. G733 Gaming Headset                 [alsa]
 │      47. Built-in Audio                      [alsa]
 │
 ├─ Sinks:
 │      54. AD106M High Definition Audio Controller Digital Stereo (HDMI) [vol: 0.40]
 │  *   55. G733 Gaming Headset Analog Stereo   [vol: 0.70]
 │      57. Built-in Audio Analog Stereo        [vol: 0.35]
 │
 ├─ Sources:
 │  *   56. G733 Gaming Headset Mono            [vol: 1.00]
 │      58. Built-in Audio Analog Stereo        [vol: 1.00]
 │
 ├─ Filters:
 │
 └─ Streams:

Video
 ├─ Devices:
 │
 ├─ Sinks:
 │
 ├─ Sources:
 │
 ├─ Filters:
 │
 └─ Streams:
 ```

 In this case, I'm setting the *G733 Gaming Headset Mono* source (56) as the default input device.

 You can set a default device with the following command:

 ```shell
 wpctl set-default 56 #where 56 is the source microphone id
 wpctl set-default 55 #if you want to set a default output as well
 ```

Make sure to set the microphone's volume correctly (e.g. to 80%):

```shell
wpctl set-volume 56 0.8
```

After performing all modifications, restart the audio services:

```shell
systemctl --user restart wireplumber pipewire pipewire-pulse
```

**Restart LibreWolf as well, and check if Discord & the browser can access your microphone.**

If issues it still can't access/detect your microphone, try setting some LibreWolf specific configurations as described below.

### Giving microphone access to LibreWolf

As suggested in one of the comments below [THIS](https://www.reddit.com/r/LibreWolf/comments/m2pq2n/unable_to_access_microphone_and_camera/) post.

```
media.getusermedia.screensharing.enabled = true
media.navigator.enabled = true
media.navigator.video.enabled = true
media.peerconnection.enabled = true
media.peerconnection.ice.default_address_only = false
media.peerconnection.ice.no_host = false
media.peerconnection.ice.tcp = true
media.peerconnection.identity.enabled = true
media.peerconnection.identity.timeout = 10000
media.peerconnection.turn.disable = false
media.peerconnection.use_document_iceservers = true
media.peerconnection.video.enabled =true
```

## Screenshare issues fix

Lots of information came from [THIS](https://gitlab.com/librewolf-community/browser/linux/-/issues/173) issue.

Making screensharing work requires setting/modifying some LibreWolf config values on the `about:config` page:

Allows downloading Google's and Cisco's GMP (Gecko Media Plugin) components:

```
media.gmp-provider.enabled =true
```

Enables use of the h.264 codec (e.g.: used by Discord for screensharing):

```
media.gmp-gmpopenh264.enabled =true
```

After modifying those values, restart LibreWolf and verify the codec downloads started on the `about:addons` page's **Plugins** part.

You should see the codec addon and a warning text that displays: *OpenH264 Video Codec provided by Cisco Systems, Inc. will be installed shortly.*

After it installs succcesfully, continue setting / modifying the following values on the `about:config` page.

Disable **resistFingerprint** and add Discord to the **resistFingerprint domain exception list**:

```
privacy.resistFingerprinting =false
privacy.resistFingerprinting.exemptedDomains =*.discord.com
```

**If you want to keep your user-agent string private, install a specifid spoofing extension as described [HERE](https://gitlab.com/librewolf-community/browser/linux/-/issues/173#note_534079374).**

[Firefox store link for extension](https://addons.mozilla.org/en-US/firefox/addon/user-agent-string-switcher/) [Extension's source code on GitHub](https://github.com/ray-lothian/UserAgent-Switcher/)

Open the extension, click on **Options** (bottom left), select **Custom Mode**, then paste in the following json content:

```json
{
  "discord.com": "Mozilla/5.0 (Windows NT 10.0; rv:80.0) Gecko/20100101 Firefox/80.0"
}
```

Make sure WebRTC is set to true (should be true by default):

```
media.peerconnection.enabled =true
```

Make sure ICE Host Candidates (and other related settings) are allowed (should be false by default):

```
media.peerconnection.ice.no_host =false
media.peerconnection.ice.proxy_only =false
media.peerconnection.ice.proxy_only_if_behind_proxy =false
media.peerconnection.ice.proxy_only_if_pbmode =false
```

Set this value to **false** as well (this one's true by default):

```
media.peerconnection.ice.default_address_only =false
```

If streaming still fails, disable hardware accelaration in Discord:

1. In Discord, go to User Settings > Voice & Video.
2. Scroll down to the Advanced section.
3. Disable the "H.264 Hardware Acceleration" toggle.

If it still fails, uncheck hardware accelaration on LibreWolf's `about:preferences#general` page (under **Performance**):

- [ ] Use recommended performance settings
  - [ ] Use hardware acceleration when available
