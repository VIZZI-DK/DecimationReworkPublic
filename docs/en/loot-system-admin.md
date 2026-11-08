# Decimation Rework Loot System

**English** · [Русский](../loot-system-admin.md)

The searched-block and supply-drop loot system is server authoritative. Player corpses and zombie drops are not managed by this system.

## Files

The server creates the following files on first start:

- `config/decimation/loot/groups/*.json` — reusable weighted item groups;
- `config/decimation/loot/tables/*.json` — ordered rolls over groups;
- `config/decimation/loot/sources.json` — block-type bindings and respawn settings;
- `config/decimation/loot/state/dimension_<id>/region_*/` — generated shared inventories and real-time timers in regional snapshots and journals. Older `dimension_<id>.dat` files are migration inputs.

Configuration writes are atomic and keep `.backup` files. Runtime state is not intended for manual editing.
Built-in group and table names are generated as plain English text and are not localization keys. Existing administrator-edited names are preserved.

## Administrator commands

- `/deciserver loot gui` opens the visual editor. The command and editor require operator permissions. The editor works in Survival.
- `/deciserver loot reload` validates and reloads JSON files. Invalid files do not replace the last valid in-memory configuration.
- `/deciserver loot cleanup` removes tracked state whose source block is missing in already loaded chunks. It never loads chunks.
- `/deciserver loot cleanup --load` explicitly loads tracked chunks in small batches, checks them, and queues command-loaded chunks for unloading.

## Editor workflow

### Groups

A group contains weighted complete `ItemStack` values. Add the held stack to preserve its complete NBT, or select an item from the searchable catalog organized by the active Minecraft and Decimation creative tabs. Configure minimum and maximum count, weight, and whether the same entry may repeat in one generation. Custom groups may be copied or deleted when no table references them. Built-in groups may be edited, disabled, copied, and restored, but not deleted.

### Tables

A table executes group rolls from top to bottom. Every roll has an activation chance, minimum and maximum draw count, and duplicate policy. The arrow buttons change roll order. `Maximum occupied slots` caps the generated result even when group draws request more items.

Zone modifiers use the existing zone types, not individual named zones. Click a zone row to select it and its checkbox to enable it. Quantity and rare-loot multipliers combine when multiple zone types overlap. The source block coordinates determine the active zone.

### Sources

A source binds one registered block type to a table. Bindings apply to all blocks of that type, not one world coordinate. The table picker searches both its readable name and system ID. A source also controls inventory size, real elapsed respawn days/hours/minutes/seconds, search sound, safe-zone access, and enabled state. Add a block from the held item or searchable block catalog.

Built-in source bindings cannot be deleted. A custom source can be removed without deleting any world block; it only removes that block type's loot behavior.

### Simulator

Select a table and run 1, 10, 100, or 1000 local generations. The simulator uses the same generator as the server and does not modify world loot. It reports average occupied slots, how often each item appeared, and its average generated quantity. Zone multipliers are intentionally not applied in the base simulation.

### Loot Director

The server stores settings and accumulated data in `config/decimation/loot-director/state.json`. Edit them in the Director tab. Time values in JSON are in milliseconds.

| Key | Default | Effect |
| --- | --- | --- |
| `enabled`, `frozen` | `true`, `false` | Enables the Director and pauses recalculation. |
| `minimumMultiplier`, `maximumMultiplier` | 0.65, 1.25 | Lower and upper bounds of the dynamic multiplier. |
| `adjustmentIntervalMillis` | 900000 | Adjustment interval of 15 minutes. |
| `rollingWindowMillis` | 86400000 | Observation window of 24 hours. |
| `dampingStrength` | 0.20 | Strength of each adjustment. |
| `minimumAttempts` | 10 | Minimum observations required for an adjustment. |
| `targetAvailability` | 0.55 for categories with no override | Target availability by category ID. |

[Loot Director commands](ADMIN_COMMANDS.md#loot)

## Respawn behavior

The timer uses real epoch time and survives restarts. It starts only when a ready source is opened and a batch is generated. Reaching the expiry time marks the batch stale, but visible contents are never replaced while somebody has the container open. After all viewers close it, the next opening discards any stale leftovers, generates a new shared batch, and starts a new timer. Leaving unwanted items behind therefore cannot block future respawns forever.

All players opening the same source instance see the same server inventory and receive normal container slot updates. Breaking or replacing a tracked source block removes its stored state immediately.

## Safe editing notes

- Save requests are revision checked, size limited, rate limited, validated, and written only after server-side verification of operator permissions.
- Large editor snapshots and saves are split into bounded 24 KiB chunks to remain below Minecraft 1.7.10's client custom-payload limit.
- A stale editor cannot overwrite changes saved by another administrator.
- Existing open batches keep the table revision that generated them; configuration reloads affect future generations.
- Keep group and table IDs lowercase and stable when editing JSON manually. The GUI generates IDs automatically.
