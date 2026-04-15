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
