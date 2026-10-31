# STM32F103C8 Keil 空白模板

本仓库仅保留 Keil/RTE 工程骨架和 Codex skill，不含原 PC13 闪灯逻辑或任何外设驱动。

- `start.uvprojx`：Keil 工程，Source Group 只含空白 `main.c`。
- `RTE/`、`DebugConfig/`：器件启动和 RTE 配置。
- `.agents/skills/stm32f103-start-keil/SKILL.md`：本工程 Codex skill。
- `AGENTS.md`：项目级约定。

尚无第二块板的原理图/引脚图；不可据此模板推断 PC13 接有 LED，也不能照搬另一块板的引脚表。Keil 打开 `start.uvprojx`，确认芯片/Pack/Arm Compiler 后按 F7。
