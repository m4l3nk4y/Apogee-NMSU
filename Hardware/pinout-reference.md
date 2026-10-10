# FlatSat v1.0 Pinout Reference

**Source:** `Firmware/pins.h`
**Board:** FlatSat v1.0 (RP2040)
**Last updated:** 2026-10-10
**Author:** Fernanda Orduno — Hardware Lead, Apogee Research Group
**Repo:** https://github.com/m4l3nk4y/Apogee-NMSU

---

## Overview

The FlatSat v1.0 exposes two independent LoRa radios, an I2C sensor bus, a GPS UART, two buttons, and an RGB status LED. This document maps every firmware-defined GPIO to its physical role. Use it as the authoritative reference when wiring, debugging, or extending the board.

---

## Subsystems at a Glance

| Subsystem | Interface | GPIO Count | Purpose |
|---|---|---|---|
| Uplink / Downlink LoRa | 2× SPI | 20 | Two SX1262 radios for bidirectional satellite communication |
| I2C sensor bus | I2C | 2 | Shared bus for BME280 (environmental) and LIS2DH12 (accelerometer) |
| GPS | UART | 2 | NMEA position and time source |
| Buttons | GPIO | 2 | User input (BOOT and RESET functions) |
| Status LED | PWM / NeoPixel | 1 | RGB indication of firmware state |

**Total: 27 GPIO pins in use.**

---

## 1. Status LED and Buttons

| Signal | GPIO | Function | Notes |
|---|---|---|---|
| `PIN_RGB_LED` | 15 | NeoPixel RGB status LED | Controlled via NeoPixelBus library |
| `PIN_BUTTON1` | 12 | Button 1 | Referenced as BOOT in setup guide |
| `PIN_BUTTON2` | 13 | Button 2 | Referenced as RST in setup guide |

**Firmware observation:** The LED is a NeoPixel-compatible RGB device, not a simple on/off pin. LED color indicates firmware state during operation.

---

## 2. Radio 0 — Uplink LoRa (SX1262)

| Signal | GPIO | Function |
|---|---|---|
| `PIN_SPI_RADIO0_CIPO` | 16 | SPI MISO — data from radio to MCU |
| `PIN_SPI_RADIO0_SCK` | 18 | SPI clock |
| `PIN_SPI_RADIO0_COPI` | 19 | SPI MOSI — data from MCU to radio |
| `PIN_RADIO0_NSS` | 17 | SPI chip select (NSS) |
| `PIN_RADIO0_RST` | 27 | Radio hardware reset |
| `PIN_RADIO0_BSY` | 26 | Busy signal from radio |
| `PIN_RADIO0_DIO1` | 3 | Digital IO 1 — used for TX/RX done interrupts |
| `PIN_RADIO0_DIO2` | 24 | Digital IO 2 — RF switch control |
| `PIN_RADIO0_DIO3` | 25 | Digital IO 3 — TCXO control (forced to 0.0 V in v1.0.3) |
| `PIN_RADIO0_ANT_SW` | 28 | Antenna switch — selects TX or RX antenna path |

**Purpose in firmware:** Handles the **uplink** — receives telecommands from the ground station or another FlatSat. During tests, live `RX:` lines were visible on the serial monitor as this radio received packets.

**Note on DIO3 / TCXO:** The firmware README notes that LoRa radios on v1.0.0 failed to initialize because `txcVoltage` was left at the RadioLib default (1.6 V). The fix in v1.0.3 sets it to **0.0 V**, because this module uses a main XTAL oscillator, not a TCXO.

---

## 3. Radio 1 — Downlink LoRa (SX1262)

| Signal | GPIO | Function |
|---|---|---|
| `PIN_SPI_RADIO1_CIPO` | 4 | SPI MISO |
| `PIN_SPI_RADIO1_SCK` | 6 | SPI clock |
| `PIN_SPI_RADIO1_COPI` | 7 | SPI MOSI |
| `PIN_RADIO1_NSS` | 5 | SPI chip select |
| `PIN_RADIO1_RST` | 11 | Radio hardware reset |
| `PIN_RADIO1_BSY` | 14 | Busy signal |
| `PIN_RADIO1_DIO1` | 8 | Digital IO 1 |
| `PIN_RADIO1_DIO2` | 9 | Digital IO 2 |
| `PIN_RADIO1_DIO3` | 10 | Digital IO 3 — also subject to the txcVoltage=0.0 fix |
| `PIN_RADIO1_ANT_SW` | 29 | Antenna switch |

