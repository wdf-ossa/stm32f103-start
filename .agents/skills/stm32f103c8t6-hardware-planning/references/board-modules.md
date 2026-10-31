# `start` board: known modules and wiring status

## Confirmed from this repository

| Item | Status | Evidence |
|---|---|---|
| MCU target | STM32F103C8, identified by the Keil target and user-reported STM32F103C8T6 | `start.uvprojx`; user conversation |
| Package target | LQFP48 is implied by the T package suffix in STM32F103C8T6 | ST ordering code / datasheet |
| Framework | Keil µVision project with ARM Compiler 6 and Keil STM32F1 RTE/device pack configuration | `start.uvprojx` |
| Firmware entry | `main.c` is intentionally blank in this repository | `main.c` |
| Physical board model/revision | Unknown | No board model or schematic is checked in |
| On-board LED/button/buzzer/display/sensor modules | Unknown and unverified | No board-level schematic or module inventory is checked in |
| Header/connector pin mapping | Unknown and unverified | No pinout image or connector map is checked in |

Do not describe candidate MCU signals as actual board module connections. Do not infer installed modules from the MCU feature set or from a similarly named development board.

## Module inventory template

Fill one row per physically present module using the schematic, board documentation, or user confirmation. Keep unsupported fields marked `unknown`.

| Module/refdes | Part/model | Power and logic level | Interface | MCU signal-to-pin mapping | Address/polarity | Evidence/source | Verified on hardware |
|---|---|---|---|---|---|---|---|
| Example: temperature/humidity sensor | Unknown | Unknown | Unknown | Unknown | Unknown | Awaiting board schematic | No |
| Example: display | Unknown | Unknown | Unknown | Unknown | Unknown | Awaiting board schematic | No |
| Example: button/LED/buzzer | Unknown | Unknown | Unknown | Unknown | Unknown | Awaiting board schematic | No |

When the board schematic arrives, replace examples with confirmed items and record connector pin names, MCU port pins, active level, pull-ups, supply rail, and source page/sheet. Keep a separate `tested` status for physical validation.
