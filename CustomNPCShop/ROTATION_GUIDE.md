# Rotation System Documentation

## Overview
The rotation system allows you to control which trades appear in your NPC shops based on time or random selection. You can organize trades into groups and cycle through them using three different modes.

## File Location
`plugins/CustomNPCShop/shops/npc_<id>/rotationSetup.yml`

## Quick Start

### 1. Create Groups
```yaml
groups:
  common:
    - trade1
    - trade2
  rare:
    - trade3
    - trade4
```

### 2. Choose a Rotation Mode

#### **Sequential Mode** (cycles in order)
```yaml
rotationType:
  type: sequential
  order:
    - common
    - rare
    - none
  eachRotationTimer: 30m
  rotationMessage: "&6Next: &e%timer%"
```

#### **Random Mode** (random selection)
```yaml
rotationType:
  type: random
  order:
    - common
    - rare
  eachRotationTimer: 1h
  rotationMessage: "&bRandom: &e%timer%"
```

#### **Time-Based Mode** (schedule by time of day)
```yaml
rotationType:
  type: timeBased
  schedule:
    "08:00":
      show: common
      message: "&aMorning deals: &e%timer%"
    "18:00":
      show: rare
      message: "&6Evening specials: &e%timer%"
```

### 3. Configure "None" State
```yaml
noneMessage: "&cShop closed!"
noneSound: ENTITY_VILLAGER_NO
```

## Advanced Features

### Individual Trade Selection
Show specific trades instead of whole groups:
```yaml
order:
  - common                    # Shows all trades in common group
  - [trade5, trade6]          # Shows ONLY these 2 specific trades
  - rare                      # Shows all trades in rare group
```

### Timer Format
- `20s` = 20 seconds
- `5m` = 5 minutes
- `2h` = 2 hours
- `1d` = 1 day

### Placeholder
`%timer%` in messages shows time remaining:
- Sequential/Random: "1h 15m 24s"
- Time-Based: "1h 15m"

## How It Works

### Sequential
1. Starts with first item in `order` list
2. Shows trades for `eachRotationTimer` duration
3. Rotates to next item when timer expires
4. Loops back to start after last item

### Random
1. Randomly selects from `order` list
2. Uses seed-based randomness (consistent across server restarts)
3. Changes selection every `eachRotationTimer`
4. Seed = `(lastRotationTime / 1000) + npcId`

### Time-Based
1. Checks server's current time
2. Finds matching time slot in `schedule`
3. Shows trades specified in that slot
4. Updates when next time slot begins
5. Wraps around midnight automatically

### None State
When rotation shows "none":
- Shop doesn't open
- Player sees chat message (`noneMessage`)
- Sound plays (`noneSound`)
- GUI is prevented from opening

## Reload
Use `/customnpcshop reload <npcId>` to refresh rotation config without restarting server.

## Examples

### Daily Schedule
```yaml
groups:
  morning: [coffee, bread]
  lunch: [sandwich, soup]
  dinner: [steak, wine]

rotationType:
  type: timeBased
  schedule:
    "00:00": { show: none, message: "&cClosed" }
    "08:00": { show: morning, message: "&aBreakfast: %timer%" }
    "12:00": { show: lunch, message: "&eLunch: %timer%" }
    "18:00": { show: dinner, message: "&6Dinner: %timer%" }
    "22:00": { show: none, message: "&cClosed" }
```

### Weekly Rotation (30m each)
```yaml
groups:
  monday: [item1, item2]
  tuesday: [item3, item4]
  # ... etc

rotationType:
  type: sequential
  order:
    - monday
    - tuesday
    - wednesday
    - thursday
    - friday
    - saturday
    - sunday
  eachRotationTimer: 1d  # 1 day each
  rotationMessage: "&6Day: &e%timer%"
```

### Mystery Shop
```yaml
groups:
  set1: [common1, common2]
  set2: [rare1, rare2]
  set3: [super_rare]

rotationType:
  type: random
  order:
    - set1
    - set1
    - set1
    - set2
    - set2
    - set3
  eachRotationTimer: 2h
  rotationMessage: "&d&lMYSTERY SHOP &r&e(%timer%)"
```
Note: set1 appears 3x more often than set3

## Troubleshooting

**Trades not showing?**
- Check trade names match exactly in settings.yml
- Ensure rotationSetup.yml is in correct folder
- Use `/customnpcshop reload <npcId>`

**Timer not updating?**
- Timer updates when shop is opened
- Sequential/Random: Checks rotation on shop open
- Time-Based: Always uses current server time

**"None" not working?**
- Ensure `none` is in `order` list (sequential/random)
- Or set `show: none` in time slot (timeBased)
- Check noneMessage and noneSound are configured

**Colors not showing?**
- Use `&` not `§` in config
- Plugin converts `&` to `§` automatically