**Purpose in firmware:** Handles the **downlink** — transmits telemetry to the ground station or another FlatSat. Also used for transmissions during attack experiments.

**Important:** Radios 0 and 1 use **separate SPI buses** — Radio 0 on GPIO 16/18/19, Radio 1 on GPIO 4/6/7. This allows simultaneous independent operation, essential for dual-FlatSat experiments. Do not cross the wires.

---

## 4. I2C Sensor Bus

| Signal | GPIO | Function |
|---|---|---|
| `PIN_I2C_SDA` | 20 | I2C data line |
| `PIN_I2C_SCL` | 21 | I2C clock line |

**Devices on this bus:**
- **BME280** — temperature, humidity, pressure
- **LIS2DH12** — 3-axis accelerometer

Both sensors share the same physical I2C bus, distinguished by their 7-bit I2C addresses.

**Security implication:** A compromised firmware image or a runaway process on the bus could spoof sensor readings to the CDHS — directly relevant to the "Trusted External Input" and "Trusted Size Field" vulnerability classes from the *Space Odyssey* paper.

---

## 5. GPS UART

| Signal | GPIO | Function |
|---|---|---|
| `PIN_GPS_TX` | 0 | UART TX — MCU transmits to GPS module |
| `PIN_GPS_RX` | 1 | UART RX — MCU receives from GPS module |

**Purpose in firmware:** Feeds the ground-station authentication gate. Per the firmware README, the access challenge/response requires the GPS to report a position within a **35 km radius** of a fixed ground station coordinate. This is the **`gs_auth_spoofing` vulnerability** exploited by one of the attack scripts.

**Security note:** If the GPS position can be spoofed or the GPS data can be substituted, ground-station authentication can be bypassed. Flag this for Student 4's attack-surface analysis.

---

## 6. USB CDC Serial Console

The firmware exposes a **dual-core USB serial console** via TinyUSB. Not a GPIO pin, but the primary interface for debugging and monitoring.

| Feature | Detail |
|---|---|
| Library | Adafruit_TinyUSB (bundled with earliphilower's RP2040 core) |
| Enumeration | Three USB CDC endpoints appear as separate COM ports |
| Observed ports | COM9, COM10, COM11 on the test machine |
| Working console port | COM11 in our tests |
| Baud rate | 9600, 8-N-1 |
| Output format | `RX: <hex bytes> \| RSSI: -XX \| SNR: XX` |

See `Hardware/setup-guide.md` for the full procedure to open this console.

---

## 7. Not Used / Reserved

The firmware does not currently define pins for:

- Additional SPI devices
- Additional UART channels
- External interrupts beyond LoRa DIO1

If the team later wants to add hardware, these unused RP2040 GPIOs are available.

---

## 8. Physical Board Layout Mapping

The RP2040 pins map to physical breakout headers on the FlatSat PCB. The KiCad files under `Hardware/flat-sat/` contain the exact silkscreen and pin-header layout. A physical board photograph with annotated labels will be added later as `Hardware/architecture-diagram.png`.

---

## 9. How to Update This Document

If you discover new pin usage (e.g., during integration or attack testing):

1. Read the relevant source file (`Firmware/pins.h` or `Firmware/*.cpp`)
2. Add a new section or row
3. Note the date and reason for the change at the bottom
4. Commit with a descriptive message:
   ```powershell
   git add Hardware\pinout-reference.md
   git commit -m "Add pin 22 usage discovered during attack 05 testing"
   git push
   ```

---

## Change Log

| Date | Author | Change |
|---|---|---|
| 2026-10-10 | Fernanda Orduno | Initial pinout reference created from `Firmware/pins.h` |

---

*See also: `Hardware/setup-guide.md`, `Hardware/logs/README.md`*