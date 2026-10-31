# STM32F103C8T6 MCU pin matrix (LQFP48)

This is a practical subset of the MCU's pin and alternate-function capabilities, not the board's connector map. Confirm every function and remap against the official ST datasheet and RM0008 before use. The `start` board schematic is not included.

Primary sources:

- [STM32F103x8/xB datasheet](https://www.st.com/resource/en/datasheet/stm32f103c8.pdf), especially the LQFP48 pinout and pin-description tables.
- [RM0008 reference manual](https://www.st.com/resource/en/reference_manual/cd00171190.pdf), especially GPIO/AFIO remapping, ADC, timers, and communication peripherals.

## Package and system facts

- Device family target: STM32F103C8, Cortex-M3, medium-density line.
- Package suffix `T`: LQFP48. Package pin numbers below are for this package only.
- C8 memory target used by this Keil project: 64 KiB Flash and 20 KiB SRAM.
- The STM32F103C8 LQFP48 exposes 10 external ADC channels (PA0-PA7 and PB0-PB1), plus internal ADC channels. Larger package variants may expose more external channels.
- GPIO port pins and peripheral alternate functions are multiplexed. A pin can serve only one selected function at a time.
- Oscillator, reset, BOOT, power, and debug pins are not interchangeable with general board connector labels.

## LQFP48 pins useful for application planning

| GPIO or signal | LQFP48 pin | Common MCU functions and cautions |
|---|---:|---|
| PA0 | 10 | ADC input; TIM2 channel candidate; USART2 flow-control signal |
| PA1 | 11 | ADC input; TIM2 channel candidate; USART2 flow-control signal |
| PA2 | 12 | ADC input; USART2 TX; timer channel candidate |
| PA3 | 13 | ADC input; USART2 RX; timer channel candidate |
| PA4 | 14 | ADC input; SPI1 NSS candidate; GPIO chip-select candidate |
| PA5 | 15 | ADC input; SPI1 SCK |
| PA6 | 16 | ADC input; SPI1 MISO; timer channel candidate |
| PA7 | 17 | ADC input; SPI1 MOSI; timer channel candidate |
| PB0 | 18 | ADC input; timer channel candidate |
| PB1 | 19 | ADC input; timer channel candidate |
| PB2 | 20 | BOOT1-related strap function; check boot configuration before using |
| PB6 | 42 | I2C1 SCL default; USART1 TX when remapped; timer channel candidate |
| PB7 | 43 | I2C1 SDA default; USART1 RX when remapped; timer channel candidate |
| PB8 | 45 | I2C1 SCL remap candidate; timer/CAN functions |
| PB9 | 46 | I2C1 SDA remap candidate; timer/CAN functions |
| PB10 | 21 | I2C2 SCL; USART3 TX; timer channel candidate |
| PB11 | 22 | I2C2 SDA; USART3 RX; timer channel candidate |
| PA8 | 29 | MCO; advanced timer channel candidate |
| PA9 | 30 | USART1 TX; advanced timer channel candidate |
| PA10 | 31 | USART1 RX; advanced timer channel candidate |
| PA11 | 32 | USB DM / CAN RX; advanced timer channel candidate |
| PA12 | 33 | USB DP / CAN TX; advanced timer channel candidate |
| PA13 | 34 | SWDIO/JTMS; reserve for SWD unless debug strategy is intentionally changed |
| PA14 | 37 | SWCLK/JTCK; reserve for SWD unless debug strategy is intentionally changed |
| PA15 | 38 | JTAG signal; can be used by remapped peripheral functions only after checking AFIO/debug configuration |
| PC13 | 2 | RTC/tamper-related pin with special electrical limits; do not assume a board LED is connected |
| PC14 | 3 | LSE oscillator input when fitted; otherwise potentially GPIO subject to configuration |
| PC15 | 4 | LSE oscillator output when fitted; otherwise potentially GPIO subject to configuration |
| PD0 | 5 | HSE oscillator input when fitted |
| PD1 | 6 | HSE oscillator output when fitted |
| BOOT0 | 44 | Boot-mode strap; confirm board pull resistor and boot use before repurposing |
| NRST | 7 | Reset input; preserve reset/debug access |

The matrix intentionally omits a complete pin-function enumeration. Check the official pin table for electrical type, remap alternatives, package availability, and electrical limitations before assigning any pin.

## Common bus routes

| Peripheral | Default/candidate signals | Route notes |
|---|---|---|
| I2C1 | PB6 SCL, PB7 SDA | Remappable to PB8 SCL, PB9 SDA; configure AFIO remap deliberately |
| I2C2 | PB10 SCL, PB11 SDA | Shares pins with USART3 default route and timer functions |
| USART1 | PA9 TX, PA10 RX | Alternate route uses PB6/PB7 and conflicts with default I2C1 pins |
| USART2 | PA2 TX, PA3 RX | PA2/PA3 are also ADC/timer-capable |
| USART3 | PB10 TX, PB11 RX | Conflicts with I2C2 default route |
| SPI1 | PA4 NSS, PA5 SCK, PA6 MISO, PA7 MOSI | Alternate remap involves debug-related pins; preserve SWD and verify AFIO settings |
| ADC1/ADC2 | PA0-PA7, PB0-PB1 | Package analog channels; preserve analog range and pin configuration |

For ADC clock, analog supply/reference, sampling time, and calibration guidance, read [the device quick reference](../../../../docs/stm32f103c8t6-quick-reference.md). Do not use ADC as a GPIO on an analog input pin at the same time.

## Resource-conflict checklist

- A GPIO pin shared by two selected alternate functions is a conflict even when both peripherals are enabled.
- USART1 on PB6/PB7 conflicts with I2C1's default bus assignment.
- I2C2 and USART3 use the same default PB10/PB11 pair.
- SPI remapping can touch debug-related pins; retain the two SWD signals for normal ST-LINK debugging.
- Timer channels may be remapped and may share pins with UART, SPI, I2C, or ADC signals. Check the exact timer remap in RM0008.
- Analog inputs require suitable voltage and source impedance. Never apply a signal above the permitted analog pin range.
- PC13-PC15 and oscillator/boot pins have special use or electrical constraints. Verify the datasheet and actual board circuit before use.
