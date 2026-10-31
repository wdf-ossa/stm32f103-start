---
name: stm32f103c8t6-peripheral-integration
description: Bring up or integrate STM32F103C8T6 clocks, GPIO alternate functions, timers, communication interfaces, interrupts, DMA, USB, or CAN in this Keil starter project.
---

# STM32F103C8T6 peripheral integration

Use for implementing or troubleshooting MCU peripheral initialization and interactions. For pin choice and board wiring, also use `$stm32f103c8t6-hardware-planning`.

## Required context

1. Read `AGENTS.md`, the README, and `start.uvprojx` before changing code. Confirm the selected Arm compiler, RTE components, include paths, clock values, and memory map.
2. Read [the device quick reference](../../../docs/stm32f103c8t6-quick-reference.md) for the device's peripheral limits and [the board photo notes](../../../docs/start-board-photo-notes.md) for visible hardware. Neither replaces RM0008 or the board schematic.
3. Inspect the current source and project groups. The repository is a blank Keil/RTE template; do not assume HAL, CMSIS-Driver, or SPL source files are already selected just because the device supports them.
4. For exact register semantics or peripheral initialization order, consult ST RM0008 and the selected library's official documentation. Do not invent register bits or copy APIs from a different STM32 family.

## Integration rules

- Configure clocks before peripherals. Derive APB and timer clocks from RCC settings, and update `SystemCoreClock`/delay assumptions consistently if the clock tree changes.
- Enable the required peripheral and GPIO clocks, configure each pin's mode/speed/pull/alternate function, and check remap conflicts before enabling the peripheral.
- Keep each driver/service in its own `.c/.h` pair. Define clear init/read/write or start/stop interfaces, bounded timeouts, explicit error returns, and one owner for each peripheral instance.
- Keep interrupt handlers short. Move protocol parsing, display updates, and slow waits to the main loop or a state machine. Use DMA only after defining buffer lifetime and completion/error handling.
- Preserve SWD and reset access during bring-up. Treat boot, oscillator, and special low-current pins according to the datasheet and board photo notes.
- Add every new source/header to the Keil project as appropriate. Avoid modifying vendor RTE bases or generated startup files unless the task requires it.

## Peripheral coverage and routing

The device exposes I2C, USART, SPI, ADC, timers, EXTI/NVIC, DMA, RTC/watchdogs, USB full-speed device, and CAN capabilities. Use only the peripheral instances available on the C8 package and route pins through the hardware-planning Skill. A module name in a request does not prove the board has that module installed.

- I2C: check bus timing, pull-ups, address mode, ACK/NACK, timeout, and bus recovery.
- USART: check TX/RX mapping, baud clock source, framing, and receive overrun handling.
- SPI: check CPOL/CPHA, bit order, frame width, prescaler, and chip-select timing.
- Timer/PWM: calculate period/duty from the actual timer clock; verify channel mapping and external load driver.
- EXTI: configure edge and pending-bit handling; do not do blocking work in the ISR.
- DMA: confirm channel/request mapping, alignment, transfer length, circular mode, and buffer ownership.
- USB/CAN: require their clocking, transceiver/connector circuitry, and physical wiring to be verified before claiming board functionality.

## Completion report

Report the files changed, peripheral instance and candidate/confirmed pins, clock assumptions, Keil build result if built, and the hardware checks still needed. A successful compile or download does not prove the board wiring or protocol works.
