# MCU-card interface — v1 (draft)

The contract between the Fieldbus base card and every MCU card (PRD BASE-11). It is MCU-neutral: every signal is a function that any candidate MCU provides (STM32C5A3VGT6, STM32H573VIT6, later others), so a new MCU only ever needs a new MCU card. v1 freezes with the first base-card fab; a breaking change becomes v2 with a new base-card revision.

Status: draft, created 2026-11-24. The pin table is filled in the design-spec step (≈ 90 signals before savings).

## Electrical

- Connector: board-to-board, no cable. Either a 2.54 mm gold pin-header pair or Hirose FX18, chosen with the housing's card spacing (enclosure fit study).
- Power from the base card's DC-DC: 5 V and 3.3 V, with ground pins between the analog lines. No converter on the MCU card (a future MCU that needs another rail gets a small regulator on its own card).
- Logic level 3.3 V.
- Spare pins are reserved for future needs.
- One pin table, `card-interface.csv`, one row per connector pin:

| Column | Meaning |
|---|---|
| `pin` | Connector pin number |
| `signal` | Net name used on every card, prefix `IF_` |
| `function` | UART_TX, UART_RX, UART_DE, SPI_SCK, SPI_MOSI, SPI_MISO, SPI_CS, I2C_SCL, I2C_SDA, CAN_TX, CAN_RX, ADC, PWM, GPIO_IN, GPIO_OUT, OD, PWR, GND, SPARE |
| `direction` | Seen from the MCU: in, out, bidir, power |
| `capability` | What the MCU pin must support, e.g. `ADC`, `timer PWM >= 10 kHz`, `FDCAN TX` |
| `notes` | Anything else |

## Mechanical

Card outline, connector position, standoff holes, height limits over the base card, and the RJ45 / USB-C / LED positions at the housing cut-outs come from the enclosure fit study and live in `mcu-card-template/`.

## Pin maps

| Signal | STM32C5A3VGT6 pin | STM32H573VIT6 pin |
|---|---|---|
| (filled in the design-spec step) | | |

## Card ID

Each MCU card carries an ID strap (resistors read by its MCU) giving card type and revision, so firmware and the test jig know which card is fitted.

## Version history

| Version | Date | Change |
|---|---|---|
| v1 draft | 2026-11-24 | Created |
