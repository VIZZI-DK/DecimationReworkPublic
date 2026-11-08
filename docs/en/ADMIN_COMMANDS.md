# Decimation Rework Administrator Commands

**English** · [Русский](../ADMIN_COMMANDS_RU.md)

Commands are shown as entered in game chat. Omit the leading slash in the server console. Administrative commands in this reference require operator permissions. **In-game** marks commands that use your position, target, or interface. See the [administrator hub guide](ADMIN_HUB.md) for menu requirements.

## Server and world

| Command | Where | Action |
| --- | --- | --- |
| `/deciserver help` | Chat or console | Shows built-in help. |
| `/deciserver gui` | In-game | Opens the administrator hub. |
| `/deciserver reload` | Chat or console | Reloads `config/decimation/server.properties` and sends clients the updated fog, DM mode, death screen, and current loot source list. |
| `/deciserver check` | Chat or console | Reports that the built-in update check is disabled. |
| `/deciserver npc` | In-game | Opens administration for the NPC you are aiming at within four blocks. |
| `/deciserver power [gui]` | In-game | Opens the city grid controls for the current dimension. |
| `/deciserver zones [gui]`, `/zone gui` | In-game | Opens the zone and flag editor. |
| `/zone list`, `/zone info` | In-game | Lists all zones or shows the zones at your current position. |
| `/zone set <type>` | In-game | Creates a zone of the selected type from your selection. Press Tab for type IDs. Aliases are `create` and `setzone`. |
| `/zone remove` | In-game | Opens the editor. Select a zone and delete it in the GUI. `delete` and `erase` also open the editor. |
| `/zone reload` | In-game | Reloads `zones.json`. The editor's **Reload file** button does the same. |
| `/deciserver zombies [gui]` | In-game | Opens the zombie area, spawn point, and profile editor. |
| `/deciserver playerspawn add` | In-game | Adds a spawn point at your current position. |
| `/deciserver playerspawn undo` | In-game | Removes the last spawn point from the list. |
| `/deciserver playerspawn gui` | In-game | Lists spawn points and provides teleport and delete actions. |
| `/deciserver supplydrop spawn` | In-game | Triggers a supply drop at your current X/Z coordinates in the dedicated server's primary world. |
| `/deciserver supplydrop add`, `undo` | In-game | Adds an X/Z supply drop point or removes the last point. |
| `/deciserver dmkit create <name>` | In-game | Creates a kit NPC using your current inventory. The name can be up to 32 characters. Kits are distributed in DM mode. |
| `/deciserver dmkit remove` | In-game | Removes the kit NPC you are aiming at within six blocks. |
| `/decivehicles reload`, `/decivehicles sync` | Chat or console | Both commands reload vehicle definitions, update loaded vehicles, and synchronize definitions with clients. |
| `/deciserver weapon damage-debug` | In-game | Toggles weapon damage diagnostics for the administrator. |

Legacy `/deciserver` subcommands are supported for scripts. `supplydropspawn`, `addsupplydropspawn`, and `undosupplydropspawn` map to `supplydrop spawn`, `add`, and `undo`. `addplayerspawn` and `undoplayerspawn` map to `playerspawn add` and `undo`.

Player spawns are stored as X/Y/Z coordinates without a dimension. Supply drop points store X/Z for the primary world. The automatic supply drop loader skips the first line of `supplydrop.properties`. With a single point in the list, a manual drop can work while the automatic drop finds no points.

## Loot

| Command | Where | Action |
| --- | --- | --- |
| `/deciserver loot gui` | In-game | Opens the editor and its Director tab. |
| `/deciserver loot reload` | Chat or console | Validates and reloads the loot configuration. If validation fails, the last working configuration stays active. |
| `/deciserver loot cleanup` | Chat or console | Deletes saved state for missing sources in loaded chunks. |
| `/deciserver loot cleanup --load` | Chat or console | Also checks unloaded chunks by loading them in batches. |
| `/deciserver loot director [status]` | Chat or console | Shows Director status and a summary by group. |
| `/deciserver loot director <enable\|disable\|freeze\|unfreeze\|reset>` | Chat or console | Enables or disables the Director, pauses or resumes adjustments, or resets accumulated state. |

[Loot editor and formats](loot-system-admin.md)

## Houses

