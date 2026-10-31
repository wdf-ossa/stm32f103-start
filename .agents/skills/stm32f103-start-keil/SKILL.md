---
name: stm32f103-start-keil
description: Start and extend the blank STM32F103C8 Keil project while preserving RTE/startup setup and requiring board-specific pinout evidence.
---

# STM32F103C8 starter skill

Use for this blank Keil project and the user's second C8 board.

1. The repository contains no original PC13 blink implementation or peripheral drivers.
2. No schematic/pinout for the second board is included. Obtain board docs before assigning pins; do not assume it shares the learning-board wiring.
3. Preserve STM32F103C8, Keil/RTE startup/vector setup, and 64 KB Flash/20 KB RAM unless explicitly asked to migrate.
4. Keep `main.c` minimal; add features in separate modules and add them to the Keil Source Group.
5. Check SWD/JTAG, boot, oscillator, and external-circuit conflicts before selecting a pin.
6. Build with Keil when available. Report compile/download success separately from physical-board behavior.
