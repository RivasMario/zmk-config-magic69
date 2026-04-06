# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

ZMK firmware config for the **magic69** — a custom handwired 65% keyboard running on a **SuperMini nRF52840** (nice!nano v2 drop-in). Build is handled entirely by GitHub Actions; there is no local build toolchain.

## Building

Push to `master` or trigger manually via GitHub Actions → the workflow calls ZMK's shared `build-user-config.yml`. Download `firmware.zip` from the Actions artifacts, unzip to get `nice_nano-magic69.uf2`.

**Build target** (`build.yaml`):
```yaml
board: nice_nano//zmk   # ZMK Zephyr 4.1+ format — NOT nice_nano_v2
shield: magic69
```

## Flashing

Double-tap RST to GND on the SuperMini → `NICENANO` USB drive appears:
```bash
cp nice_nano-magic69.uf2 /run/media/$USER/NICENANO/
```

## Hardware

- **Controller:** SuperMini nRF52840 (Pro Micro footprint, `nice_nano//zmk` target)
- **Matrix:** 5 rows × 16 cols, `col2row` diode direction
- **Row pins:** `pro_micro 1`, `pro_micro 0`, `P1.01`, `P1.02`, `P1.07`
- **Col pins:** `pro_micro 2–9`, `pro_micro 10`, `pro_micro 14–16`, `pro_micro 18–21`
- **Layout:** 65% (69 keys), standard ANSI with nav cluster

## Keymap structure

Three layers defined in `config/boards/shields/magic69/magic69.keymap`:

| Layer | Trigger | Purpose |
|---|---|---|
| `DEFAULT` (0) | — | Normal typing |
| `UPPER` (1) | Hold FN (row 4, col 9) | F1–F12, print screen, home/end |
| `BLUE` (2) | Hold BLUE key (row 1, col 14 — where DEL would be) | Bootloader, BT profile select/clear |

**BT keys** (hold BLUE + tap): last 4 keys of top row = `BT_SEL 0–3`; PGDN position = `BT_CLR`. BT TX power set to +8 dBm in `kconfig.conf`.

## Shield file layout

```
config/boards/shields/magic69/
  magic69.keymap      # Layer bindings
  magic69.overlay     # GPIO matrix, pin assignments, matrix transform
  kconfig.conf        # BT config, keyboard name
  Kconfig.defconfig   # Sets ZMK_KEYBOARD_NAME
  Kconfig.shield      # Shield detection
```

## ZMK version notes

- Tracks ZMK `main` branch via `config/west.yml`
- Zephyr 4.1+ requires `nice_nano//zmk` board format (not `nice_nano_v2`)
- Shield is in `config/boards/shields/` — this is the deprecated location per ZMK; migration to a proper module is a future task
