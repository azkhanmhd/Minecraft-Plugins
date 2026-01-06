# Custom Villager Breeding System

**Production-level Minecraft plugin for complete control over villager breeding**

Version: 1.0 | API: 1.21 | Minecraft Version: 1.21+ | Author: Azk

---

## 🎯 Features

### Core Features
- ✅ **Disable vanilla breeding** - Complete control over breeding mechanics
- ✅ **Player-initiated breeding** - Two-click selection system with Bread (Changeable)
- ✅ **Per-villager cooldown** - Configurable rest period between breedings
- ✅ **Per-villager max limit** - Limit breeds per villager with auto-reset
- ✅ **Per-chunk breeding limit** - Prevent overpopulation in specific areas
- ✅ **Custom item support** - Require specific custom model data for resource pack items
- ✅ **JSON data storage** - Efficient villager tracking via UUID
- ✅ **Fully configurable** - Every aspect controllable via YAML
- ✅ **Debug mode** - Detailed logging for troubleshooting
- ✅ **Low resource usage** - Optimized for performance

### Anti-Exploit Protection
- 🛡️ Vanilla breeding completely disabled
- 🛡️ Trading GUI cancelled during breeding mode
- 🛡️ Time-limited selection window
- 🛡️ Radius check for villager pairs
- 🛡️ Cannot breed villager with itself
- 🛡️ Thread-safe data management

---

## 📦 Installation

1. Download the latest JAR from releases [`Download`](CustomVillagerBreedingSystem-1.0.jar)
2. Place in your server's `plugins/` folder
3. Restart or reload your server
4. Configure `settings.yml` and `chunksBreeds.yml`
5. Use `/cvbs reload` to apply changes

---

## ⚙️ Configuration

### settings.yml

```yaml
enabled: true                    # Enable/disable entire system
debug: false                     # Debug logging
require-item:
  material: BREAD               # Item needed for breeding
  amount: 2                     # Amount consumed per breed
  check-custom-model: false     # Require specific custom model data (for custom items)
  custom-model-data: 0          # Custom model data number (e.g., 1001 for resource pack items)
click-window: 180               # Time (seconds) between villager selections
villager-radius: 8              # Max distance between villagers

# Per-Villager Cooldown
villager-cooldown:
  enabled: true
  time: 5m                      # Format: s, m, h, d (30s, 5m, 2h, 1d)
  msg: "&cVillager needs rest. Wait &e%time%"
  sound: "minecraft:entity.villager.no"

# Per-Villager Max Breeds
villager-max-breeds:
  enabled: true
  max: 2                        # Max breeds before reset
  reset-time: 12h               # Time until reset
  msg: "&cVillager exhausted. Wait &e%time%"
  sound: "minecraft:entity.villager.no"

# Birth Effects
birth-effects:
  particles:
    type: HEART
    count: 10
  sound: "minecraft:entity.villager.celebrate"
  msg: "&aBreeding successful!"
```

### chunksBreeds.yml

```yaml
chunkMaxEnabled: true           # Enable chunk limits
maxBreeds: 8                    # Max births per chunk
maxResetTime: 24h              # Chunk counter reset time

chunkMaxBreedAlert:
  msg: "&cToo many villagers here. Wait &e%time%"
  sound: "minecraft:entity.villager.no"
```

---

## 🎮 Usage

### Breeding Process

1. **Hold bread** (or configured item) in main hand
2. **Right-click first villager** - Selection confirmed
3. **Right-click second villager** within time window and radius
4. **Validation checks:**
   - Item has correct custom model data (if enabled)
   - Both villagers pass cooldown check
   - Both villagers pass max breed limit
   - Chunk hasn't exceeded limit
   - Player has enough items
5. **Success:** Baby villager spawns with effects

### Custom Item Support

Enable `check-custom-model` to require specific custom model data:
- Perfect for resource packs with custom items (Magic Bread, Golden Carrot, etc.)
- Set `custom-model-data` to the exact model number from your resource pack
- Players must use items with matching custom model data to breed
- Disable check to use any item of the specified material

