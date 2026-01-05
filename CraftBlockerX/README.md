# CraftBlockerX

A lightweight Minecraft Bukkit/Spigot plugin that allows server administrators to block specific items from being crafted. Perfect for servers that want to disable certain overpowered items or maintain gameplay balance.

## Features

- 🚫 Block any craftable item from being crafted
- 📝 Support for multiple blocked items
- 🔄 Live reload configuration without server restart
- 🛡️ Comprehensive protection (crafting tables, crafter blocks, player inventory crafting, shift-clicks, drags)
- ⚙️ Easy YAML configuration
- 📊 Optional logging of blocked crafting attempts

## Download

**[Download CraftBlockerX-1.0-SNAPSHOT.jar](https://github.com/azkhanmhd/Minecraft-Plugins/blob/main/CraftBlockerX/CraftBlockerX-1.0-SNAPSHOT.jar)**

Click "Download" on the GitHub page to get the jar file.

## Installation

1. **Download** the plugin jar file from the link above
2. **Stop** your Minecraft server
3. **Place** the `CraftBlockerX-1.0-SNAPSHOT.jar` file into your server's `plugins` folder
4. **Start** your server
5. The plugin will generate a default `config.yml` in `plugins/CraftBlockerX/`

## Configuration

After first run, edit `plugins/CraftBlockerX/config.yml`:

```yaml
# Configure the list of materials to block from crafting.
# Use server material names or vanilla names (case-insensitive).
# Add or remove items as needed.
blockedMaterials:
  - "MACE"
  - "TNT"
  - "NETHERITE_SWORD"

# If true, blocked actions are logged to console.
logBlocked: true
```

### Finding Material Names

Material names follow the Bukkit Material enum. Common examples:
- `MACE` - The mace weapon
- `TNT` - Explosive blocks
- `DIAMOND_SWORD` - Diamond swords
- `NETHERITE_HELMET` - Netherite armor pieces
- `ENCHANTING_TABLE` - Enchantment tables

For a complete list, see the [Spigot Material List](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Material.html).

## Commands

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/craftblockerx` | `/cbx` | Shows plugin info | - |
| `/craftblockerx reload` | `/cbx reload` | Reloads the configuration | `craftblockerx.reload` |

## Permissions

| Permission | Description | Default |
|------------|-------------|---------|
| `craftblockerx.reload` | Allows reloading the plugin config | OP |

## How It Works

CraftBlockerX prevents crafting through multiple protection layers:

1. **Recipe Removal** - Removes blocked recipes from the server on startup
2. **Crafting Preview** - Prevents the result item from appearing in the crafting output slot
3. **Event Blocking** - Blocks all attempts to craft, drag, or move blocked items in crafting interfaces
4. **Recipe Undiscovery** - Undiscovers blocked recipes for all online players

This ensures players cannot craft blocked items through normal crafting tables, crafter blocks, inventory crafting, or any exploits.


## Requirements

- Minecraft Server 1.21+ (Bukkit/Spigot/Paper)
- Java 17+

## Testing

1. Add items to the `blockedMaterials` list in the config
2. Try to craft those items in-game
3. The crafting should be prevented and (if enabled) logged to console
4. Use `/cbx reload` to test configuration changes without restarting

## Troubleshooting

- **Plugin not working?** Check the server console for "CraftBlockerX" messages
- **Config not loading?** Make sure the YAML syntax is correct (proper indentation)
- **Material not found?** Verify the material name matches your server version
- **Changes not applying?** Use `/cbx reload` or restart the server

## Support

If you encounter any issues or have suggestions:
- Open an issue on the [GitHub repository](https://github.com/azkhanmhd/Minecraft-Plugins)

## 📝 **License**

These plugins are **open for anyone to use** on their Minecraft servers.

**❌ Prohibited:**
- Selling or redistributing these plugins
- Posting JAR files online or on other platforms
- Claiming ownership or authorship

**Violations of these terms will result in license revocation and potential legal action.**

For custom development or commercial licensing, contact via [azkhan.org](https://azkhan.org)

---
---

> Made With ❤️&☕ By Azk 💗
