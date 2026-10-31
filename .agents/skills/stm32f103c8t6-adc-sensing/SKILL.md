---
name: stm32f103c8t6-adc-sensing
description: Plan or implement STM32F103C8T6 ADC acquisition, analog sensor input, calibration, or the internal die-temperature sensor; use when reading analog values or temperature through ADC.
---

# STM32F103C8T6 ADC and sensing

Use for analog inputs or the MCU's internal temperature sensor. For physical header pins and external wiring, also use `$stm32f103c8t6-hardware-planning`.

## Device constraints

Read [the device quick reference](../../../docs/stm32f103c8t6-quick-reference.md) before selecting channels or calculating readings.

- STM32F103C8 in LQFP48 exposes 10 external ADC channels on PA0-PA7 and PB0-PB1. The MCU temperature sensor uses an internal ADC channel and is not a header pin.
- ADC clock must remain within 0.6-14 MHz. ADC analog supply/reference conditions differ from the digital GPIO supply; keep VDDA at or above 2.4 V for ADC operation and within the specified supply limits.
- Use the ADC channel table for this exact package. Do not assume the 16-channel family maximum is present on the C8 LQFP48 package.
- Choose sample time for the source impedance and required resolution. High-impedance sources need longer acquisition time or a buffer.
- Calibrate ADC after power-up as recommended by the datasheet, and discard/average samples only when justified by the sensor noise and response-time requirements.

## Internal temperature sensor

- It reports MCU die temperature, not ambient temperature; CPU load, board heating, and nearby components affect the result.
- The datasheet's typical values are V25 = 1.43 V and average slope = 4.3 mV/°C; specified linearity is up to ±2 °C, and temperature conversion needs a sample time of at least 17.1 μs.
- A first-order estimate is `T ≈ 25 + (V25 - VSENSE) / Avg_Slope`, with voltage in volts and slope in V/°C. Use the exact datasheet formula and note that these are typical values, not per-device calibration constants.
- Do not present the internal sensor as a humidity sensor or as an accurate room thermometer. Use an external temperature/humidity device for environmental measurements.

## Implementation workflow

1. Confirm whether the signal is an external analog sensor or the internal temperature channel.
2. Confirm the sensor's output range, common ground, analog pin, source impedance, filtering, and protection. Keep input voltage inside the ADC range.
3. Configure the ADC clock, channel, sample time, trigger mode, and calibration using the project's existing Keil driver framework. Check whether RTE already provides the required driver before adding dependencies.
4. Use bounded conversion waits and return an error on timeout. For multi-channel or continuous high-rate sampling, evaluate timer triggering and DMA rather than polling in a delay loop.
5. Convert raw ADC code using the actual reference voltage and sensor transfer function. Include offset/gain assumptions and report the expected accuracy.
6. Build with Keil when available, then compare measured values against a known source. Keep compile success separate from analog accuracy and board-level verification.

## Do not assume

- Do not assume every board header pin is free or connected directly to the MCU.
- Do not assume a 5 V-tolerant digital pin is safe as an analog ADC input.
- Do not infer that the board photo's USB-C or LED markings imply an ADC sensor module is populated.
