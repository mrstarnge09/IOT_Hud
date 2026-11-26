# IOT_Hud
Spectra is a Windows 11 PyQt6 tactical HUD and universal Arduino IDE serial telemetry bridge for real-time 433.92 MHz RF analysis, signature locking, and plug-and-play IoT sensor monitoring on COM5 (115200 baud).

# Spectra — Arduino IoT & PyQt6 Tactical HUD

A lightweight Windows 11 **PyQt6** desktop tactical HUD and **Arduino IDE** serial telemetry bridge for real-time **433.92 MHz RF analysis**, signature vaulting, and plug-and-play IoT sensor monitoring.

## Hardware & Serial Setup
- **Microcontroller:** Arduino Uno (or compatible board)
- **Serial Port & Baud:** `COM5` at `115200` baud
- **433 MHz Receiver Data Pin:** `D2` (`INT0` / Interrupt 0)
- **Power & Ground:** `5V` and `GND` connected cleanly to your receiver/sensor module

## Project Layout
- `iot_core.ino` — Universal Arduino IDE serial template sending `IOT:DEV=...,VAL=...,UNIT=...,STAT=...,MSG=...` frames at `115200` baud.
- `stark_rf_scanner.ino` — 433.92 MHz RF receiver firmware capturing `250 ms` `TELEM:` windows (`EDGES`, `DUTY`, `PKT`) and raw microsecond `FRAME:` bursts on pin `D2`.
- `iot_hud_core.py` — Universal PyQt6 HUD that adapts its arc gauge, units, baseline, and threshold to any sensor sketch sending `IOT:` lines.
- `stark_system_hud.py` (offline PyQt6 HUD) — Translucent, always-on-top Windows 11 HUD with system tray controls, click-through ghost mode, local rule-based signal notes, and `stark_rf_vault.json` signature storage.

## Quick Start
1. **Install Python Dependencies:**
   ```bash
   pip install PyQt6 pyserial
