# Neptune 4 Plus MCU Recovery

![Status](https://img.shields.io/badge/status-resolved-brightgreen)
![Focus](https://img.shields.io/badge/focus-firmware%20debugging-blue)

---

## Overview

After a failed firmware update, my Elegoo Neptune 4 Plus became non-functional and booted into a persistent Klipper error:

> "Option serial in section mcu must be specified"

Although the system UI remained responsive, all hardware control was lost. This indicated a failure not at the operating system level, but in the communication between the Klipper host and the printer’s MCU.

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

The failure occurred at the communication boundary between these two systems.

---

## Initial Symptoms

- Klipper error screen on boot  
- “Restore Firmware” and “Restart Klipper” options available  
- No temperature readings (0°C across all sensors)  
- No response from motors or heaters  
- Printer detected by Windows as a USB-to-serial (CH340) device  

---

## Observed Error

On boot, the system displayed:

> "Option serial in section mcu must be specified"

The UI remained responsive, but all hardware-level functionality was unavailable.

---

## Investigation

### Device Manager Observations

Using Windows Device Manager:

- The printer appeared as a **USB-SERIAL CH340 device**  
- Multiple COM ports were present  
- The correct port was identified by disconnecting and reconnecting the printer  

The active connection was confirmed on **COM8**.

The troubleshooting process focused on isolating the failure layer before applying fixes, avoiding unnecessary system resets or hardware replacement.

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

The restore process did not reflash or repair the MCU firmware, indicating the failure existed below the level handled by the standard recovery process.

---

## Attempted Fix: Direct MCU Communication

Initiated direct MCU communication using **STM32CubeProgrammer**:

- Interface: UART  
- Port: COM8  
- Baud rate: 115200  
- Adjusted connection parameters (parity, data bits, stop bits)  

### Result

Connection failed with a boot mode error, indicating the MCU was not in a state that allowed direct flashing via UART without additional intervention.

---

## Root Cause

The failure was caused by a mismatch or corruption in the MCU firmware layer, preventing Klipper from establishing a serial connection to the mainboard.

Although the host system (Linux/Klipper) remained operational, the MCU was either:

- Running invalid or incompatible firmware  
- Not properly initialized during the update process  

This created a partial system failure where the UI remained functional, but hardware control was lost.

---

## Final Resolution

The issue was resolved by restoring a valid firmware state to the MCU and re-establishing communication with the Klipper host.

After confirming the failure was isolated to the MCU layer and not the host system, recovery efforts focused on firmware-level repair rather than full system reinstallation.

This involved:

- Verifying USB serial communication via CH340 interface  
- Identifying the correct COM port through Device Manager isolation  
- Initiating direct MCU communication using STM32CubeProgrammer  
- Adjusting connection parameters and validating communication state  
- Restoring the MCU to a working firmware state through targeted recovery steps  

Once the MCU firmware was corrected, the system immediately returned to normal operation. The Klipper error cleared, temperature readings initialized correctly, and full control of motors and heaters was restored.

This confirmed that the failure was isolated to the MCU firmware layer and that no hardware replacement or host system reinstallation was required.

---

## Outcome

The printer was fully restored to working condition without hardware replacement.

This recovery validated a structured, layered troubleshooting approach and demonstrated that targeted firmware repair can resolve failures that appear system-wide.

---

## Technical Concepts Applied

- Serial communication (UART over USB via CH340)  
- COM port identification and isolation in Windows  
- Firmware vs operating system separation  
- Layered troubleshooting (hardware → firmware → OS)  
- Device communication validation and failure analysis  

---

## Skills Demonstrated

- Hardware and firmware troubleshooting  
- Serial communication debugging (UART / COM ports)  
- Windows Device Manager diagnostics  
- Root cause analysis across system layers (OS vs MCU)  
- Use of manufacturer firmware tools (STM32CubeProgrammer)  
- Applying vendor documentation to real-world issues  

---

## Relevance to IT Roles

This project demonstrates real-world troubleshooting skills applicable to IT support and systems roles, including:

- Diagnosing hardware vs software failures  
- Working with device interfaces and drivers  
- Interpreting system-level error messages  
- Applying structured troubleshooting methodology  
- Using vendor tools to perform low-level recovery  

---

## What I Learned

- Firmware issues can exist independently from the operating system  
- Vendor recovery tools may fail in edge cases  
- Understanding system architecture is critical for effective troubleshooting  
- Direct hardware tools are sometimes required when higher-level recovery methods fail  
