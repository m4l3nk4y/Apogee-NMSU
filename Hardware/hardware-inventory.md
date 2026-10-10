# FlatSat v1.0 Hardware Inventory

**Board:** FlatSat v1.0 (Pwnsat / Electronic Cats)
**Processor:** Raspberry Pi RP2040 (dual-core ARM Cortex-M0+, 264 KB SRAM)
**Source:** `Hardware/flat-sat/FlatSat.csv` (KiCad bill of materials)
**Last updated:** 2026-10-10
**Author:** Fernanda Orduno — Hardware Lead, Apogee Research Group

---

## Overview

The FlatSat v1.0 is a two-radio satellite security research platform. It contains an RP2040 microcontroller, two SX1262 LoRa transceivers with independent SPI buses, two environmental sensors on a shared I2C bus, an external flash chip, a NeoPixel status LED, two push buttons, a USB-C connector, an SWD debug header, and SMA antenna connectors.

The board carries **63 populated components** across 30 unique part numbers. Four additional components are **DNP (Do Not Populate)** — present in the schematic for future use, but not soldered onto the board.

---

## Major Functional Components

These are the parts that determine what the board can do. All are populated.

| Ref | Component | Function | Key Spec |
|---|---|---|---|
| U9 | **RP2040** | Main OBC — dual-core ARM Cortex-M0+ | 133 MHz, 264 KB SRAM |
| U4, U8 | **SX1262IMLTRT** | LoRa transceivers (Uplink + Downlink) | Sub-GHz, SPI interface |
| U5 | **IS25LP032D** | External flash memory | 32 Mbit (4 MB) |
| U10 | **LIS2DH** | 3-axis accelerometer | I2C / SPI, low-power |
| U11 | **BME280 / BMP280** | Environmental sensor | Temperature, humidity, pressure |
| U3, U7 | **PE42421** | RF antenna switches | Controls Uplink/Downlink routing |
| U2, U6 | **0900FM15D0039** | RF filters | Band-pass filtering for LoRa |
| U1 | **TLV75733PDBVR** | Voltage regulator | 3.3 V LDO, SOT-23-5 |
| D2 | **SK6812MINI** | Addressable RGB status LED (NeoPixel-compatible) | Single-wire control |
| P1 | **USB_C_Plug_USB2.0** | USB-C connector | Power + serial console |
| Y1, Y2 | 32 MHz crystal | LoRa radio reference clock | SMD 2520, 4-pin |
| Y3 | 12 MHz crystal | RP2040 system clock | SMD 3225, 4-pin |

---

## Full Bill of Materials

Components are grouped as they appear in the source CSV.