---

## 🔧 Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/cvbs toggle [on\|off]` | Enable/disable breeding system | `cvbs.admin` |
| `/cvbs reload` | Reload all configurations | `cvbs.admin` |
| `/cvbs debug` | Toggle debug mode | `cvbs.admin` |

**Aliases:** `/customvillagerbreeding`, `/villagerbreeding`

---

## 🔑 Permissions

| Permission | Description | Default |
|-----------|-------------|---------|
| `cvbs.admin` | Access to all commands | op |

---

## 📊 Data Storage

### villagers.json
```json
{
  "uuid-here": {
    "lastBreed": 1704556800000,
    "breedCount": 2,
    "maxReachedAt": 1704556800000
  }
}
```

### chunksBreeds.yml
```yaml
chunks:
  world:0:5:
    currentBreeds: 5
    maxBreeds: 8
    lastReset: 1704556800000
```

---

## 🔍 Debug Mode

Enable with `/cvbs debug` to see:
- First villager selections
- Successful breeding events
- Chunk resets
- Villager breed count resets
- Blocked vanilla breeding attempts

---

## ⚡ Performance

### Optimizations
- **No repeating tasks** - Reset logic runs on-demand
- **ConcurrentHashMap** - Thread-safe without locks
- **Async cleanup** - Every 5 minutes in background
- **Cached data** - In-memory with periodic saves
- **Minimal event listeners** - Only essential events

### Resource Usage (Predicted)
- **RAM:** ~1-5 MB for 1000 tracked villagers
- **CPU:** Negligible (event-driven)
- **Storage:** ~10 KB per 100 villagers (JSON)

---

## 🐛 Troubleshooting

**Villagers won't breed:**
1. Check `/cvbs toggle` is enabled
2. Verify you have correct item/amount
3. Check cooldown hasn't expired
4. Ensure breed limit not reached
5. Verify chunk limit not exceeded

**Data not saving:**
1. Look for errors in console
2. Ensure plugin writes files
3. Try `/cvbs reload`

**Vanilla breeding still works:**
1. Ensure plugin is loaded (`/plugins`)
2. Check for conflicting plugins
3. Verify event listener is registered

---

## 📋 Requirements

- **Minecraft:** 1.21+
- **Server:** Paper/Purpur (Spigot compatible)
- **Java:** 21+
- **Dependencies:** Gson (shaded in JAR)

---

## 🎯 Example Configurations

### Strict Mode (Low Population)
```yaml
villager-cooldown.time: 10m
villager-max-breeds.max: 1
villager-max-breeds.reset-time: 24h
maxBreeds: 5
maxResetTime: 48h
```

### Relaxed Mode (High Population)
```yaml
villager-cooldown.time: 2m
villager-max-breeds.max: 5
villager-max-breeds.reset-time: 6h
maxBreeds: 15
maxResetTime: 12h
```

### No Limits Mode
```yaml
villager-cooldown.enabled: false
villager-max-breeds.enabled: false
chunkMaxEnabled: false
```

---



## 🔄 Changelog

### Version 1.0
- Initial release
- Full breeding system implementation
- Per-villager cooldown and max limits
- Per-chunk breeding limits
- JSON villager data storage
- Complete configuration system
- Admin commands
- Debug mode

---

## 🤝 **Support**

For issues, suggestions, or contributions:
- **Website:** [azkhan.org](https://azkhan.org)
- **GitHub:** [@azkhanmhd](https://github.com/azkhanmhd)

---

## 📝 **License**

These plugins are **open for anyone to use** on their Minecraft servers.

**❌ Prohibited:**
- Selling or redistributing these plugins
- Posting JAR files online or on other platforms
- Claiming ownership or authorship

**Violations of these terms will result in license revocation and potential legal action.**

For custom development or commercial licensing, contact via [azkhan.org](https://azkhan.org)

---

> Made With ❤️&☕ By Azk 💗
