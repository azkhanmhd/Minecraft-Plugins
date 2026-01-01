# PreventWeaponUseX

A lightweight Minecraft Bukkit/Spigot plugin that allows server administrators to prevent specific items from being used in certain worlds. Perfect for servers that want to disable overpowered weapons in specific areas or maintain gameplay balance across different worlds.

## Features

- 🚫 Block any item from being used in specific worlds
- 🌍 World-specific restrictions
- 🔍 Wildcard pattern support (e.g., `%any%_SPEAR` matches all spear types)
- 🔄 Live reload configuration without server restart
- 🛡️ Comprehensive protection (right-click, left-click actions)
- ⚙️ Easy YAML configuration
- 📊 Optional debug logging of blocked usage attempts
- 🔒 No bypass permissions - even OPs are restricted

## Download

**[Download PreventWeaponUseX.jar](https://github.com/azkhanmhd/Minecraft-Plugins/blob/main/PreventWeaponUseX/PreventWeaponUseX.jar)**

Click "Download" on the GitHub page to get the jar file.

## Installation

1. **Download** the plugin jar file from the link above
2. **Stop** your Minecraft server
3. **Place** the `PreventWeaponUseX.jar` file into your server's `plugins` folder
4. **Start** your server
5. The plugin will generate a default `config.yml` in `plugins/PreventWeaponUseX/`

## Configuration

After first run, edit `plugins/PreventWeaponUseX/config.yml`:

```yaml
enabled: true
debug: false
restrictions:
  - items:
      - "MACE"
    worlds:
      - "world"
      - "void"
  - items:
      - "%any%_SPEAR"
    worlds:
      - "rpg"
      - "world"
```

### Configuration Explained

- **enabled**: Set to `true` to enable the plugin, `false` to disable without unloading
- **debug**: Set to `true` to log all blocked usage attempts to console
- **restrictions**: List of restriction rules, each containing:
  - **items**: List of material names or patterns to block
  - **worlds**: List of world names where these items are blocked

### Item Patterns

You can use exact material names or wildcard patterns:
- **Exact match**: `"MACE"` - Blocks only the mace
- **Wildcard**: `"%any%_SPEAR"` - Blocks all items ending with `_SPEAR` (e.g., `WOODEN_SPEAR`, `IRON_SPEAR`)

### Finding Material Names

Material names follow the Bukkit Material enum. Common examples:
- `MACE` - The mace weapon
- `DIAMOND_SWORD` - Diamond swords
- `NETHERITE_AXE` - Netherite axes
- `BOW` - Bows
- `CROSSBOW` - Crossbows

For a complete list, see the [Spigot Material List](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Material.html).

## Commands

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/pwu reload` | `/preventweaponusex reload` | Reloads the configuration | OP only |

## Permissions

**No bypass permissions** - Even operators cannot bypass item restrictions. Only operators and console can use the reload command.

## How It Works

PreventWeaponUseX prevents item usage through event interception:

1. **Interaction Blocking** - Intercepts all player interaction events (left-click, right-click)
2. **World Checking** - Verifies if the player's current world is in the restriction list
3. **Item Matching** - Checks if the item matches any patterns (exact or wildcard)
4. **Usage Prevention** - Cancels the event and notifies the player

This ensures players cannot use blocked items in restricted worlds, regardless of their permissions or status.

## Requirements

- Minecraft Server 1.21+ (Bukkit/Spigot/Paper)
- Java 21+

## Testing

1. Add items and worlds to the `restrictions` list in the config
2. Try to use those items in the specified worlds
3. The usage should be prevented with a message
4. Use `/pwu reload` to test configuration changes without restarting
5. Enable `debug: true` to see blocked attempts in console

## Troubleshooting

- **Plugin not working?** Check the server console for "PreventWeaponUseX" messages
- **Config not loading?** Make sure the YAML syntax is correct (proper indentation)
- **Material not found?** Verify the material name matches your server version
- **Changes not applying?** Use `/pwu reload` or restart the server
- **Items still usable?** Check that the world name matches exactly (case-sensitive)

## Support

If you encounter any issues or have suggestions:
- Open an issue on the [GitHub repository](https://github.com/azkhanmhd/Minecraft-Plugins)

---

> Made With ❤️&☕ By Azk 💗
