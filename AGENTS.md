# Codex project instructions - STM32F103C8 blank template

- This repository is a blank Keil/RTE skeleton with no PC13 blink code or peripheral drivers.
- The MCU is STM32F103C8T6 in an LQFP48 package. The MCU pin/peripheral candidates are documented in `.agents/skills/stm32f103c8t6-hardware-planning/references/mcu-pin-matrix.md`; these are chip capabilities, not proof of the board's wiring.
- A user-provided product photo is documented in `docs/start-board-photo-notes.md`. It identifies visible board parts and silkscreen labels only; no schematic or board revision is bundled, so circuit connections and electrical details remain `unverified`.
- Do not reuse the learning-board pin map or infer hidden circuitry from a similar-looking board. Use the photo notes for visible parts and the hardware-planning Skill for MCU-side candidates.
- Preserve STM32F103C8, startup/vector setup, RTE, and 64 KB Flash/20 KB RAM unless asked to change them.
- Keep `main.c` minimal and add future features as separate modules. Report physical validation separately from compilation.
- For requests involving pin choice, module wiring, alternate-function conflicts, or board inventory, use `$stm32f103c8t6-hardware-planning` and read its relevant references before proposing a mapping.
- For requests implementing or bringing up I2C, SPI, USART, timers/PWM, EXTI, DMA, clocks, or watchdogs, use `$stm32f103c8t6-peripheral-integration`.
- For requests involving analog sensors, ADC setup, or the MCU's internal temperature sensor, use `$stm32f103c8t6-adc-sensing`.
- For requests to coordinate editing in VS Code/Codex with Keil build, debug, or download, use `$stm32f103-vscode-keil-workflow`.
- For routine source/project work, use `$stm32f103-keil-vscode`.
- Keep firmware edits local by default. Do not commit or push user code unless the user explicitly asks for that Git operation.
