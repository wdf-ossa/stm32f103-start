# STM32F103C8T6 quick reference for firmware skills

This is a short working summary for Codex, not a replacement for the complete datasheet or reference manual. The user-supplied Chinese manual identifies itself as a translation of an older English edition and warns that translation errors are possible. For final hardware decisions, compare against the current ST English documentation and the exact device/package.

Primary source: [ST STM32F103x8/xB datasheet](https://www.st.com/resource/en/datasheet/stm32f103c8.pdf). Peripheral register behavior and initialization sequences belong to [RM0008](https://www.st.com/resource/en/reference_manual/cd00171190.pdf).

## Device and package limits

- STM32F103C8 is a medium-density Cortex-M3 device. The project targets 64 KiB Flash and 20 KiB SRAM; its maximum core clock is 72 MHz.
- The `T6` ordering code uses the LQFP48 package. The C8/LQFP48 pin table exposes 10 external ADC inputs: PA0-PA7 and PB0-PB1. Family/package maximums must not be substituted for this part's available pins.
- The operating supply range is 2.0-3.6 V. ADC operation requires VDDA of at least 2.4 V. For LQFP48, the reference inputs are internally tied to the analog supply/ground; there is no separate external reference pin.
- Only pins marked FT in the package pin table are 5 V tolerant. Never infer tolerance for a pin from another port or package.

## Clock, boot, and debug

- Reset starts from the internal 8 MHz HSI clock. The board photo shows a crystal marked `8.000`, consistent with an 8 MHz HSE, but firmware must explicitly configure HSE/PLL to use it.
- HSE is specified for the 4-16 MHz range. System clock may reach 72 MHz; APB1 is limited to 36 MHz and APB2 to 72 MHz. Derive timer and peripheral clocks from the configured tree rather than assuming the core clock.
- If USB full-speed device operation is used, the datasheet requires HSE plus PLL and a 48 MHz USB clock; the CPU clock configuration must be compatible with the USB clocking requirements.
- Preserve SWD pins and NRST for ST-LINK debugging. Peripheral remapping can overlap debug pins; check AFIO configuration and the actual board before changing debug mode.
- BOOT0 and the related boot configuration choose Flash, system memory, or SRAM boot. The photo shows a BOOT0-labeled jumper block, but its exact strap truth table and default position are not confirmed.

## GPIO and special pins

- GPIO supports input, pull-up/pull-down, push-pull/open-drain output, and alternate-function use. Configure mode, speed, pull, and alternate-function mapping to match the electrical interface.
- A 5 V tolerant digital input is not permission to drive every analog or special-function pin with 5 V. Check the FT flag and the datasheet electrical limits for the exact pin.
- PC13-PC15 are special low-current pins: the manual specifies up to 3 mA sink capability, only one of the three used as an output at a time, 2 MHz output mode, and a maximum 30 pF load. They cannot source current. The board photo's PC13 LED marking therefore needs low-current, polarity-aware handling and verification against the circuit.
- The visible PC14/PC15 pins can be used for the low-speed oscillator when fitted; PD0/PD1 are associated with the high-speed oscillator after reset. Do not assign them to GPIO without checking the board oscillator circuit and device remap rules.

## Peripheral selection facts

| Peripheral | Device capability useful during planning | Skill guidance |
|---|---|---|
| EXTI/NVIC | GPIO inputs can map to 16 external interrupt lines; edge selection and masking are configurable | Avoid long work in ISRs; queue a flag/event and handle it in the main loop |
| Timers | TIM1 advanced-control timer plus TIM2-TIM4 general-purpose 16-bit timers; capture/compare/PWM use GPIO alternate functions | Check channel-to-pin mapping, remap, timer clock, polarity, and load driver |
| I2C | Up to two interfaces; standard and fast modes; 7-bit and 10-bit addressing | Confirm bus pins, pull-ups, address, voltage compatibility, and timeout/recovery behavior |
| USART | Up to three USARTs on the C8 medium-density device | Confirm TX/RX pin mapping, baud clock error, logic level, and serial adapter wiring |
| SPI | Up to two interfaces on the C8 medium-density device; full-duplex master/slave | Confirm mode 0-3, word size, baud prescaler, chip select, and shared-bus behavior |
| ADC | Two 12-bit ADCs; C8/LQFP48 has 10 external channels, plus internal channels | Keep ADC clock within 0.6-14 MHz; use sufficient sample time for the source impedance and calibrate after power-up |
| DMA | Seven channels; available for ADC and several communication/timer peripherals | Check channel/request mapping and buffer ownership before enabling concurrent transfers |
| USB FS | Device controller; requires HSE/PLL-derived 48 MHz clock | A USB-C receptacle on the board photo does not prove that D+/D- are routed to the MCU |
| CAN | CAN 2.0A/B controller | The MCU needs an external CAN transceiver and verified board wiring; the MCU pins are not a bus-level CAN interface |

The exact pin mapping, remap options, and package availability come from the pin-definition table and RM0008. The peripheral names above do not establish that a physical module or transceiver is fitted to the board.

## ADC and internal temperature sensor

- The ADC clock range is 0.6-14 MHz. The conversion range is ground/VSSA to VREF+; on this package VREF+ follows VDDA.
- ADC precision depends on VDDA, reference quality, source impedance, sampling time, and board layout. Use longer sampling time for high-impedance sources. The manual recommends one ADC calibration at each power-up.
- The internal temperature sensor is connected to an internal ADC channel, not a board header. At 25 °C, the manual gives a typical VSENSE of 1.43 V and average slope of 4.3 mV/°C; the linearity range is up to ±2 °C. The specified sample time for temperature readings is at least 17.1 μs.
- The internal sensor measures die temperature and is affected by MCU self-heating and nearby heat sources. It is suitable for rough MCU temperature monitoring, not accurate ambient temperature or humidity measurement. Use an external sensor for room/environment readings.
- Treat V25 and average slope as typical characterization, not per-chip calibration. State expected error and validate the conversion formula against the exact datasheet/revision.

## Source note

The attached Chinese PDF is a translated STM32F103x8/xB datasheet and is useful for quick lookup. It is not the board schematic and does not document the `start` board's module-to-pin wiring. For register-level driver implementation, use the reference manual and the selected Keil driver framework.
