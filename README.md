# Neptune 4 Plus MCU Recovery

![Status](https://img.shields.io/badge/status-in--progress-yellow)
![Focus](https://img.shields.io/badge/focus-firmware%20debugging-blue)

## Overview

After attempting a firmware update on my Elegoo Neptune 4 Plus, the printer became non-functional and booted into a persistent Klipper error:

> "Option serial in section mcu must be specified"

The UI still loaded, but the printer reported 0°C temperatures and would not respond to commands.

---

## System Architecture

The printer consists of two main systems:

- **Host System (Linux / Klipper)**  
  Handles UI and high-level control

- **MCU (Mainboard Firmware)**  
  Controls motors, heaters, and sensors

The issue was a failure in communication between these two layers.

---

## Initial Symptoms

- Klipper error screen on boot
- Restore Firmware option present
- No temperature readings (0°C)
- Printer detected via USB as CH340 serial device

### Error Display

![Klipper Error](images/klipper-error.jpg)

---

## Investigation

### USB / Serial Check

Using Windows Device Manager:

- Identified multiple CH340 devices
- Isolated correct port via unplug/replug method
- Confirmed printer on COM8

### Device Manager Output

![Device Manager](images/device-manager.png)

---

## Attempted Fix: SD Firmware Restore

- Downloaded correct firmware (Type-C version)
- Copied full firmware folder to FAT32 microSD
- Used Restore Firmware option

### Result

- Restore completed
- Issue persisted

---

## Attempted Fix: Direct MCU Access

Used STM32CubeProgrammer:

- UART connection on COM8
- Baud rate 115200

### Result

Connection failed with boot mode error.

### STM32 Error

![STM32 Error](images/stm32-error.png)

---

## Root Cause

- Host system operational
- MCU firmware missing or corrupted
- Klipper unable to communicate over serial

---

## Skills Demonstrated

- Hardware and firmware troubleshooting
- Serial communication debugging (UART / COM ports)
- Windows Device Manager diagnostics
- Root cause analysis across system layers
- Use of manufacturer firmware tools

---

## What I Learned

- Firmware issues can exist independently from the OS
- Vendor tools may fail in edge cases
- Understanding system architecture is critical
