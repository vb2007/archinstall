# Fixing issues with microphone access & screenshares on LibreWolf

For me, communication platforms (e.g.: Discord) couldn't access my microphone or allow screensharing.

## Microphone issues fix

### Testing if even the system can access the microphone

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
media.peerconnection.video.enabled
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

**If you want to keep your user-agent string private, install a specifid spoofing extension as describer [HERE](https://gitlab.com/librewolf-community/browser/linux/-/issues/173#note_534079374).**

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
