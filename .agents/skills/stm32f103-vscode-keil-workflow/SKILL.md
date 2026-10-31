---
name: stm32f103-vscode-keil-workflow
description: Coordinate STM32F103 firmware work where VS Code/Codex edits source and Keil µVision builds, debugs, and downloads the same project checkout.
---

# VS Code source editing with Keil build/debug/download

Use this skill when the user wants VS Code/Codex for writing firmware and Keil MDK for compiling, on-target debugging, and programming the STM32F103 board.

## One project folder

- Open the repository root in VS Code and open that exact checkout's `start.uvprojx` in Keil. Both applications must point to the same folder.
- Do not create a second working copy for Keil. Before build or download, confirm the `.uvprojx` path in Keil matches the VS Code workspace path; this prevents building one copy and flashing a stale copy.
- Preserve the project's STM32F103C8 target, Arm Compiler selection, RTE/startup files, Flash/RAM map, and ST-LINK/SWD configuration unless the user asks to change them.

## Responsibilities by application

- **VS Code + Codex:** inspect requirements and source, edit `main.c` and reusable `.c/.h` modules, update the Keil project file/group when adding source files, review diffs, and keep changes under Git.
- **Keil µVision:** compile the `.uvprojx` target, inspect compiler/linker output, start an ST-LINK debug session, run/step and inspect variables/registers, then download and verify the firmware on the board.
- VS Code editing or IntelliSense alone does not compile or flash this project. Do not introduce a separate GCC/CMake build path unless requested.

## Edit → build → debug → download loop

1. **Inspect first.** Read `AGENTS.md`, the relevant project Skill, existing source, and `start.uvprojx`. Check device, compiler, RTE components, source groups, clocks, memory settings, and current debug/programmer selection.
2. **Make changes in VS Code.** Keep `main.c` focused on application flow. Put each reusable peripheral/service in its own module. For every new `.c` file, make sure it is included in the Keil target; a file visible in Explorer is not necessarily part of the build.
3. **Save and review.** Save all files. Review the Git diff for unintended changes to generated RTE, startup, linker, or user-specific Keil settings.
4. **Build in Keil.** Switch to the same project in µVision and build the intended target (normally F7). If a new source file is missing from the project tree, add it to the correct group and build again. Fix source in VS Code, then rebuild in Keil.
5. **Debug in Keil.** Use the configured ST-LINK/SWD target. Set breakpoints around initialization and the feature under development; inspect return codes, variables, and relevant peripheral registers. Keep ISR work short and avoid long blocking waits while debugging timing-sensitive code.
6. **Download in Keil.** Use the project's configured Flash algorithm and ST-LINK settings. Confirm erase/program/verify results in the Keil output before judging firmware behavior.
7. **Check the board.** Verify the requested behavior on the physical board. Record what compiled, what downloaded, what was observed, and what remains unverified as separate results.

## When Keil reports an error

- Capture the first actual error and its file/line, not only the final `Target not created` summary.
- For missing symbols/includes, check whether the module file is in the Keil target and whether its include path/RTE component is configured.
- For a path mismatch, reopen the `.uvprojx` from the VS Code workspace folder and repeat the build there.
- For `Illegal start vector`, inspect the selected device/startup/vector configuration and preserve the known-good STM32F103C8 RTE startup unless there is evidence it is wrong.
- For flash-load or ST-LINK errors, check the selected device, flash algorithm, probe connection, SWD wiring, and target power before changing firmware.
- Paste the complete first error block into Codex and ask it to diagnose the source/project configuration. Do not claim success from a successful compile alone.

## Suggested prompt for Codex

> Use `$stm32f103-vscode-keil-workflow`. Inspect this workspace and `start.uvprojx`, then implement the requested firmware change in VS Code. Keep the project settings intact, include any new source files in the Keil target, and tell me exactly what to build, debug, and verify in Keil.

## Completion summary

Report changed files, Keil build result, debug/download result, and physical board observation separately. If Keil was unavailable, state that source editing was completed but build/debug/download remain for the user to perform.
