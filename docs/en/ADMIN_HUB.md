# Decimation Rework Administrator Guide

**English** · [Русский](../ADMIN_HUB.md)

Open **Esc → Administration** or run `/deciserver gui` in chat. The hub and its menus require operator permissions. With a world loaded, you can search the hub by menu name or category, scroll the list, and refresh which entries are available.

See the [command reference](ADMIN_COMMANDS.md) for console operations and the [server settings reference](SERVER_SETTINGS.md) for manual configuration.

## Menus in the hub

The Requirements column lists any additional conditions. To manage an NPC or trader, aim at that entity from within four blocks before opening the hub. The server verifies the target when you select the entry.

| Section | Menu | Requirements | What you can do |
| --- | --- | --- | --- |
| World | Zones and Flags | — | Create a zone from a selection, change its size and dimension, and set its priority, flags, tags, and entry and exit actions. Inspect the effective rules at your position, show boundaries, and teleport. [Flags and actions](zone-flags.md) |
| World | House management | — | View the owner, coordinates, remaining time, fuel, and vehicle count. Teleport to and open a house, extend its term by seven days, change its owner, delete it, or pause upkeep for all houses. |
| World | Municipal electricity | Uses the current dimension | Manage the main power switch, schedule an outage, and configure districts, substations, and load limits. |
| Loot and Zombies | Loot and loot director | — | Configure item groups, tables, sources, and refresh intervals. Run simulations and use the Director tab to adjust dynamic multipliers and view statistics. [Editor and data format](loot-system-admin.md) |
| Loot and Zombies | Zombie Population | — | Create areas, assign profiles and weights, generate points in loaded chunks, disable individual points, and display them in the world. [Population settings](zombie-population.md) |
| NPCs | NPCs, dialogues and quests | Aim at a supported NPC | Bind a dialogue, write lines and responses, configure conditions and memory, attach a quest to a response, and preview branches. Quests can be archived. [Dialogue and conversation objectives](NPC_DIALOGUE_GUIDE.md) |
| NPCs | Trader and catalogue | Creative mode, aim at a trader | Open the trade screen and edit its stock. Catalogs belong to trader types, so editing one affects other NPCs of the same type. |
| Items | Weapon Editor | Creative mode | Adjust damage, recoil, spread, delays, attachment compatibility, and attachment positions. Saving synchronizes the profile with clients. [Settings and preview](weapon-tuning-admin.md) |
| Players | Player spawn points | — | Refresh the list, teleport to a point, or delete it after confirming. Run `/deciserver playerspawn add` to add a point at your position. |
| Players | Clans and administration | — | Find clans, open their details, change verification status, and review settings and the journal. [Clans and recovery](clan-administration.md) |
| Debug | Hitbox Editor | Creative mode | Adjust player hitbox sizes and offsets. The server validates edits, saves the profile, and sends it to clients. |
| Debug | Skeletal box debugging | Creative mode | Open the hitbox visualization and editor. In Creative, **F6** also opens the local debug screen. Saving hitboxes requires server-side permission. |

## Menus and tools outside the hub

| Task | How to open | Details |
| --- | --- | --- |
| Map administration | **M** or `/deci map marker gui` | Administrative actions require Creative mode and operator permissions. Public markers are saved on the server. You can create an icon from an item and teleport from the map. [Markers, regions, and map cache](MAP_ADMIN_GUIDE.md) |
| Weapon tuning from the attachment screen | Open the attachment screen and select **Weapon Editor** | The same permissions apply as when opening it from the hub. |
| Death history | `/deci deathhistory <player>` | Requires operator permissions. The history opens in-game for the administrator. |
| Player screenshot | `/screenshot <player>` | Requires operator permissions. The target must be online. The result opens for the requesting administrator. |
| Area selection | In Creative, right-click a block with the selection tool for the first corner and Shift + right-click for the second | The selection stores a position one block above the clicked block. You can also use `/deci region pos1` and `/deci region pos2`. |
| Road sign or map note text | Right-click the object in Creative | Opens the text editor. Saving requires Creative mode and operator permissions. |
| Prop rotation | In Creative, right-click a supported prop with the rotation tool | The server validates the change. Operator permissions are required. |
| Force power to a device | In Creative, right-click a device with the generator admin tool | Supports generators, lights, spotlights, transformers, and keycard readers. With operator permissions, Shift + right-click opens the **Municipal electricity** menu. |
| Remove a structure during map checks | Right-click the structure with the Creative construction hammer | Targeting any watchtower wall removes the entire tower. The server applies the zone's demolition rules. |

## Common tasks

### Set up a safe zone

1. Select two opposite corners, including the intended vertical range.
2. Open **Zones and Flags**, create a zone from the selection, and verify the dimension.
3. Apply the safe-zone preset. Review the PvP, loot, container, and construction rules individually, then save.
4. Stand inside the zone and open the effective rules tab. Where zones overlap, the tab shows the active value and its source for your position.

### Connect a district to the city grid

1. Select the district boundaries and run `/deciserver power` in the target dimension.
2. Create a named district. Districts in the same dimension cannot overlap.
3. Place a transformer within the district, aim at it from within six blocks, and link it. A newly linked substation starts out damaged. Select **Admin repair**.
4. Set a load limit from 1 to 1,000,000, then enable the district and the main power supply. To schedule an outage, enter a delay in minutes. Set it to `0` to cancel.

### Set up player and supply drop spawn points

1. Stand at the desired player spawn and run `/deciserver playerspawn add`. Use the spawn point menu to verify the teleport destination.
2. Stand at the desired automatic supply drop location and run `/deciserver supplydrop add`. For an immediate drop, use `/deciserver supplydrop spawn`.

### Update a trader's stock

1. Enter Creative, open the NPC's trade screen, and select **Edit**.
2. Hold an item and add it through the editor. For an existing entry, change its prices, reorder it, or confirm its deletion.
3. **Apply my outfit** saves the appearance for the entire trader type. For separate stock or clothing, create a type with `/trader type add` and place an NPC of that type. The commands require operator permissions.

### Inspect player hitboxes

1. Enter Creative and open the editor from the hub or the **F6** menu. Choose a target, stance, and body part.
2. Set XYZ offsets and dimensions in blocks, and set rotation in degrees. The server accepts offsets from −1.5 to 1.5, dimensions from 0.02 to 2, and rotations from −180° to 180°.
3. Check the preview. Resetting one or all hitboxes updates the draft. Select **Save** to apply it on the server.

## If a menu entry is unavailable

- Verify your operator permissions and reopen the hub after any permission change.
- Use Creative mode for weapon editing, hitboxes, and trade editing. For dialogue and trading, check your distance and aim.
- If the editor rejects an outdated revision, refresh the menu and reapply your changes to the current data.
- Use the reload command for the file you changed. `/deciserver reload` reloads the mod's general server settings. Zones, loot, maps, and vehicles have their own commands.
