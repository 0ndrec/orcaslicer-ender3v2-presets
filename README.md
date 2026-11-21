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

### Printer (`presets/machine`)

All three inherit from the system preset **`Creality Ender-3 V2 0.4 nozzle`**.

| Preset | Nozzle | Changes from the base preset |
|---|---|---|
| `Ender-3-Klipper 0.4 nozzle` | 0.4 mm, brass | G-code flavor `klipper`; max Z acceleration 1000/500 mm/s²; max retract acceleration 1500/1000 mm/s² |
| `Ender-3-Klipper 0.4 HGX` | 0.4 mm | Same as above, plus: retraction 3 mm @ 45 mm/s, deretraction 30 mm/s, Z-hop 0.3 mm, Moonraker host type |
| `Ender-3-Klipper 0.6 HGX` | 0.6 mm | Same as the 0.4 HGX preset, with `nozzle_diameter` = 0.6 |

### Process (`presets/process`)

| Preset | Inherits from | Changes |
|---|---|---|
| `0.20mm SpeedUp @Creality Ender3V2` | `0.20mm Standard @Creality Ender3V2` | Outer wall 90 mm/s, inner wall 150 mm/s, sparse infill 220 mm/s, internal solid infill 120 mm/s, top surface 70 mm/s, gap infill 80 mm/s, support 180 mm/s; first layer 25 mm/s (infill 40 mm/s); accelerations: outer wall 1800, inner wall 3800, travel 2500 mm/s²; infill 8 % gyroid |

### Filament (`presets/filament`)

All inherit from the system preset **`Creality Generic PETG`**. Values not listed are inherited.

| Preset | Nozzle temp | First layer temp | Bed temp | Flow ratio | Other |
|---|---|---|---|---|---|
| `Creality Generic PETG (235)` | 250 °C | 250 °C | 60 °C | 1.00 | |
| `Creality Generic PETG (240)` | 240 °C | 260 °C | inherited | 1.02 | |
| `Creality Generic PETG (255)` | inherited | inherited | inherited | inherited | Same as the base profile, saved under its own name |
| `Creality Generic PETG (260)` | 250 °C | 260 °C | inherited | 1.07 | |
| `Creality Speed PETG (255)` | 250 °C | 250 °C | 65 °C | 1.00 | Max volumetric speed 19 mm³/s; temp range 240–255 °C |

> Flow ratios and temperatures were tuned for specific spools on one printer. Run OrcaSlicer's flow-rate and temperature calibrations for your own filament.

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
- **Pick matching combinations.** Use the 0.6 mm printer preset with a 0.6 mm nozzle installed. The `SpeedUp` process works with both nozzle sizes, but check line widths when switching to 0.6.

## Troubleshooting

- **A preset doesn't appear / "parent preset not found":** the Creality Ender-3 V2 system profile or the Creality Generic PETG filament isn't enabled. Enable them (see *Requirements*) and import again.
- **Filament presets are hidden for the Klipper printer:** in the filament dropdown untick *Show compatible only*, or open the filament preset → **Dependencies** and add your printer preset to the compatible printers.
- **Presets were imported but values look wrong:** these are user presets on top of the system profiles, so a different OrcaSlicer version may ship different base values. Check the values listed in the tables above.

## Disclaimer

These presets work on one particular printer. Printers differ: verify temperatures, speeds and accelerations before starting long prints. Use at your own risk.
