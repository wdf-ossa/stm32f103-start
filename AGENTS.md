# Codex project instructions - STM32F103C8 blank template

- This repository is a blank Keil/RTE skeleton with no PC13 blink code or peripheral drivers.
- The MCU is STM32F103C8T6 in an LQFP48 package. The MCU pin/peripheral candidates are documented in `.agents/skills/stm32f103c8t6-hardware-planning/references/mcu-pin-matrix.md`; these are chip capabilities, not proof of the board's wiring.
- No schematic, board revision, or verified on-board module/pin list for the second board is bundled. Keep physical board connections marked `unverified`; do not reuse the learning-board pin map or infer modules from common Blue Pill designs.
- Preserve STM32F103C8, startup/vector setup, RTE, and 64 KB Flash/20 KB RAM unless asked to change them.
- Keep `main.c` minimal and add future features as separate modules. Report physical validation separately from compilation.
- For requests involving pin choice, module wiring, alternate-function conflicts, or board inventory, use `$stm32f103c8t6-hardware-planning` and read its relevant references before proposing a mapping.
- For routine source/project work, use `$stm32f103-keil-vscode`.
