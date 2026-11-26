# Fieldbus controller hardware — working rules

Read first, from the umbrella repo (this repo sits at `modules/fieldbus-controller/` inside it):
- Requirements: `../../docs/arch/docs/01_Requirements_PRD.md` — BASE-*, IO-*, FB-*, PWR-* rows. The PRD wins over this file.
- Part selection, cost and owner-facing rules: `../../docs/arch/CLAUDE.md`.

## Boards and the MCU-card interface
- Two standard PCBs: base card + raised plug-in MCU card. The battery + charger board is its own repo.
- The MCU-card interface (`docs/mcu-card-interface.md` + `docs/card-interface.csv`) is the contract between them. It is MCU-neutral: every signal is a function any candidate MCU provides. Change it only by bumping its version; v1 freezes with the first base-card fab.
- Every MCU card starts from `mcu-card-template/`; only the MCU sheet is new. The sheets around the MCU (NOR, RTC, Ethernet, USB-C, LED, card connector) are shared circuit blocks.
- Card-connector nets use the signal names from `card-interface.csv` (prefix `IF_`). Run the interface check (`lib/tools/`) on the base card and every MCU card after any change.
- MCU cards: the STM32H5 card (H563 / H573, same pinout) for the model with Ethernet (≥ 2 MB flash / ≥ 640 KB RAM, PRD BASE-10 update 10) and the 5 prototypes (H563); the STM32C5A3 card (or the H5 card with an H523) for the model without Ethernet — the standard pick is still open. Motor control is a separate motor-control card (IO-10 update 5); the DRV8873S provision on the base card is gone.

## KiCad
- KiCad 10; projects sit one level below the repo root, so library paths start with `${KIPRJMOD}/../lib/`.
- Hierarchical sheets; repeated channels (AI, DI, relays, outputs) as repeated sheets, so their layout can be reused with the multi-channel tool.
- Symbol fields: MPN, Manufacturer, MPN_Alt, LCSC, Notes. Not-fitted parts use KiCad's DNP flag.
- Prices only in `bom/prices.csv` (INR at 100 units, source, date). Relays and fuses: the owner's quotes only.
- ERC and DRC clean before any release; fab outputs go through the jobset into `fab/` (not committed).

## Mechanical
- build123d (Python) in `mech/`; the venv lives outside the repo. Export STEP / DXF / PDF / PNG. Housing models come from `lib/mech/`.

## Never add parts silently
A change that adds parts or cost is stated first, with its INR delta and alternatives, before it goes into the design.
