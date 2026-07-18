<div align="center">
    <h1>AntiCrasher (SkyXNetwork Fork)</h1>
    <p><em>Updated and adapted for Paper 26.1.2 - SkyXNetwork Edition</em></p>
</div>

---

> **This is NOT the official AntiCrasher plugin.** This is an unofficial fork maintained by SkyXNetwork, updated and
> fixed to work with **Paper 26.1.2** (Minecraft 26.1.2). The original plugin is
> by [CraftSupport](https://github.com/CraftSupport/AntiCrasher) / [smashyalts](https://github.com/smashyalts/AntiCrasher).
> All credit for the original work goes to the original authors.

---

## What was changed

The original AntiCrasher plugin was incompatible with Paper 26.1.2 due to a critical NPE (NullPointerException) in
packetevents reflection (`NMS_ITEM_STACK_CLASS` null). This was caused by:

1. **PacketEvents was shaded (bundled) into the plugin jar** - Paper 26.1+ dropped the internal plugin remapper, so
   shaded packetevents could no longer resolve Mojang-mapped NMS classes at runtime.
2. **Outdated PacketEvents version** - the plugin used `2.12.3-SNAPSHOT`, which lacked proper support for Paper 26.1.2's
   Mojang-mapped environment.
3. **Self-initialization of PacketEvents** - the plugin tried to build and initialize PacketEvents itself, conflicting
   with the server's own PacketEvents instance.

### Fixes applied

| Change                                 | Description                                                                                                                                                       |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **PacketEvents upgraded to 2.13.0**    | Latest stable release with full MC 26.1.2+ and Mojang-mapping support                                                                                             |
| **PacketEvents no longer shaded**      | Plugin now requires PacketEvents to be installed separately as a server plugin. This lets PacketEvents handle its own NMS reflection and Mojang-mapping correctly |
| **Removed self-initialization**        | AntiCrasher no longer calls `PacketEvents.setAPI()` / `load()` / `init()` / `terminate()`. It uses the already-initialized API from the PacketEvents plugin       |
| **Paper API updated to 26.1.2**        | Build dependency updated from `1.21.8-R0.1-SNAPSHOT` to `26.1.2.build.+`                                                                                          |
| **Added `codemc-releases` repository** | PacketEvents stable releases are hosted here                                                                                                                      |
| **Plugin dependency declared**         | `plugin.yml` now declares `depend: [packetevents]` to ensure proper load order                                                                                    |

## Requirements

- **Paper 26.1.2** (or compatible)
- **PacketEvents plugin** - must be installed separately in your `plugins` folder. Download
  from [Modrinth](https://modrinth.com/plugin/packetevents)
  or [SpigotMC](https://www.spigotmc.org/resources/packetevents-api.80279/)
- **Java 25** or newer

## Installation

1. Download and install the [PacketEvents plugin](https://modrinth.com/plugin/packetevents) into your server's `plugins`
   folder.
2. Download `AntiCrasher-bukkit-v2.0.12.jar` from the `libs/` folder (or build it yourself).
3. Place the jar in your server's `plugins` folder.
4. Restart the server.

## Patches

The following exploits are patched:

- **Bundle Crash Exploit** - prevents malicious SELECT_BUNDLE_ITEM packets with negative indices
- **Book Dupe Exploit** - prevents EDIT_BOOK packets with oversized titles (>32 chars)
- **Negative Slot ID Crash Exploit** - prevents CLICK_WINDOW packets with negative slot IDs (servers older than 1.20.5)
- **NBT Tab Completion Crash Exploit** - prevents TAB_COMPLETE packets with excessively long inputs

## Commands

- `/ac reload` - Reloads configs. Requires `anticrasher.command.reload` permission.

## Permissions

| Permission                   | Description                              | Default |
|------------------------------|------------------------------------------|---------|
| `anticrasher.command`        | Use the /anticrasher command             | op      |
| `anticrasher.command.reload` | Reload configs                           | op      |
| `anticrasher.alerts`         | Receive exploit detection alerts in chat | op      |
| `anticrasher.bypass`         | Bypass all AntiCrasher checks            | op      |
| `anticrasher.updates`        | Receive plugin update notifications      | op      |

## Building from source

```bash
# Requires Java 25+ and Gradle
.\gradlew :bukkit:build
```

The output jar will be in `libs/AntiCrasher-bukkit-v2.0.12.jar`.

## Credits

- **Original plugin
  **: [CraftSupport](https://github.com/CraftSupport/AntiCrasher), [smashyalts](https://github.com/smashyalts/AntiCrasher),
  RivenBytes, Skullians, RebelMythik, MachineBreaker
- **Fork adapted for**: SkyXNetwork
- **PacketEvents**: [retrooper](https://github.com/retrooper/packetevents)

## License

This project retains the license of the original repository.
