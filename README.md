# STM32F103C8 Keil blank template

Clone this repository and open the folder in VS Code. Open the repository root so Codex can read `AGENTS.md` and `.agents/skills/stm32f103-keil-vscode/SKILL.md`.

## Quick start

```powershell
git clone https://github.com/wdf-ossa/stm32f103-start.git
code stm32f103-start
```

Install the recommended extensions shown by VS Code: OpenAI Codex and Microsoft C/C++ IntelliSense. The recommendations are in `.vscode/extensions.json`.

## Build and flash

The Keil project is `start.uvprojx`. VS Code is the editor/AI workspace; use Keil MDK and the STM32F1 device pack to compile and the configured ST-LINK path to flash. This repository does not provide a standalone GCC/CMake build. Preserve the STM32F103C8 target and its startup/RTE/memory settings.

The firmware entry point is intentionally empty. Add only modules needed by the next task. No board schematic is included, so do not assume any particular on-board LED or pin wiring.
