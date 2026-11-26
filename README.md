# IOT_Hud
https://drive.google.com/file/d/1MA0CKXuCIhV0-Ev5V2gaeM-O7VS8VhCa/view?usp=sharing
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

   ## Reverse-Engineering & Customizing for Your Setup
Spectra is built to be reverse-engineered and adapted when a sensor or remote does not fit default decoders or thresholds:

- **Inspect Raw Pulse Timings (`FRAME:`):** When a 433.92 MHz remote or custom transmitter returns `PKT=0`, inspect the microsecond pulse lengths (`short_us`, `long_us`, `ratio`, and `bits`) in `decode_frame()` inside `stark_system_hud.py` to separate short/long pulses, spot repeating bit patterns, and tell fixed codes apart from rolling codes.
- **Tune Thresholds & Cooldowns:** Adjust `ALERT_COOLDOWN_SEC`, the rolling baseline window (`maxlen=40`), or the `25.0`-edge floor in `SerialScannerWorker` (`stark_system_hud.py`) or `UniversalIotWorker` (`iot_hud_core.py`) to match noisy rooms, fast sensors, or slow actuators.
- **Extend the Serial Protocol:** Add custom key-value tags or new `IOT:` / `TELEM:` fields in your `.ino` sketch (`115200` baud) and parse them in `parse_iot_line()` or `SerialScannerWorker` to drive custom HUD labels, colors, or local actions.
- **Customize the HUD Layout & Tray Behavior:** Edit `IoTArcGauge` / `ArcGaugeWidget`, window size, default screen position, or click-through ghost mode in the PyQt6 HUD class to fit your workflow.
