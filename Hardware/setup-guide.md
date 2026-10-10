# FlatSat Setup & Recovery Guide

**Team:** Apogee Research Group — New Mexico State University
**Project:** Dual-FlatSat Cyber-Physical Space Systems
**Board:** FlatSat v1.0 (Pwnsat / Electronic Cats, RP2040)
**Last updated:** 2026-10-10
**Author:** Fernanda Orduno — Hardware Lead

---

## Purpose

This guide documents how to bring up a single FlatSat board on a local Windows machine, from plugging it in to confirming it is running and communicating. Follow this procedure for first-time setup or when recovering a board that is not responding.

If a step fails, see **Section 8: Troubleshooting**.

---

## 1. Prerequisites

Before starting, confirm you have:

- A FlatSat v1.0 board
- A USB data cable (not charge-only)
- A Windows laptop with internet access
- Git installed — verify with `git --version`
- VS Code installed, with the **PlatformIO IDE** extension
- Access to the shared repo: `https://github.com/m4l3nk4y/Apogee-NMSU`

---

## 2. Clone the Repository

Open PowerShell:

```powershell
cd $HOME
git clone https://github.com/m4l3nk4y/Apogee-NMSU.git
cd Apogee-NMSU
```

The repo is now at `C:\Users\<your-username>\Apogee-NMSU`.

> The test machine used username `m4l3n` (digit 4, not the letter l). Yours may differ — use your own path.

---

## 3. Open the Firmware Folder in VS Code

1. **File → Open Folder**
2. Navigate to: `C:\Users\<your-username>\Apogee-NMSU\Firmware`
3. Click **Select Folder**

On first open, PlatformIO automatically:

- Reads `platformio.ini`
- Downloads the RP2040 toolchain, Pico SDK, and TinyUSB submodule
- Installs libraries: RadioLib, NeoPixelBus, SparkFun LIS2DH12, Adafruit BME280

**Wait 2–5 minutes for first-time setup.** The bottom status bar shows `PlatformIO: Loading tasks...` until finished.

When complete, these icons appear in the blue status bar:

| Icon | Action |
|---|---|
| ✓ | Build |
| → | Upload |
| 🔌 | Serial Monitor |
| 🗑️ | Clean |

---

## 4. Flash the Firmware (First Time Only)

Skip to Section 5 if the FlatSat already has working firmware.

### 4.1 Enter bootloader mode

1. Hold down the **BOOT** button on the FlatSat
2. While holding, press and release **RST**
3. Release **BOOT**

The board appears as a USB mass-storage device called **`RPI-RP2`**.

### 4.2 Flash the firmware

