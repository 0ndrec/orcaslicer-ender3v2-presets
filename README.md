# OrcaSlicer presets for Creality Ender-3 V2 (Klipper)

Custom [OrcaSlicer](https://github.com/SoftFever/OrcaSlicer) presets for a **Creality Ender-3 V2** running **Klipper + Moonraker**, including variants for an HGX-style extruder and a 0.6 mm nozzle, a faster 0.20 mm process profile and several tuned PETG filament profiles.

All presets are *user presets*: they store only the values that differ from OrcaSlicer's built-in Creality system profiles and inherit everything else from them.

## Contents

```
presets/
├── machine/    printer profiles
├── process/    print (process) profiles
└── filament/   filament profiles
```

### Naming

- **Printers:** `Ender-3 V2 Klipper <nozzle size> <extruder>`. `nozzle` = stock extruder, `HGX` = HGX-style extruder with tuned retraction.
- **Filaments:** `<base filament> <nozzle temperature>C F<flow ratio>`. For example, `Creality Generic PETG 240C F1.02` prints at 240 °C with a flow ratio of 1.02. The first-layer temperature can differ, so check the table below.

### Printer (`presets/machine`)

All three inherit from the system preset **`Creality Ender-3 V2 0.4 nozzle`**. OrcaSlicer has no 0.6 mm Ender-3 V2 base preset, so the 0.6 variant also builds on the 0.4 one and overrides the nozzle diameter.

| Preset | Nozzle | Changes from the base preset |
|---|---|---|
| `Ender-3 V2 Klipper 0.4 nozzle` | 0.4 mm, brass | G-code flavor `klipper`; max Z acceleration 1000/500 mm/s²; max retract acceleration 1500/1000 mm/s² |
| `Ender-3 V2 Klipper 0.4 HGX` | 0.4 mm | Same as above, plus: retraction 3 mm @ 45 mm/s (base: 5 mm @ 60 mm/s), deretraction 30 mm/s (base: 40), Z-hop 0.3 mm (base: 0.4), Moonraker host type |
| `Ender-3 V2 Klipper 0.6 HGX` | 0.6 mm | Same as the 0.4 HGX preset, with `nozzle_diameter` = 0.6 |

### Process (`presets/process`)

| Preset | Inherits from | Changes |
|---|---|---|
| `0.20mm SpeedUp @Creality Ender3V2` | `0.20mm Standard @Creality Ender3V2` | Outer wall 90 mm/s, inner wall 150 mm/s, sparse infill 220 mm/s, internal solid infill 120 mm/s, top surface 70 mm/s, gap infill 80 mm/s, support 180 mm/s; first layer 25 mm/s (infill 40 mm/s); accelerations: outer wall 1800, inner wall 3800, travel 2500 mm/s²; infill 8 % gyroid (base: 15 %) |

### Filament (`presets/filament`)

All inherit from the system preset **`Creality Generic PETG`** (named `Generic PETG @Creality` in newer OrcaSlicer releases, which still accept the old name). Base values: 255 °C nozzle, 255 °C first layer, 80 °C bed, flow ratio 0.95, max volumetric speed 10 mm³/s. Values marked *base* are inherited.

| Preset | Nozzle | First layer | Bed | Flow ratio | Max volumetric speed |
|---|---|---|---|---|---|
| `Creality Generic PETG 255C F0.95` | 255 °C *base* | 255 °C *base* | 80 °C *base* | 0.95 *base* | 10 mm³/s *base* |
| `Creality Generic PETG 250C F1.00` | 250 °C | 250 °C | 60 °C | 1.00 | 10 mm³/s *base* |
| `Creality Generic PETG 250C F1.07` | 250 °C | 260 °C | 80 °C *base* | 1.07 | 10 mm³/s *base* |
| `Creality Generic PETG 240C F1.02` | 240 °C | 260 °C | 80 °C *base* | 1.02 | 10 mm³/s *base* |
| `Creality Speed PETG 250C F1.00` | 250 °C | 250 °C | 65 °C | 1.00 | **19 mm³/s** |

`Creality Speed PETG 250C F1.00` also narrows the allowed temperature range to 240–255 °C.

## Requirements

- OrcaSlicer 2.3 or newer (the presets were created with 2.3.x and are in use with 2.5.x).
- The **Creality** vendor profiles must be installed, with the **Creality Ender-3 V2** printer enabled. The presets inherit from it and will not load without it.
  - In OrcaSlicer: *Printer selector → Add/Remove printers* (or the setup wizard), choose **Creality → Ender-3 V2**, and enable the **Creality Generic PETG** filament.

## Installation

### Option 1: Import from inside OrcaSlicer (recommended)

1. Download this repository (*Code → Download ZIP*) and unpack it, or `git clone` it.
2. In OrcaSlicer open **File → Import → Import Configs…**
3. Select the `.json` files you want from `presets/machine`, `presets/process` and `presets/filament`. You can select several files at once.
4. The presets show up under **User presets** in the printer, process and filament dropdowns.

Import the printer presets first, so the process and filament presets have a compatible printer to attach to.

### Option 2: Copy the files manually

1. Close OrcaSlicer.
2. Copy the folders `presets/machine`, `presets/process` and `presets/filament` into your OrcaSlicer user folder, merging with what is already there:

   | OS | Folder |
   |---|---|
   | Windows | `%APPDATA%\OrcaSlicer\user\default\` |
   | macOS | `~/Library/Application Support/OrcaSlicer/user/default/` |
   | Linux | `~/.config/OrcaSlicer/user/default/` |

   If you are signed in to an OrcaSlicer/Bambu cloud account, the folder may be named after your user ID instead of `default`.
3. Start OrcaSlicer. The presets show up under **User presets**.

## After importing

- **Set your printer's address.** Network settings were removed from these presets. Open the printer preset, go to **Connection** (the Wi-Fi/network icon next to the printer name), choose **Octo/Klipper** as the host type and enter the IP address or hostname of your Moonraker instance.
- **Check the Klipper config.** The presets use the `klipper` G-code flavor. Your start/end G-code and macros (e.g. `PRINT_START`) must match what your Klipper config expects. The start/end G-code is inherited from the stock Ender-3 V2 profile, so adjust it if you use custom macros.

## Recommendations

### Choosing a combination

| Goal | Printer | Process | Filament |
|---|---|---|---|
| Everyday PETG prints | `0.4 nozzle` or `0.4 HGX` | stock `0.20mm Standard @Creality Ender3V2` | `Creality Generic PETG 255C F0.95` as a baseline, then a tuned one |
| Fast prints | `0.4 HGX` | `0.20mm SpeedUp @Creality Ender3V2` | `Creality Speed PETG 250C F1.00` |
| Large, strong parts | `0.6 HGX` | `SpeedUp` or `Standard`, with wider lines (see below) | any PETG preset |

### Speed and volumetric flow

- The `SpeedUp` process asks for roughly 12–14 mm³/s on inner walls and 18–20 mm³/s on sparse infill (150 and 220 mm/s at 0.2 mm layers). The **Generic PETG** presets cap flow at **10 mm³/s**, so OrcaSlicer slows those moves down and `SpeedUp` prints only a little faster than `Standard` with them.
- Only `Creality Speed PETG 250C F1.00` (19 mm³/s) lets `SpeedUp` run close to full speed. Use it only with filament and a hotend that can melt that much. Run **Calibration → Max flowrate** first and lower the value if you see under-extrusion.
- The `SpeedUp` accelerations go up to 3800 mm/s². Klipper caps every move at `max_accel` from `printer.cfg`, so raise that value if it is lower, or the presets will print slower than expected.
- Before using high accelerations, set up **input shaping** in Klipper (`SHAPER_CALIBRATE` with an accelerometer, or a ringing tower). Without it, expect ringing on walls.

### Calibrate each spool

The temperatures and flow ratios here were tuned for specific spools. For a new spool, use OrcaSlicer's **Calibration** menu in this order:

1. **Temperature tower.** PETG usually prints between 230 and 260 °C. Lower temperatures string less, higher ones bond layers better.
2. **Flow rate.** Save the result in your own copy of the filament preset, and include the new value in the name (`F1.xx`) to keep the naming consistent.
3. **Pressure advance.** On Klipper, set it in `printer.cfg` (`[extruder] pressure_advance`) or per filament in OrcaSlicer (*Filament → Enable pressure advance*). Don't do both, or one value overrides the other.
4. **Retraction test.** See the next section.

### Retraction

- The `HGX` presets use 3 mm @ 45 mm/s, which is a middle ground. A direct-drive extruder usually needs only 0.5–1.5 mm, while a Bowden setup needs 4–6 mm. Run **Calibration → Retraction test** and use the shortest length that removes stringing.
- PETG strings more than PLA. If stringing remains after tuning retraction, lower the temperature by 5 °C and dry the filament.
- The 0.3 mm Z-hop helps avoid the nozzle dragging over PETG blobs. You can turn it off for flat, simple parts to save time.

### 0.6 mm nozzle

The process presets use 0.4 mm line widths (inherited from the 0.4 nozzle profile). With the `0.6 HGX` printer preset:

- Set line widths to about 0.6–0.68 mm (*Process → Quality → Line width*) and save a copy of the process preset for the 0.6 nozzle.
- Layer heights up to 0.4 mm work (about 25–75 % of the nozzle diameter).
- Raise the max volumetric speed of the filament (run *Max flowrate* again), because a 0.6 nozzle can push more plastic than the 10 mm³/s Generic PETG limit.

### Bed and adhesion

- PETG can bond too well to smooth PEI and glass and tear the surface when you remove the part. Use a textured PEI sheet, or apply a thin layer of glue stick as a release agent.
- 60–65 °C (the `250C F1.00` and `Speed PETG` presets) is usually enough on PEI. Use 75–80 °C (the base value) for large parts that warp or lift at the corners.
- Use a slower first layer (25 mm/s in `SpeedUp`) and a correct Z offset. Don't squish PETG as much as PLA, or it builds up on the nozzle.

### Filament care

- PETG absorbs moisture. If you hear popping, see heavy stringing or get a rough surface, dry the spool at about 65 °C for 4–6 hours and store it with desiccant.
- Clean PETG residue off the nozzle regularly. It picks up burnt plastic and drops it onto prints.

## Troubleshooting

- **A preset doesn't appear / "parent preset not found":** the Creality Ender-3 V2 system profile or the Creality Generic PETG filament isn't enabled. Enable them (see *Requirements*) and import again.
- **Filament presets are hidden for the Klipper printer:** in the filament dropdown untick *Show compatible only*, or open the filament preset → **Dependencies** and add your printer preset to the compatible printers.
- **Presets were imported but values look wrong:** these are user presets on top of the system profiles, so a different OrcaSlicer version may ship different base values. Check the values listed in the tables above.

## Disclaimer

These presets work on one particular printer. Printers differ: verify temperatures, speeds and accelerations before starting long prints. Use at your own risk.
