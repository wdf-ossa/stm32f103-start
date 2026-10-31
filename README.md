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

The firmware entry point is intentionally empty. Add only modules needed by the next task. A user-provided board photo and its visible components/header labels are documented in `docs/start-board-photo-notes.md`; no schematic is included, so treat unseen connections and electrical details as unverified.

## Codex project skills

- `.agents/skills/stm32f103-keil-vscode/SKILL.md`: project discovery, Keil/RTE preservation, module structure, and build/flash workflow.
- `.agents/skills/stm32f103c8t6-hardware-planning/SKILL.md`: STM32F103C8T6 pin/peripheral planning, module interface candidates, and board-wiring evidence rules.
- `docs/start-board-photo-notes.md`: visible parts and silkscreen observations from the user's board photo.

The hardware skill separates MCU-supported pin functions from physical connections. The exact `start` board schematic and its installed module list are not present in this repository, so candidate pins must not be reported as confirmed board wiring.
