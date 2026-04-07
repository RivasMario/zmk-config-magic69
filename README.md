# magic69

![magic69](assets/PXL_20240621_193531119.jpg)

ZMK firmware config for a custom handwired 65% keyboard built on a **SuperMini nRF52840** (nice!nano v2 drop-in).

## Hardware

- **Controller:** SuperMini nRF52840 (`nice_nano//zmk` target)
- **Layout:** 65% ANSI, 69 keys, with navigation cluster
- **Matrix:** 5 rows × 16 cols, col2row diodes

## Building

Firmware is built via GitHub Actions — no local toolchain needed.

1. Push to `master` or trigger manually via **Actions → build**
2. Download `firmware.zip` from the workflow artifacts
3. Unzip to get `nice_nano-magic69.uf2`

## Flashing

1. Double-tap RST to GND on the SuperMini — a `NICENANO` USB drive appears
2. Copy the firmware:
   ```bash
   cp nice_nano-magic69.uf2 /run/media/$USER/NICENANO/
   ```

## Keymap

| Layer | Activation | Contents |
|---|---|---|
| DEFAULT | — | Standard ANSI layout |
| UPPER | Hold FN (bottom row) | F1–F12, media controls, home/end, print screen |
| BT_LAYER | Hold RALT | Bootloader (ESC), BT profile select (top-row last 4), BT clear (PGDN) |

### UPPER layer highlights
- Number row → F1–F12
- INS → Ctrl+Alt+Del
- PGUP/PGDN → Home/End
- Row 3 → Prev / Play-Pause / Next / Stop / Mute / Vol-Down / Vol-Up

### Bluetooth
- Hold RALT to enter BT_LAYER
- Top row last 4 keys: `BT_SEL 0`–`BT_SEL 3` (switch profile)
- PGDN position: `BT_CLR` (clear current profile)
- ESC position: `&bootloader` (enter bootloader mode)
- TX power: +8 dBm
