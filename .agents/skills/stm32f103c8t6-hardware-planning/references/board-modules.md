# `start` board: known modules and wiring status

Photo-derived observations are recorded in [`docs/start-board-photo-notes.md`](../../../../docs/start-board-photo-notes.md). Treat them as visual evidence, not a substitute for the schematic.

## Confirmed from this repository

| Item | Status | Evidence |
|---|---|---|
| MCU target | STM32F103C8, identified by the Keil target and user-reported STM32F103C8T6 | `start.uvprojx`; user conversation |
| Package target | LQFP48 is implied by the T package suffix in STM32F103C8T6 | ST ordering code / datasheet |
| Framework | Keil µVision project with ARM Compiler 6 and Keil STM32F1 RTE/device pack configuration | `start.uvprojx` |
| Firmware entry | `main.c` is intentionally blank in this repository | `main.c` |
| Physical board model/revision | Unknown | No exact board model/revision or schematic is checked in |
| Visible board parts | USB-C connector, 8.000 MHz crystal, regulator package, `PWR` and `PC13` LED markings, reset switch, BOOT0-labeled jumper block | User-provided product photo; actual unit and circuit details not independently inspected |
| On-board buzzer/display/sensor | Not visible in the provided photo; presence is not established | User-provided product photo only |
| Header labels | GPIO/power labels are visible; the photo notes list readable groups | User-provided product photo; exact connector order should be checked against the physical board |
| Electrical wiring and module pin mapping | Unverified | Requires schematic, board documentation, or continuity measurement |

Do not describe candidate MCU signals as actual board module connections. Do not infer installed modules from the MCU feature set or from a similarly named development board. A visible `PC13` marking near an LED suggests a connection, but does not establish polarity, resistor path, or electrical behavior.

## Module inventory template

Fill one row per physically present module using the schematic, board documentation, or user confirmation. Keep unsupported fields marked `unknown`.

| Module/refdes | Part/model | Power and logic level | Interface | MCU signal-to-pin mapping | Address/polarity | Evidence/source | Verified on hardware |
|---|---|---|---|---|---|---|---|
| Example: temperature/humidity sensor | Unknown | Unknown | Unknown | Unknown | Unknown | Awaiting board schematic | No |
| Example: display | Unknown | Unknown | Unknown | Unknown | Unknown | Awaiting board schematic | No |
| Example: button/LED/buzzer | Unknown | Unknown | Unknown | Unknown | Unknown | Awaiting board schematic | No |

When the board schematic arrives, replace examples with confirmed items and record connector pin names, MCU port pins, active level, pull-ups, supply rail, and source page/sheet. Keep a separate `tested` status for physical validation.
