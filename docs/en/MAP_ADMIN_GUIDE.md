# World Map Administration

**English** · [Русский](../MAP_ADMIN_GUIDE_RU.md)

Configure the Decimation Rework server map, detailed regional overlays, public markers, and icons from items or HTTPS URLs.

## Contents

1. [Quick start](#quick-start)
2. [Player access](#player-access)
3. [File layout](#file-layout)
4. [Base map configuration](#base-map-configuration)
5. [Detailed regional maps](#detailed-regional-maps)
6. [Server markers](#server-markers)
7. [Item marker icons](#item-marker-icons)
8. [Managing the map in the GUI](#managing-the-map-in-the-gui)
9. [Commands](#commands)
10. [Player controls](#player-controls)
11. [Image caching](#image-caching)
12. [Troubleshooting](#troubleshooting)

## Quick start

1. Start the server once. The mod creates `config/decimation/map` and any missing configuration files.
2. Upload the map image to a public HTTPS host.
3. Fill in `config/decimation/map/map.json`.
4. Run `/deci map reload` with operator permissions or restart the server.
5. Carry a map item and press `M`.

Minimal configuration:

```json
{
  "schemaVersion": 1,
  "revision": 1,
  "maps": [
    {
      "id": "dayz_main",
      "name": "DayZ Map",
      "dimension": 0,
      "imageUrl": "https://dcmre.work/serverfiles/maps/main-map.png",
      "imageSha256": "",
      "minX": -4000,
      "maxX": 4000,
      "minZ": -4000,
      "maxZ": 4000,
      "defaultZoom": 1.0,
      "minZoom": 0.5,
      "maxZoom": 3.0
    }
  ]
}
```

Enter the raw URL as a JSON string.

## Player access

The main setting is in `config/decimation/server.properties`:

```properties
requireMapItem=true
```

- `true` requires regular players to carry a map item in any inventory slot to open the full-screen map.
- `false` lets players open the map without an item.
- Operators in Creative can open the map without an item with either setting.

Press `M` to open the map. You can change the key in Minecraft's controls settings.

The map item also displays the current server map on its model. Right-click while holding it to cycle through `1x`, `2x`, and `4x`. The selected zoom is kept in client memory rather than item data.

## File layout

```text
config/
└── decimation/
    ├── server.properties
    └── map/
        ├── map.json
        ├── markers.json
        ├── icons.json
        └── regions/
            ├── city_center.json
            └── military_base.json
```

- `map.json` defines base maps by dimension.
- `regions/*.json` defines detailed images for individual areas.
- `markers.json` stores persistent server markers.
- `icons.json` holds the catalog of icons created from item stacks. Manage it through the GUI.

Saves keep a backup copy. Keep the `.backup` file when recovering a damaged configuration.

## Base map configuration

Configure at most one base map per dimension and up to 16 maps in total.

### Map fields

| Field | Purpose |
| --- | --- |
| `schemaVersion` | Format version. Use `1`. |
| `revision` | Internal document revision. Leave it unchanged in manual edits. |
| `id` | Unique system ID of up to 64 lowercase Latin letters, digits, underscores, hyphens, and dots. |
| `name` | Display name of up to 96 characters. |
| `dimension` | Dimension ID. The Overworld is `0`, the Nether is `-1`, and the End is `1`. Third-party dimensions can use other IDs. |
| `imageUrl` | Direct public HTTPS URL for the image. |
| `imageSha256` | Optional file SHA-256 hash of 64 lowercase hexadecimal characters. An empty string is allowed. |
| `minX`, `maxX` | Left and right image edges in world X coordinates. |
| `minZ`, `maxZ` | Top and bottom image edges in world Z coordinates. |
| `defaultZoom` | Zoom when opening the map. |
| `minZoom`, `maxZoom` | Minimum and maximum zoom. |

The left and right image edges map to `minX` and `maxX`. The top and bottom edges map to `minZ` and `maxZ`. Match the aspect ratio to `(maxX - minX) : (maxZ - minZ)` to avoid stretching the image.

### Image limits

- Use an absolute public HTTPS URL.
- Local, private, and loopback addresses are blocked.
- The maximum download size is 16 MiB.
- The maximum resolution is 8192 × 8192 pixels.
- An optional SHA-256 hash lets the client verify file integrity.

If you replace an image without a SHA-256 hash, change its URL, for example to `main-map-v2.png`, so clients fetch the updated file.

## Detailed regional maps

A regional map overlays a detailed image on part of the base map at higher zoom levels. It leaves gameplay zone rules unchanged. Store each region in its own JSON file under `config/decimation/map/regions`. The server supports up to 32 files.

Example `regions/city_center.json`:

```json
{
  "schemaVersion": 1,
  "id": "city_center",
  "name": "City Center",
  "dimension": 0,
  "imageUrl": "https://dcmre.work/serverfiles/maps/city-center.png",
  "imageSha256": "",
  "minX": -800,
  "maxX": 600,
  "minZ": -500,
  "maxZ": 900,
  "zoomThreshold": 1.8,
  "priority": 10,
  "enabled": true
}
```

- `zoomThreshold` is the minimum zoom at which the detail image appears.
- `priority` controls overlay order. Higher-priority regions draw later, covering lower-priority ones.
- `enabled` lets you disable a region while keeping its file.

Use the same coordinate system as the base map for that dimension. The GUI's **Display zones** toggle outlines all enabled overlays. Run `/deci map reload` after creating or editing a file. Use `/deci map region list` to check loaded regions.

## Server markers

All players can see server markers. The server saves them in `config/decimation/map/markers.json` and synchronizes them with clients. Use the GUI for individual markers or edit the file for bulk changes.

Example:

```json
{
  "schemaVersion": 1,
  "revision": 1,
  "markers": [
    {
      "id": "central_trader",
      "dimension": 0,
      "x": 120.0,
      "z": -340.0,
      "category": "Traders",
      "title": "Central Trader",
      "description": "Weapons, ammo, and gear",
      "iconUrl": "",
      "iconSha256": "",
      "visible": true,
      "priority": 10,
      "size": 1.5,
      "highlighted": true
    }
  ]
}
```

- The server supports up to 2,000 persistent markers.
- `id` must be unique and follow the base map ID rules.
- `category` accepts up to 48 characters, `title` up to 96, and `description` up to 512.
- Coordinates must fall within the base map for the marker's dimension.
- `visible=false` hides the marker.
- Higher `priority` places the marker higher in the sorted list.
- `size` ranges from `0.5` to `3.0`. Older entries without a size use `1.0`.
- `highlighted=true` adds a pulsing orange highlight.

`iconUrl` accepts a saved item icon reference (`item:<id>`) or a direct public HTTPS URL for a PNG or JPG. For URL icons, leave `iconSha256` empty. The client accepts images up to 2 MiB and 1024 × 1024 pixels, blocks localhost and private addresses, and caches the downloaded image. Leave both fields empty for a marker without an icon.

## Item marker icons

The catalog stores the full `ItemStack`, including NBT and weapon modifications, with its count set to one.

### Add an icon

1. Enter Creative with operator permissions.
2. Hold the item and open the map with `M`.
3. Right-click an empty spot on the map and choose **Create server marker**.
4. In the icon editor, select **Add held item** and give the icon a recognizable name.
5. Select the saved icon and save the marker.

The catalog supports up to 128 custom icons. Multiple markers can share one icon.

### Edit or remove an icon

Right-click an icon in the marker editor. Rename it, replace it with the item in your hand while keeping its ID and marker links, or confirm its deletion. If you delete an icon or remove the mod that supplied its item, linked markers use the default dot.

`icons.json` stores serialized NBT in Base64. The GUI saves and synchronizes catalog changes. If you edit the file manually, restart the server to reload the catalog. `/deci map reload` only reloads maps, regions, and markers.

## Managing the map in the GUI

Creative mode shows the administrative controls. The server checks operator permissions for each change.

### Create a marker

1. Open the map with `M` or `/deci map marker gui`.
2. Right-click an empty spot and choose **Create server marker**.
3. Enter the title, category, description, and coordinates. You can fill in your current coordinates with the position button.
4. Select a catalog icon, switch to the URL tab and enter a direct HTTPS URL, or leave the marker without an icon.
5. Set the size, optionally enable highlighting, and save.

The editor generates the ID automatically.

### Edit or remove a marker

Right-click a server marker and select **Edit marker** or **Delete marker**. Deletion requires confirmation.

### Teleport

In Creative, right-click a location and choose **Teleport here**. The server requires operator permissions. It finds a safe height within the current dimension and the map bounds.

## Commands

`/deci map` commands and administrative map actions require operator permissions.

| Command | Purpose |
| --- | --- |
| `/deci map reload` | Reloads `map.json`, regional files, and `markers.json`. |
| `/deci map region list` | Lists loaded regions and their dimensions. |
| `/deci map marker gui` | Opens the map. Enter Creative to show administrative controls. |
| `/deci map marker list [page]` | Lists ten persistent markers per page. |
| `/deci map marker add <name> <category> <x> <z>` | Creates a simple marker in the current dimension. |
| `/deci map marker remove <id>` | Removes a persistent marker by ID. |

Use short values without spaces with `marker add`. Use the GUI for full titles, descriptions, item icons, or URL icons. Press Tab to complete subcommands and marker IDs for removal.

## Player controls

### Full-screen map

- Press `M` to open the map.
- Left-click and drag to pan, or use the mouse wheel to zoom.
- Press `Space` to reset zoom and center the map on your position.
- Right-click the map for actions. Use **Set personal marker** to place a personal GPS marker.
- The server tab shows persistent and dynamic server markers. The client tab shows your GPS marker and death marker.
- Search by category, title, or description.
- Left-click a category to expand or collapse it. Right-click to temporarily hide or show its markers.
- Left-click a marker in the list to center the map and set your GPS marker there.
- Use the cross in the client tab to remove your personal marker.

Personal GPS markers are stored on the client and visible only to you.

### Map item

Hold the item to see the surrounding area. A rotating arrow shows your position. Right-click to cycle `1x → 2x → 4x → 1x`. The item uses the same base map as the full-screen view for the current dimension.

## Image caching

Clients download images directly over HTTPS and cache them separately for each server in `decimation/cache/maps`. The server sends the configuration, URLs, and checksums. Host the files where every player can download them.

- Use PNG or another format supported by Minecraft and Java ImageIO.
- Change the URL or SHA-256 hash when replacing an image.
- Use a direct file URL.
- Verify the HTTPS certificate and confirm that downloads work without authentication.
- Optimize image size to reduce loading time when players join.

The client limits downloads to 16 MiB and 8192 × 8192 pixels, and enforces redirect and timeout limits. If an image is corrupt or unavailable, the client shows a fallback or leaves the map unavailable.

## Troubleshooting

### The map does not open

1. Check for a map item in your inventory or temporarily set `requireMapItem=false`.
2. Check that `map.json` has an entry for the current `dimension`.
3. Run `/deci map reload` and read the chat or console output.
4. Check for trailing commas, Markdown links, and invalid quotation marks in the JSON.

### The image is missing

1. Open `imageUrl` in a browser without signing in.
2. Check that it uses HTTPS and points directly to the image file.
3. Check the 16 MiB and 8192 × 8192 limits.
4. If you set `imageSha256`, recalculate it after replacing the file.
5. If the image has no hash, change the filename or URL to bypass the old client cache.

### The map is offset or stretched

1. Check the actual boundary coordinates `minX`, `maxX`, `minZ`, and `maxZ`.
2. Confirm that the top of the image, or north, corresponds to `minZ`.
3. Compare the image's aspect ratio with the coordinate bounds.
4. Create temporary markers at known coordinates and compare them with the image.

### A region does not appear

1. Check `enabled=true`.
2. Confirm that `dimension` matches the base map.
3. Zoom in to at least `zoomThreshold`.
4. Check the region bounds and direct HTTPS URL.
5. Run `/deci map region list` and `/deci map reload`.

### Administrative actions are missing or rejected

- Enter Creative to show the controls.
- Verify your operator permissions.
- Keep marker coordinates within the base map.
- Teleport within the current dimension.
- Restart after manually editing item icons.

### Update configuration safely

1. Back up `config/decimation/map`.
2. Edit the relevant JSON files and validate their syntax.
3. Run `/deci map reload`.
4. If the server reports an error, it keeps the working configuration. Correct the file and retry.
5. Ask players who have the map open to close and reopen it to refresh the view.