| Command | Where | Action |
| --- | --- | --- |
| `/deciserver house gui` | In-game | Lists houses and opens management controls. |
| `/deciserver house list` | Chat or console | Shows owners, IDs, and coordinates. |
| `/deciserver house freeze`, `unfreeze` | Chat or console | Pauses or resumes house upkeep. |
| `/deciserver house info` | In-game | Shows details for the house you are aiming at within six blocks. |
| `/deciserver house extend <days>` | In-game | Extends the selected house's term by 1–3,650 days. |
| `/deciserver house setowner <player>` | In-game | Transfers the house to the specified online player. |
| `/deciserver house delete` | In-game | Deletes the house you are aiming at. |

## Players, map, and territories

| Command | Where | Action |
| --- | --- | --- |
| `/deci heal [player]` | Chat, or console with a player name | Heals the specified online player. Omit the name to heal yourself. |
| `/deci caps add [player] <amount>` | Chat, or console with a player name | Adds a positive whole number of caps. |
| `/deci humanity <add\|remove> [amount]` | In-game | Changes your humanity. The default amount is 20. |
| `/deci resetdata` | In-game | Resets your Decimation data. |
| `/deci item` | In-game | Prints the internal name of the item in your hand. |
| `/deci region <pos1\|pos2>` | In-game | Sets a selection corner at your position. |
| `/deci region expand <up\|down>` | In-game | Moves the upper selection coordinate by one block. |
| `/deci deathhistory <player>` | In-game | Opens the server's death history. |
| `/deci map reload` | Chat or console | Reloads the map, regions, and server markers. |
| `/deci map region list` | Chat or console | Lists registered map regions. |
| `/deci map marker gui` | In-game | Opens the map. Enter Creative to show the administrative controls. |
| `/deci map marker list [page]` | Chat or console | Lists markers and their IDs. |
| `/deci map marker add <name> <category> <x> <z>` | In-game | Adds a marker in the current dimension. |
| `/deci map marker remove <id>` | Chat or console | Removes a public marker. |
| `/turf create <name>`, `/turf remove`, `/turf reload` | In-game | Manages legacy turfs. Creation uses your selection, and removal uses the turf at your position. Turfs are stored separately from zones and clan bases. |
| `/screenshot <player>` | In-game | Requests a screenshot from an online player's client and opens it in the GUI. |

`/deci` also supports `addcaps`, `fillcaps`, `addhumanity`, `removehumanity`, `pos1`, `pos2`, `expand.up`, `expand.down`, and `getitem`. The legacy subcommands `setcondition.bleeding`, `setcondition.infected`, and `setcondition.irradiatedd` apply a condition to the sender. `/rspawn` is available to players in zones with the `rspawn` flag while out of combat. `/givemethepowerofdeath` toggles the sender's `haunted` state and is available to regular players.

[Map administration](MAP_ADMIN_GUIDE.md) · [Zone flags](zone-flags.md)

## Traders

Run trader commands in-game. The stock editor also requires Creative mode.

| Command | Action |
| --- | --- |
| `/trader help`, `/trader types [1\|2]` | Shows help or lists base types. |
| `/trader type list` | Lists built-in and custom types. |
| `/trader type add <id> <base_type>` | Creates a custom type based on a built-in type. |
| `/trader type remove <id>` | Removes a custom type. |
| `/trader create <type>` | Records your coordinates and creates the trader in the primary world. |
| `/trader remove` | Removes nearby traders. Aliases are `delete` and `erase`. |
| `/trader reload` | Reloads catalogs, fills in missing default data, and respawns traders. |

## Clans

Run these commands in-game as an operator. Identify a base by its exact name or UUID. Region operations use your current selection.

| Command | Action |
| --- | --- |
| `/clan help`, `/clan base list` | Shows help or lists bases. |
| `/clan base create <name> [size]` | Creates a base in the current dimension. Valid sizes are `TINY`, `SMALL`, `MEDIUM`, `BIG`, and `LARGE`. The default is `SMALL`. |
| `/clan base <setbase\|setprotection\|setattack> <base>` | Replaces the base, protection, or attack area. |
| `/clan base capture <base>` | Requests a capture subject to the capture rules. |
| `/clan base remove <base>` | Archives the data and removes the base. |

[Clan administration, settings, and journal](clan-administration.md)

## Distant terrain generation

`/dcr generate <radius>` builds missing LOD data around the sender. Use `regen` to rebuild existing data. The radius is measured in chunks. For a remote area or the server console, provide the center in block coordinates, for example `/dcr generate 0 0 64`. Add `--generate` to allow the command to generate missing world chunks. `/dcr status` shows progress and `/dcr cancel` stops the job.
