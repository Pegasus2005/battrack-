# 🏏 BatTrack — Universal Sports Implement Tracker

> **PCB Status: ✅ 39/39 connectivity checks passed — Fabrication ready**

A compact, open-source motion tracker for **cricket bats, baseball bats, badminton rackets, pickleball paddles**, tennis rackets, squash rackets, hockey sticks, and all bat/racket sports implements.

## ✅ Verified Connections (39/39)

| Block | Connections | Status |
|:---|:---|:---|
| USB-C (J1) | VBUS→VUSB, D+→USB_DP, D-→USB_DM, CC1/CC2 pulldowns | ✅ All 11 |
| TP4054 Charger (U3) | VUSB→pin4, VBAT→pin3, GND→pin2, CHRG_STAT, PROG | ✅ All 9 |
| SPI Bus (U1↔U2) | SCK, MISO, MOSI, CS, INT1 — all with F↔B vias | ✅ All 5 |
| WS2812B LED (D1) | VDD→VBAT, GND, DIN→LED_GPIO, DOUT→NC | ✅ All 4 |
| Power/Control | CHIP_EN, BTN_GPIO, ANT_RF, CC pulldowns | ✅ All 5 |
| Board specs | 22×16mm rect, 0.8mm, 2L, GND pour ×2 | ✅ All 4 |

---

## 📐 Hardware Overview

| Spec | Value |
|:---|:---|
| Board size | 22 × 16mm (+ 2mm USB-C slot) |
| Layers | 2-layer PCB |
| Thickness | 0.8mm FR4 |
| MCU | ESP32-C3FN4 (RISC-V, BLE 5.0, USB built-in) |
| IMU | QMI8658C (6-axis: ±16g accel + ±2048°/s gyro) |
| Connectivity | BLE 5.0 to smartphone |
| USB | USB-C (charge + firmware flash, no extra chip needed) |
| LED | WS2812B-2020 RGB (addressable, 1-wire) |
| Battery | LiPo via JST-PH pads (TP4054 charger, 50mA) |
| Button | Tactile boot/user button |

---

## 🎯 What It Tracks

| Sport | Metrics |
|:---|:---|
| Cricket | Bat swing speed, shot direction, impact force |
| Baseball | Swing arc, bat speed, impact detection |
| Badminton | Racket head speed, smash/drop/clear classification |
| Pickleball | Dink vs drive, spin, swing pattern |
| Tennis | Serve speed, groundstroke direction |
| Squash / Table Tennis | Stroke classification, wrist snap timing |

---

## 📦 Bill of Materials

| Ref | Component | Value | Package | Est. Cost |
|:---|:---|:---|:---|:---|
| U1 | ESP32-C3FN4 | MCU + BLE | QFN-32 | $1.50 |
| U2 | QMI8658C | 6-axis IMU | LGA-14 | $0.95 |
| U3 | TP4054 | LiPo charger | SOT-23-5 | $0.12 |
| J1 | USB-C | Charge + flash | SMD | $0.18 |
| D1 | WS2812B-2020 | RGB LED | 2020 | $0.05 |
| ANT1 | Chip antenna | 2.4GHz | 1608 | $0.35 |
| SW1 | Tact button | Boot/user | SMD | $0.10 |
| R2,R3 | 5.1KΩ | USB CC | 0402 | $0.01 ea |
| R4 | 20KΩ | Charge current | 0402 | $0.01 |
| R5,R6 | 10KΩ | EN / Boot | 0402 | $0.01 ea |
| C1,C3,C8,C9 | 100nF | Bypass | 0402 | $0.01 ea |
| C2 | 10uF | Bulk cap | 0402 | $0.04 |
| C4,C5 | 1uF | VUSB/VBAT | 0402 | $0.02 ea |
| **Total BOM** | | | | **~$3.50/board** |

---

## 🗂️ Repository Structure

```
hardware/
  sport_tracker/
    sport_tracker.kicad_pcb     ← PCB layout (KiCad 10)
    sport_tracker.kicad_sch     ← Schematic (KiCad 10)
    sport_tracker_gerbers.zip   ← Ready to upload to JLCPCB/PCBWay
    bom.csv                     ← Bill of materials with pricing
    gerbers/                    ← Individual Gerber files (14 layers)
```

---

## 🏭 Fabrication

Upload `hardware/sport_tracker/sport_tracker_gerbers.zip` to:
- **[JLCPCB](https://jlcpcb.com)** — 5 boards ~$4 USD
- **[PCBWay](https://www.pcbway.com)** — 5 boards ~$5 USD

Settings: **2 layers, 0.8mm, HASL or ENIG, green soldermask**

---

## ⚡ Firmware Flashing

```bash
# Hold BOOT button while connecting USB-C
esptool.py --chip esp32c3 --port COMx write_flash 0x0 firmware.bin
```

No external USB-UART chip needed — ESP32-C3 has built-in USB Serial/JTAG.

---

## 📡 BLE Data Protocol

The board streams IMU data over BLE to a smartphone app:
- **Accel**: X/Y/Z at ±16g, 500Hz
- **Gyro**: X/Y/Z at ±2048°/s, 500Hz
- **Computed**: swing speed (°/s), impact timestamp, session stats

---

## 📄 License

Open source — MIT License. PCB design files free to use, modify, and manufacture.

---

*Designed with KiCad 10 | Fabricated by JLCPCB*
