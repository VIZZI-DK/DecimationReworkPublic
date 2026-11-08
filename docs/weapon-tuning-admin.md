# Weapon tuning administration

The server creates profiles on its first start:

- `config/decimation/weapon-tuning/weapons/<weapon>.json`
- `config/decimation/weapon-tuning/attachments/<attachment>.json`
- `config/decimation/weapon-tuning/manifest.json`

Magazine capacity and ammunition types are intentionally not part of these files.

Creative operators can open the normal attachment screen and press **Weapon editor**. The server checks both creative mode and operator permission. The editor contains:

- **Parameters** — damage, recoil, aimed/base spread, separate hip-fire spread, fire rate, delays, reload phases, attachment modifiers, and intrinsic weapon features;
- **Compatibility** — attachments permitted for a selected weapon;
- **Position** — per-weapon attachment position, rotation, and scale.

Position values are additional offsets relative to the model's existing anchor. Changes entered in the Position tab are previewed on the currently held weapon. **Save** validates and stores the selected profile, increments the server revision, and sends the resulting profiles to every client. **Defaults** restores the original code-defined values in the draft; press **Save** to persist them.

Optical attachments expose two independent first-person ADS values. **Optical view scale** enlarges the sight picture around the eyepiece, while **Eyepiece proximity** moves the held weapon towards or away from the camera during the aiming transition. Neither value changes the world model or the actual magnification/FOV.

Intrinsic weapon switches belong to the weapon profile, not an attachment profile:

- **Integrated suppressor** suppresses the shot, hides the muzzle flash, and uses the normal suppressor heat system. It does not modify recoil, spread, or aiming speed.
- **Integrated flash hider** only hides the muzzle flash.
- **Integrated bipod** uses the same supported-position check and active recoil/spread modifiers as the detachable bipod.

Profiles may also be edited while the server is stopped. Invalid numbers, unsafe identifiers, oversized arrays, and malformed payloads are rejected or clamped during loading and network synchronization.
