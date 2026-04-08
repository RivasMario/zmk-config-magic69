# GEMINI.md

This file provides instructional context for Gemini CLI when working in the `zmk-config-magic69` repository.

## Project Overview

This is a **ZMK firmware configuration** for the "magic69" keyboard.
- **Hardware:** Custom handwired 65% keyboard (69 keys) using a **SuperMini nRF52840** (nice!nano v2 alternative).
- **Core Technology:** [ZMK Firmware](https://zmk.dev/) running on Zephyr.

## Building and Flashing

### Building
There is **no local build toolchain**. All builds are handled by GitHub Actions.
- **Target:** Board `nice_nano//zmk`, Shield `magic69`.
- **Workflow:** Push changes to `master` → Actions build → Download `firmware.zip` from artifacts.

### Flashing
1. **Enter Bootloader:** Double-tap `RST` to `GND` on the SuperMini or hold the "BLUE" layer key and tap the ESC position.
2. **Mount:** A `NICENANO` USB drive will appear.
3. **Copy:**
   ```bash
   cp nice_nano-magic69.uf2 /run/media/$USER/NICENANO/
   ```

## Repository Structure

| Path | Description |
|---|---|
| `config/west.yml` | Tracks ZMK `main` branch and other modules. |
| `config/boards/shields/magic69/` | Primary shield configuration directory. |
| `magic69.keymap` | Layer bindings and key behaviors. |
| `magic69.overlay` | GPIO matrix, pin assignments, and matrix transform. |
| `kconfig.conf` | Kconfig settings (BT power, keyboard name, etc.). |
| `build.yaml` | Build matrix configuration for GitHub Actions. |

## Keyboard Layout & Layers

The keymap (`magic69.keymap`) defines three layers:

1. **DEFAULT (0):** Standard 65% ANSI typing layer.
2. **UPPER (1):** Triggered by holding the `FN` key. Includes F1-F12, media controls, and navigation.
3. **BLUE (2):** Triggered by holding the "BLUE" key (top-right, where DEL usually is). Used for hardware management:
   - **BT_SEL 0-3:** Profiles (top row, last 4 keys).
   - **BT_CLR:** Clear current profile (PGDN position).
   - **Bootloader:** ESC position.

## Development Conventions

- **Board Naming:** Use `nice_nano//zmk` for compatibility with Zephyr 4.1+ (do not use `nice_nano_v2`).
- **Matrix:** 5x16 matrix using `col2row` diodes.
- **BT Power:** Configured to `+8 dBm` for stability.
- **Shield Location:** Currently in `config/boards/shields/`. Future work may involve migrating to a standalone ZMK module.
