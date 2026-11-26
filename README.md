# UC-HW-Fieldbus-Controller — Fieldbus controller hardware

The Fieldbus controller of the Universal Controller family: base card (DC-DC and power stage; model with Ethernet: 4 AI, 6 DI; model without Ethernet: 6 AI, 10 DI, 2 extra outputs; both: 6 relays, 4 high-side outputs, isolated RS-485 and CAN-FD, stack bus, terminals) plus a raised plug-in MCU card (MCU, Ethernet on the model with Ethernet, USB-C, NOR, RTC, LED). Motor control is a separate motor-control card (PRD IO-10 update 5).

This repo holds its schematics, PCBs, bill of materials, mechanical CAD and production guidelines. Requirements live in the umbrella repo `Universal-Controller_Hardware`, in `docs/arch/docs/01_Requirements_PRD.md` (BASE-1 to BASE-11, IO-1 to IO-10, FB-1 to FB-5, PWR-10); the shared interfaces (stack bus, 4G UART link, battery socket, power stage, enclosure family) in `docs/arch/docs/02_Specifications.md`.

## Boards

| Folder | Board | Status |
|---|---|---|
| `base-card/` | Base card | not started |
| `mcu-card-template/` | Template every MCU card starts from | not started |
| `mcu-card-stm32c5/` | MCU card, STM32C5A3VGT6 — model without Ethernet; too small for the model with Ethernet (PRD BASE-10 update 10) | not started |
| `mcu-card-stm32h5/` | MCU card, STM32H563VIT6 / H573VIT6 (same LQFP100 pinout; also takes H523 / H533 / H562) — model with Ethernet and the 5 prototypes (H563); H573 for PSA L3 | not started |

The optional battery + charger board has its own repo (`UC-HW-Battery-Board`).

### Compatibility

| MCU card | Base card | Card interface | Minimum firmware |
|---|---|---|---|
| — | — | v1 (draft) | — |

## Folders

| Folder | Contents |
|---|---|
| `lib/` | Shared library `UC-HW-Lib` (git submodule, pinned) |
| `docs/` | Design spec: block diagram, pin maps, calculations, power budget, design reviews |
| `bom/` | BOM per board (generated from KiCad), module BOM (boards + housing + fixings + packaging), `prices.csv` |
| `mech/` | build123d (Python) sources for cut-outs, front label and fit check; exports STEP / DXF / PDF / PNG |
| `production/` | Production guidelines: fab notes, assembly, programming, test, QC, labelling, packaging |
| `fab/` | Generated fab outputs — not in git; zipped into each release |

## Getting it

    git clone --recurse-submodules <repo URL>

Inside the umbrella repo it is checked out at `modules/fieldbus-controller/`.

## Releases

Each board has its own revision letter (A, B, …) in its title block and on its silkscreen, and its own git tag per fab release (`<board>-rev-<letter>`), with the fab zip attached. Release notes state the minimum firmware version.
