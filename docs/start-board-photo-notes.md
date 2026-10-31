# STM32F103C8T6 start board: photo observations

Source: the user-provided product photo showing the front of the narrow blue STM32 development board, USB connector at the bottom. These are visual observations from a product-style image, not a schematic or inspection of the user's physical unit.

## Board identity and visible components

- The user identifies the board MCU as STM32F103C8T6. The image is consistent with an STM32F103C8-style LQFP48 board, but the exact board vendor/revision is not visible.
- A USB Type-C receptacle is visible. The photo alone does not establish whether USB data lines are routed to the MCU, whether it is used only for power, or the protection circuitry present.
- A metal crystal is marked `8.000`, consistent with an 8 MHz HSE crystal.
- A 3.3 V regulator package is visible and the header has a `3.3` power marking. The regulator's exact part number, input range, and current capability need the schematic or component marking.
- Two indicator positions are marked `PWR` and `PC13`. The latter suggests a user LED associated with PC13, but LED polarity, series resistor, and exact circuit should be verified before driving it.
- A tactile switch is marked `RESET`.
- A jumper block is marked `BOOT0`; the exact strap truth table and default jumper position are not legible enough to record confidently.
- GPIO and power labels are printed along both long headers. No display, temperature sensor, humidity sensor, or buzzer is visible in this photo.

## Readable header labels

The board uses shortened silkscreen names: `A0` corresponds to MCU `PA0`, `B0` to `PB0`, and so on. Readable labels include:

- Left header groups: `VB`, `C13`-`C15`, `A0`-`A7`, `B0`-`B1`, `B10`-`B11`, `R`, `3.3`, and `G`.
- Right header groups: `3.3`, `G`, `5V`, `B3`-`B9`, `A8`-`A12`, `A15`, `B13`-`B15`, and ground/power markings.

The image is not sharp enough to certify every hole-to-label row or every duplicated power pin. Confirm connector orientation and exact row order on the actual board before wiring. Do not treat a silkscreen label as evidence that a connector pin is electrically connected as expected without checking the board revision.

## Safe reusable conclusions

- The photo supports a board-level inventory of a reset switch, boot-selection jumper block, power indication, a PC13-marked indicator, 8 MHz crystal, regulator, USB-C connector, and GPIO/power headers.
- The board appears to expose common STM32 GPIO labels on the two side headers, useful for planning external modules.
- No external sensor, display, or buzzer is shown as a populated on-board module. These should be treated as add-on modules unless further documentation shows otherwise.
- Before creating board macros, confirm actual wiring and active levels. Record a source for each mapping and keep unverified candidates separate.

## What is still needed for a verified board map

Any one of the following would improve confidence: the board schematic, the seller's exact model/revision, a sharp close-up of both header labels and BOOT jumper, or continuity/voltage measurements on the physical board. Use those to confirm USB data routing, regulator model, LED polarity, reset/boot strap behavior, and exact header row order.
