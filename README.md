# Neptune 4 Plus MCU Recovery

![Status](https://img.shields.io/badge/status-resolved-brightgreen)
![Focus](https://img.shields.io/badge/focus-firmware%20debugging-blue)

## Overview

After attempting a firmware update on my Elegoo Neptune 4 Plus, the printer became non-functional and booted into a persistent Klipper error:

> "Option serial in section mcu must be specified"

The UI still loaded, but the printer reported 0°C temperatures and would not respond to commands.

---

## Note

This project documents a real-world troubleshooting process. Screenshots were not captured during the process, but all observations and steps are accurately recorded.

---

## System Architecture

The printer consists of two primary systems:

- **Host System (Linux / Klipper Host)**  
  Handles the operating system, UI, and high-level control

- **MCU (Mainboard Firmware)**  
  Controls motors, heaters, and sensors, and communicates with the host over a serial interface

The issue involved a failure in communication between these two layers.

---

## Initial Symptoms

- Klipper error screen on boot
- “Restore Firmware” and “Restart Klipper” options available
- No temperature readings (0°C across all sensors)
- Printer detected by Windows as a USB-to-serial (CH340) device
- No response from motors or heaters

---

## Observed Error

On boot, the system displayed the following Klipper error:

> "Option serial in section mcu must be specified"

The UI remained responsive, but all hardware-level functionality was unavailable.

---

## Investigation

### Device Manager Observations

Using Windows Device Manager:

- The printer appeared as a **USB-SERIAL CH340 device**
- Multiple COM ports were present
- The correct port was identified by unplugging and reconnecting the printer

The active connection was confirmed on **COM8**.

---

## Attempted Fix: SD-Based Firmware Restore

- Downloaded the correct firmware package (Type-C version)
- Copied the firmware folder to a FAT32-formatted microSD card
- Used the **Restore Firmware** option from the printer UI

### Result

- Restore process completed without visible errors
- Printer rebooted successfully
- **Issue persisted with the same MCU communication error**

### Conclusion

The restore process did not reflash or repair the MCU firmware.

---

## Attempted Fix: Direct MCU Communication

Used **STM32CubeProgrammer** to attempt direct communication with the MCU:

- Interface: UART
- Port: COM8
- Baud rate: 115200

### Result

Connection failed with a boot mode error, indicating the MCU was not in a state that allowed direct flashing via UART.

---

## Root Cause

The failure was caused by a mismatch or corruption in the MCU firmware layer, preventing Klipper from establishing a serial connection to the mainboard.

Although the host system (Linux/Klipper) remained operational, the MCU was either:

- Running invalid or incompatible firmware
- Not properly initialized during the update process

This created a partial system failure where the UI remained functional, but hardware control was lost.

---

## Final Resolution

The issue was ultimately resolved by restoring proper communication between the Klipper host system and the MCU.

After isolating the failure to the MCU firmware layer and confirming USB-level connectivity via CH340 serial, the recovery process focused on re-establishing a valid firmware state for the mainboard.

Once the MCU firmware was correctly restored, the system behavior returned to normal:

- Klipper error screen cleared
- Temperature readings returned to expected values
- Printer regained full control of motors and heaters
- Normal operation resumed

This confirmed that the failure was isolated to the MCU firmware layer and not due to hardware damage or host system corruption.

---

## Outcome

The printer was fully restored to working condition without hardware replacement.

This recovery demonstrated the ability to diagnose failures across multiple system layers and apply targeted solutions rather than relying on full system resets.

---

## Skills Demonstrated

- Hardware and firmware troubleshooting
- Serial communication debugging (UART / COM ports)
- Windows Device Manager diagnostics
- Root cause analysis across system layers (OS vs MCU)
- Use of manufacturer firmware tools (STM32CubeProgrammer)
- Applying vendor documentation to real-world issues

---

## What I Learned

- Firmware issues can exist independently from the operating system
- Vendor recovery tools may fail in edge cases
- Understanding system architecture is critical for effective troubleshooting
- Direct hardware tools are sometimes required when higher-level recovery methods fail
