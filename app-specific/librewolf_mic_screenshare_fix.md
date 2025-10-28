# Fixing issues with microphone access & screenshares on LibreWolf

For me, communication platforms (e.g.: Discord) couldn't access my microphone or allow screensharing.

## Microphone issues fix

### Testing if even the system can access the microphone

## Screenshare issues fix

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

Add Discord to the **resistFingerprint domain exception list**:

```
privacy.resistFingerprinting.exemptedDomains =*.discord.com
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