| Reference | Value | Footprint | Qty | LCSC# | Notes |
|---|---|---|---|---|---|
| BOOT1, Button1, Button2, RESET1 | SW | TS1088R02026 | 4 | C455281 | Push buttons (BOOT, RESET, and two user buttons) |
| C1, C5, C7, C8, C13, C14, C18, C19, C24, C25, C27, C28, C32, C33, C34, C35, C36, C37, C38 | 0.1 µF | C_0402 | 19 | C56392 | Decoupling capacitors |
| C2, C15 | 100 pF | C_0402 | 2 | C541374 | RF coupling |
| C3, C4, C16, C17 | 3.3 pF | C_0402 | 4 | C1565 | RF tuning |
| C6, C26, C31 | 1 µF | C_0402 | 3 | C52923 | Bulk decoupling |
| C9, C20 | 47 pF | C_0402 | 2 | C437519 | RF filter |
| C10, C12, C21, C23 | 1 nF | C_0402 | 4 | C335530 | Bypass |
| C11, C22 | 470 nF | C_0402 | 2 | C47339 | Bypass |
| C29, C30 | 27 pF | C_0402 | 2 | C424826 | Crystal load capacitors |
| D2 | SK6812MINI | SK6812MINI-HS | 1 | C2922787 | RGB status LED |
| F1 | 0.75 A | Fuse_0603 | 1 | C182438 | Input protection fuse |
| J1, J2 | Antenna | SMA_EDGE | 2 | C530661 | Uplink / Downlink SMA connectors |
| J3 | periphs | PinHeader 1×06 2.54 mm | 1 | — | Peripherals breakout |
| J4 | SWD | PinHeader 1×04 2.54 mm | 1 | — | **DNP** — SWD debug header |
| JP1, JP2 | Jumper | SolderJumper-3 | 2 | — | **DNP** — solder jumpers |
| L1, L4 | 9.1 nH | L_0603 | 2 | C395094 | RF matching |
| L2, L5 | 47 nH | L_0603 | 2 | C578001 | RF matching |
| L3, L6 | 15 µH | L_0603 | 2 | C57838 | Power supply inductor |
| P1 | USB_C_Plug_USB2.0 | C393939 | 1 | C709357 | USB-C connector |
| R1, R2 | 5.1 kΩ | R_0402 | 2 | C25905 | USB-C CC resistors |
| R3, R4, R6, R7 | 100 Ω | R_0402 | 4 | C25076 | Series termination |
| R5 | 10 kΩ | R_0402 | 1 | C25531 | Pull-up |
| R8, R10 | 27 Ω | R_0402 | 2 | C25100 | RF series |
| R9, R15 | 4.7 kΩ | R_0402 | 2 | C25900 | I2C pull-ups |
| R13 | 1 kΩ | R_0402 | 1 | C11702 | LED current limit |
| R14 | 470 Ω | R_0402 | 1 | C25117 | LED current limit |
| U1 | TLV75733PDBVR | SOT-23-5 | 1 | C485517 | 3.3 V LDO regulator |
| U2, U6 | 0900FM15D0039 | 0900FM15D0039E | 2 | — | RF filters |
| U3, U7 | PE42421 | SOT-363 | 2 | C470893 | RF antenna switches |
| U4, U8 | SX1262IMLTRT | QFN-24 | 2 | C191341 | LoRa transceivers |
| U5 | IS25LP032D | USON-8 | 1 | C5119926 | 32 Mbit flash |
| U9 | RP2040 | QFN-56 | 1 | C2040 | Microcontroller |
| U10 | LIS2DH | LGA-14 | 1 | C155670 | 3-axis accelerometer |
| U11 | BME280 / BMP280 | LGA-8 | 1 | C92489 | Environmental sensor |
| Y1, Y2 | 32 MHz | Crystal_SMD_2520 | 2 | C255969 | LoRa reference crystals |
| Y3 | 12 MHz | Crystal_SMD_3225 | 1 | C2848217 | RP2040 system crystal |

---

## DNP Components (Not Populated)

These parts exist in the schematic but are **not soldered onto the board**. They are reserved for future hardware extensions. Do not expect them to be usable.

| Reference | Value | Purpose |
|---|---|---|
| J4 | SWD header | Debug port — intended for SWD flashing but not populated |
| JP1, JP2 | Solder jumpers | Configuration jumpers — not populated |

**Hardware note:** Since the SWD header is DNP, you cannot use SWD for debugging or firmware extraction without physically soldering a pin header onto the board. This matters for Student 4's attack-surface work — it means physical-access attacks require desoldering or attaching to test pads.

---

## Interfaces Summary

| Interface | Connected To | Purpose |
|---|---|---|
| USB-C (P1) | RP2040 | Power + serial console + firmware flashing |
| SMA J1 | Uplink LoRa (U4) | Receive telecommands from ground station |
| SMA J2 | Downlink LoRa (U8) | Transmit telemetry to ground station |
| J3 (1×06 header) | RP2040 GPIO | Peripherals breakout |
| J4 (SWD, DNP) | RP2040 debug port | Not populated |

---

## How This Inventory Was Built

1. **Source:** The `FlatSat.csv` file in `Hardware/flat-sat/` is the KiCad bill of materials, exported directly from the schematic.
2. **Grouping:** Components were grouped by the reference designators listed in the CSV (e.g., `C1,C5,C7,...` is one row with Qty=19).
3. **Cross-check:** The pinout reference (`Hardware/pinout-reference.md`) was used to identify which components connect to which GPIOs.
4. **Physical verification:** A physical inspection of the board (visual only) confirmed that the four DNP components are indeed absent.

---

## Change Log

| Date | Author | Change |
|---|---|---|
| 2026-10-10 | Fernanda Orduno | Initial hardware inventory created from `Hardware/flat-sat/FlatSat.csv` |

---

*See also: `Hardware/pinout-reference.md`, `Hardware/setup-guide.md`, `Hardware/architecture-diagram.png` (to be added)*