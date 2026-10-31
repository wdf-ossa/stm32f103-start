---
name: stm32f103c8t6-hardware-planning
description: Plan STM32F103C8T6 pin assignments and common peripheral modules for this Keil starter; use when selecting GPIO, buses, ADC, timers, or checking board wiring and pin conflicts.
---

# STM32F103C8T6 pin and module planning

Use this skill for pin selection, peripheral routing, module planning, or board-hardware inventory in this repository.

## Evidence boundary

- The project target is STM32F103C8 in LQFP48, and the user identifies the chip as STM32F103C8T6.
- A user-provided product photo is summarized in `docs/start-board-photo-notes.md`; this repository still does not include the physical board schematic, exact revision, or verified electrical module map.
- The pin matrix is derived from the MCU package and ST documentation. It describes silicon capability only; it does not establish which pins are routed to headers, LEDs, buttons, sensors, or other board parts.
- Use `docs/stm32f103c8t6-quick-reference.md` for package-specific ADC limits, special-pin limits, and other datasheet facts. The C8/LQFP48 variant has 10 external ADC channels; do not assume the family maximum applies to this package.
- Do not copy the learning-board mapping or assume unseen wiring from a similar board. Keep board-level wiring `unverified` until supported by a schematic, board documentation, or user confirmation.
- The listed routes are useful examples, not a complete alternate-function catalog.

## Before proposing a pin map

1. Read [photo observations](../../../docs/start-board-photo-notes.md) and [board inventory and module status](references/board-modules.md).
2. Read [MCU pin matrix](references/mcu-pin-matrix.md) for candidate functions and package constraints.
3. Inspect the project's current source, RTE settings, clock setup, and `.uvprojx` before selecting a peripheral instance or changing clocks.
4. Ask for the board schematic or exact module/connector pinout when a task requires confirmed physical connections, voltage levels, pull-ups, or active polarity. Continue with architecture or a clearly labeled candidate map when possible.
5. Check conflicts across the whole design: shared pins, alternate-function remaps, timer-channel overlap, ADC use, oscillator pins, BOOT pins, and debug access. Do not assign two active functions to the same pin.
6. Put confirmed board wiring in a maintained board reference and named C macros in a board/pin header. Keep guessed routes out of production definitions.

## Common module interfaces

Use these only as MCU-side candidate routes. Confirm module voltage, logic levels, pull-ups, bus address, connector order, and board wiring independently.

| Module or function | MCU interface candidates | Information to confirm before implementation |
|---|---|---|
| I2C temperature/humidity sensor, e.g. AHT20 | I2C1 default PB6 SCL / PB7 SDA; I2C1 remap PB8 SCL / PB9 SDA; or I2C2 PB10 SCL / PB11 SDA | Exact sensor model, address, supply/logic levels, pull-ups, bus sharing, and actual connector pins |
| I2C OLED | Same I2C candidates; share a bus only when voltage and pull-ups are compatible | Controller, I2C/SPI variant, address, resolution, reset pin, and connector order |
| SPI display or serial flash | SPI1 default PA4 NSS, PA5 SCK, PA6 MISO, PA7 MOSI; chip-select may be a separate GPIO when the device supports it | Device protocol, voltage, chip-select/reset/DC pins, bus sharing, and actual routing |
| UART module or serial console | USART1 PA9 TX / PA10 RX; USART2 PA2 TX / PA3 RX; USART3 PB10 TX / PB11 RX | TTL versus RS-232/RS-485 level, cross-connected TX/RX, baud rate, connector pins, and whether the board already uses that UART |
| Analog sensor | ADC1/ADC2 channels on PA0-PA7, PB0-PB1 | Output range, source impedance, analog supply/reference, divider/filter, channel availability, and input protection |
| PWM buzzer, servo, or dimmable load | A timer output candidate selected from the MCU alternate-function table; GPIO alone is not a power driver | Timer/channel mapping, frequency/resolution, output polarity, load current, transistor/driver stage, and pin availability |
| Button or digital switch | Any available GPIO input; EXTI can be used when interrupt notification is needed | Active level, external/internal pull, debounce, shared EXTI line, and physical switch pin |
| LED | Any verified GPIO route; a board LED may be active-low | Actual resistor/driver path, polarity, current, and board pin |

Do not infer a module from an MCU peripheral. For example, I2C support does not mean an I2C sensor is installed on this board.

## Pin definition convention

- Prefer one board-specific header such as `board_pins.h` for confirmed physical assignments, with names like `SENSOR_I2C_SCL_PORT` and `SENSOR_I2C_SCL_PIN`.
- Keep peripheral bus setup in its driver/BSP module and application policy in `main.c` or an application module.
- Comment each mapping with its evidence source and polarity where relevant. Mark temporary candidate routes as `CANDIDATE` and do not silently turn them into board facts.
- If changing pin remap or debug configuration, state the affected signals and preserve SWD unless the user explicitly requests otherwise.
- PC13-PC15 have unusually low output capability. Follow the exact limits summarized in the quick reference; do not drive an LED as a sourcing output or assume a high GPIO speed is safe.

## References

- [MCU pin matrix](references/mcu-pin-matrix.md): package GPIOs, common bus candidates, debug/boot/oscillator constraints.
- [Board modules](references/board-modules.md): confirmed facts, unknown wiring, and the inventory template to fill when board documents are provided.
- [Board photo notes](../../../docs/start-board-photo-notes.md): visible parts and pin-header silkscreen groups; not an electrical schematic.
- [STM32F103C8T6 quick reference](../../../docs/stm32f103c8t6-quick-reference.md): data-manual facts for this exact density/package.
- [ST STM32F103C8 datasheet](https://www.st.com/resource/en/datasheet/stm32f103c8.pdf).
- [ST RM0008 reference manual](https://www.st.com/resource/en/reference_manual/cd00171190.pdf).