**Option A — Drag and drop:**
1. Open File Explorer
2. In one window, navigate to: `Apogee-NMSU\Firmware\build\`
3. In another window, open the `RPI-RP2` drive
4. Drag `flatsat-firmware.uf2` into the `RPI-RP2` drive
5. Wait 2–5 seconds; the drive disappears and the board reboots

**Option B — PlatformIO Upload:**
1. In VS Code, click the **→** upload icon in the status bar
2. Wait for build and upload to finish

### 4.3 Eject before unplugging

If the board is still in bootloader mode (visible as `RPI-RP2`), right-click the drive and choose **Eject** before unplugging. This prevents a partial flash.

---

## 5. Open the Serial Monitor

The FlatSat exposes its console as a USB CDC serial device. On Windows, it appears as one of three COM ports — **COM9, COM10, or COM11** on the test machine. Which one is the working console can vary; try each.

### 5.1 Launch the monitor

**Option A — VS Code icon:**
1. With the `Firmware` folder open, look at the blue status bar
2. Click the **🔌** Serial Monitor icon

**Option B — Terminal:**
```powershell
pio device monitor
```

Or specify the port directly:
```powershell
pio device monitor -p COM11 -b 9600
```

### 5.2 Confirmed working configuration

| Setting | Value |
|---|---|
| Port | COM11 (yours may be COM9 or COM10) |
| Baud rate | 9600 |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |

### 5.3 Expected output

```
--- Terminal on COM11 | 9600 8-N-1
RX: 081FC0000014000010B3530BC2620D002FD2FF0000FB036810E3CD | RSSI: -34 | SNR: 11
RX: 0801C0000012000010B302B310000068100100000000009B70     | RSSI: -32 | SNR: 10
RX: 081FC0000014000010B9540BC2620D002FCEFF0000FB036810E6B5 | RSSI: -28 | SNR: 11
```

Each `RX:` line is a CCSDS/SPP packet received over the LoRa uplink from another FlatSat. RSSI is signal strength in dBm; SNR is signal-to-noise ratio in dB.

### 5.4 Stop the monitor

Press **Ctrl+C** inside the terminal. The prompt returns to PowerShell.

If Ctrl+C does not work, right-click the **Monitor - Task** terminal tab and choose **Kill Terminal**.

---

## 6. Confirming a Working Board

A FlatSat is confirmed working when all of the following are true:

- [ ] The board mounts as a COM port
- [ ] PlatformIO opens the serial monitor successfully
- [ ] Live `RX:` lines appear with non-zero RSSI values
- [ ] Packets arrive at a steady rate (about 2 Hz)
- [ ] Signal quality is healthy (RSSI between -20 and -50 dBm)

If all boxes are checked, the board is running and communicating.

---

## 7. Recovery Procedure

If the board stops responding:

1. Unplug the USB cable
2. Hold **BOOT**
3. Plug the USB cable back in (still holding BOOT)
4. Press and release **RST**
5. Release **BOOT**
6. The `RPI-RP2` drive reappears
7. Re-flash using Section 4.2

---

## 8. Troubleshooting

**No `RPI-RP2` drive after holding BOOT + RST**
- Try a different USB cable (some are charge-only)
- Try a different USB port
- Confirm you are holding BOOT before pressing RST

**`pio: command not found` in PowerShell**
- PlatformIO's CLI is not on your PATH. This is normal.
- Use the VS Code status-bar icons instead of the `pio` command.

**No 🔌 icon in the VS Code status bar**
- PlatformIO is still installing. Wait 2–5 more minutes.
- If it does not appear after 10 minutes, restart VS Code.

**Serial monitor is open but shows no `RX:` lines**
- You are on the wrong COM port. Try COM9, then COM10, then COM11.
- Confirm the baud rate is **9600**.
- Confirm the FlatSat is powered (blue LED visible).
- Confirm the peer FlatSat is powered and transmitting.

**`git: command not found`**
- Git is not installed. Download from `https://git-scm.com/download/win`
- After installing, fully close and reopen VS Code so the new PATH is loaded.

**`Permission denied` on `git push`**
- You are pushing to a repo you do not have write access to.
- Verify with `git remote -v`. `origin` should be:
  `https://github.com/m4l3nk4y/Apogee-NMSU.git`
- If not, run:
  ```powershell
  git remote set-url origin https://github.com/m4l3nk4y/Apogee-NMSU.git
  ```

**GitHub asks for authorization**
- A browser window opens: **"Authorize Git Credential Manager"**
- Click the green **Authorize git-ecosystem** button
- Return to PowerShell; the push resumes automatically

**File exists but commits as 0 bytes**
- You opened Notepad but did not paste or save. Reopen, paste content, press **Ctrl+S**.

---

## 9. Quick Reference

```powershell
# Clone (first time only)
cd $HOME
git clone https://github.com/m4l3nk4y/Apogee-NMSU.git

# Open firmware folder in VS Code
code Apogee-NMSU\Firmware

# Plug in the FlatSat, then open the serial monitor
cd Apogee-NMSU\Firmware
pio device monitor -p COM11 -b 9600

# Ctrl+C to stop

# Commit and push your log
cd ..
git add Hardware\logs\<your-log>.txt
git commit -m "Add <description>"
git push
```

---

## 10. References

- FlatSat repo: `https://github.com/Pwnsat/FlatSat`
- FlatSat wiki: `https://github.com/Pwnsat/FlatSat/wiki`
- PlatformIO: `https://platformio.org`
- Space Odyssey paper: Willbold et al., "Space Odyssey: An Experimental Software Security Analysis of Satellites," IEEE S&P 2023

---

*Maintained by the Apogee Research Group — NMSU Dual-FlatSat Cyber-Physical Space Systems project.*