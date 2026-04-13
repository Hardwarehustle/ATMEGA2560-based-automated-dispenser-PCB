# 💊 Automated Dispenser — ATMEGA2560 Based Smart Dispensing Controller PCB

> **Designed by:** Janardhan BV  
> **Tool:** EasyEDA  
> **MCU:** ATMEGA2560-16AU  
> **Date:** November 2024 | **Rev:** 1.0  
> **Type:** Personal Project

---

## 📌 Project Overview

This project is a **full-featured automated dispenser controller PCB** built around the **ATMEGA2560-16AU** (Arduino Mega 2560 compatible) microcontroller. The board is designed to drive **3 stepper motors** and **1 servo motor** for precise dispensing operations, with intelligent scheduling via a **real-time clock (RTC)**, remote alerting via a **SIM800L GSM module**, presence detection via an **IR sensor**, a **buzzer** for audio feedback, and an **I2C LCD display** for real-time status.

The system is ideal for **medicine dispensers, food dispensers, automated vending mechanisms**, or any time-triggered multi-compartment dispensing application.

The design covers:
- Full **schematic capture** in EasyEDA (ATMEGA2560-16AU + CH340G + all peripherals)
- **Multi-layer PCB layout** with 4x stepper/servo motor drives
- **GSM-based remote notification/control** via SIM800L
- **RTC-based scheduling** for timed dispensing events
- **Comprehensive GPIO breakout headers** for expansion
- **USB programming** via CH340G (Micro-USB)

---

## 🖼️ Project Visuals

### 3D Board View
![3D Top View](images/TopView.jpg)

### Schematic
![Schematic](images/Schematic_Dispenser.jpg)

---

## 🏗️ System Architecture

```
                         ┌────────────────────────┐
                         │    ATMEGA2560-16AU      │
    DC Power In ─────►  │  (Arduino Mega compat.) │◄──── USB (CH340G)
    (Barrel Jack)        │                         │
                         │  D2–D53 / A0–A15        │
                         └──────────┬──────────────┘
              ┌───────────┬─────────┼──────────────────────┐
              ▼           ▼         ▼                      ▼
     3x ULN2003AN      Servo    CH340G               SIM800L
     Stepper Drivers   Motor    (USB-UART)           (GSM Module)
     (ST_MOTR_1/2/3)  (D44)
              │                                           │
     3x Stepper                        ┌──────────────────┤
     Motors (28BYJ-48)                 ▼                  ▼
                                      RTC             IR Sensor
                                    (I2C)             (D15)
                                      │
                                      ▼
                                 LCD Display
                                  (I2C, 4-pin)
                                      │
                                   BUZZER (D43)
                                      │
                              4x Analog Sensors (P1–P4)
                              A0, A1, A2, A3
```

---

## 📊 Component BOM

### Core ICs

| Ref | Component | Value / Part | Function |
|:---:|:---|:---|:---|
| U7 | MCU | ATMEGA2560-16AU (TQFP-100) | Main microcontroller |
| U8 | USB-UART Bridge | CH340G | USB to serial for programming |
| U1, U9, U10 | Motor Driver | ULN2003AN (DIP-16) | Stepper motor driver (×3) |
| IC6 | LDO Regulator | NCP1117ST50T3G | 5V power regulator |
| Q2 | N-CH MOSFET | SI2302 (C2891732) | Motor power switching |

### Passive Components

| Ref | Value | Function |
|:---:|:---:|:---|
| R1 | 10 kΩ | SIM800L pull-up |
| R2 | 1 kΩ | SIM800L current limit |
| R5–R14, R18, R19 | 1 kΩ (×12) | LED current limiting resistors |
| R22 | 10 kΩ | RESET pull-up |
| R23–R25 | 1 kΩ | CH340G signal resistors |
| C1, C2 | 47 µF | Bulk supply decoupling |
| C3, C4 | 22 pF | MCU crystal load caps |
| C5 | 100 nF | Bypass cap |
| C6, C7, C8 | 10 nF | HF decoupling |
| C22–C25, C30, C31 | 100 nF | CH340G / MCU bypass caps |
| X2 | 16 MHz Crystal | CH340G oscillator |
| D1 | LED (×4) | Status LEDs: ORANGE, YELLOW, GREEN, RED |
| BUZZER | CMI-1295-0585T | Piezo buzzer for alerts (D43) |

---

## 🔌 Peripheral Connectors

| Connector | Type | Pins | Connected Signals |
|:---:|:---|:---:|:---|
| ST_MOTR_1 | B5B-XH 5-pin | 5 | Stepper Motor 1 coils (IN1–IN4 + GND) |
| ST_MOTR_2 | B5B-XH 5-pin | 5 | Stepper Motor 2 coils |
| ST_MOTR_3 | B5B-XH 5-pin | 5 | Stepper Motor 3 coils |
| SV_MOTR1 | HDR 3-pin | 3 | Servo Motor (D44, VCC, GND) |
| RTC | HDR-F 6-pin | 6 | I2C RTC Module (D20=SDA, D21=SCL) |
| SIM_MODULE1 | HDR 6-pin | 6 | SIM800L GSM Module (UART) |
| IR_CON | B3B-XH 3-pin | 3 | IR Sensor (+3V3, Signal D15, GND) |
| LCD_CON | B4B-XH 4-pin | 4 | I2C LCD (VCC, GND, D20, D21) |
| P1 | B2B-XH 2-pin | 2 | Analog Sensor 1 (A0, GND) |
| P2 | B2B-XH 2-pin | 2 | Analog Sensor 2 (A1, GND) |
| P3 | B2B-XH 2-pin | 2 | Analog Sensor 3 (A2, GND) |
| P4 | B2B-XH 2-pin | 2 | Analog Sensor 4 (A3, GND) |
| BAT | B2B-XH 2-pin | 2 | Battery Backup (VCC, GND) |
| DC1 | PJ-102B Barrel Jack | 3 | External DC Power Input |
| USB1 | Micro-USB | 5 | USB Programming / Power |

---

## 🎛️ GPIO Assignment Table

| Signal | ATMEGA2560 Pin | Function |
|:---|:---:|:---|
| TX / RX (USB) | PE0 / PE1 | CH340G UART Programming |
| D15 | PJ1 | IR Sensor Input |
| D20 (SDA) | PD1 | I2C — RTC + LCD |
| D21 (SCL) | PD0 | I2C — RTC + LCD |
| D43 | PL6 | Buzzer Output |
| D44 | PL5 | Servo Motor PWM |
| A0–A3 | PF0–PF3 | Analog Sensor Inputs (P1–P4) |
| ULN2003 IN1–IN4 (U1) | D2–D5 / D6–D9 | Stepper Motor 1 control |
| ULN2003 IN1–IN4 (U9) | — | Stepper Motor 3 control |
| ULN2003 IN1–IN4 (U10) | — | Stepper Motor 2 control |
| MOSI/MISO/SCK | PB2/PB3/PB1 | SPI / ICSP |
| D53 (SS) | PB0 | SPI Chip Select |

---

## ⚙️ Stepper Motor Driver — ULN2003AN

Three **ULN2003AN** Darlington transistor arrays drive three **28BYJ-48** unipolar stepper motors. [web:66] Each ULN2003AN handles 7 Darlington pairs, of which 4 are used per motor for the standard 4-phase coil sequence. [web:57] Motor supply is 5–12V via a dedicated `ST_MOTR_VCC` rail switched by the **SI2302 MOSFET (Q2)**.

```
ATMEGA2560 GPIO (IN1–IN4)  ──►  ULN2003AN  ──►  28BYJ-48 Stepper Motor
                                (500mA/ch)         (5V, 4-phase unipolar)
Step sequence: Blue→Pink→Yellow→Orange→Red
```

---

## 📡 GSM Module — SIM800L

The **SIM800L** connects via UART (Serial1) to the ATMEGA2560, enabling: [web:65]
- **Outbound SMS alerts** when dispensing occurs or errors are detected
- **Inbound SMS commands** to trigger manual dispensing remotely
- **GPRS data connectivity** for IoT logging (optional)

```
ATMEGA2560 (TX1/RX1) ──► SIM_MODULE1 header ──► SIM800L GSM Module
AT Command Interface  →  Quad-band 850/900/1800/1900 MHz
```

> ⚠️ SIM800L requires a **4.2V regulated supply at up to 2A peak** — a dedicated buck converter is recommended for the SIM module supply. [web:61]

---

## 🕐 RTC Module

The **RTC module** (e.g., DS3231 or DS1307) connects to the 6-pin RTC header over **I2C (D20/D21)**. It enables:
- Precise **time-triggered dispensing schedules** (morning/evening/custom)
- **Alarm interrupts** to wake the MCU from sleep for low-power operation
- Persistent timekeeping even during power cuts (battery-backed)

---

## 💡 Status LEDs

| LED Color | Signal | Purpose |
|:---:|:---:|:---|
| RED | A-series GPIO | Error / Fault indicator |
| ORANGE | A-series GPIO | Dispensing in progress |
| YELLOW | A-series GPIO | Low-level warning |
| GREEN | A-series GPIO | System OK / Ready |

All LEDs use **1kΩ current-limiting resistors** (R5–R14, R18, R19) to limit current to ~3.3mA per LED at 3.3V logic.

---

## 🔋 Power Architecture

```
External DC Input (Barrel Jack DC1)
   └──► NCP1117ST50T3G (5V LDO) ──► VCC (MCU, ICs, Peripherals)
                                 └──► ST_MOTR_VCC (Stepper supply via SI2302 MOSFET)
Micro-USB (USB1) ──► VBUS ──► CH340G + VCC (USB-powered mode)
BAT connector ──► Battery backup for RTC
```

---

## 📐 PCB Design Highlights

- **MCU:** ATMEGA2560-16AU TQFP-100 placed centrally
- **Motor Drivers:** U1, U9, U10 (ULN2003AN DIP-16) in IC sockets (S1, S2, S3) for replaceability
- **Bulk decoupling:** C1, C2 (47µF) + distributed 100nF ceramics near every IC
- **Crystal caps (C3, C4 = 22pF)** placed close to MCU XTAL pins
- **Reset button** (6×6×6mm tactile switch) accessible on top edge
- **ICSP header** (2×3, 2.54mm) for ISP programming fallback
- **SPI header (J10)** for external SPI peripherals
- **Comprehensive GPIO breakout headers** (H1–H10) exposing all digital and analog pins
- **SIM800L header** on left edge for direct module stacking

---

## 🧪 Bring-Up & Testing Procedure

### Step 1 — Power Supply Check
- Apply 7–12V DC to DC1 (barrel jack)
- Verify NCP1117ST50T3G output: **+5.0V** on VCC net
- Measure current draw in idle: ~80–120 mA (MCU + CH340G)

### Step 2 — USB Enumeration
- Connect Micro-USB to PC
- CH340G should enumerate as a **COM port** (Windows) or `/dev/ttyUSB0` (Linux)
- Upload a blink sketch via Arduino IDE (select **Arduino Mega 2560**)

### Step 3 — Stepper Motor Test
- Connect 28BYJ-48 motors to ST_MOTR_1/2/3
- Run 4-phase step sequence on each ULN2003AN
- Verify smooth rotation without stalling

### Step 4 — RTC Module Test
- Connect DS3231 to RTC header
- Read/write time via I2C (D20/D21)
- Confirm timekeeping accuracy over 24 hours

### Step 5 — GSM Module Test
- Insert SIM card into SIM800L
- Send AT command via Serial: `AT` → expect `OK`
- Send SMS: `AT+CMGS="+91XXXXXXXXXX"` → verify delivery

### Step 6 — Full System Integration
- Program dispensing schedule in RTC
- Verify stepper triggers at correct times
- Confirm SMS alert on dispensing event
- Test IR sensor presence detection and buzzer feedback

---

## 🖥️ Software / Firmware Stack

```
Libraries Required (Arduino IDE):
├── Wire.h              — I2C (RTC + LCD)
├── RTClib.h            — DS1307 / DS3231 RTC
├── LiquidCrystal_I2C.h — I2C LCD display
├── Stepper.h / AccelStepper.h  — Stepper motor control
├── Servo.h             — Servo motor (D44)
└── SoftwareSerial.h    — SIM800L UART communication
```

---

## 📁 Repository Structure

```
Automated-Dispenser/
├── images/
│   ├── TopView.jpg                          ← 3D board render
│   └── Schematic_Dispenser_project.pdf      ← Full schematic
├── Firmware/
│   └── dispenser_main/
│       └── dispenser_main.ino               ← Arduino sketch
├── Gerber/
│   └── (Gerber files for PCB fabrication)
├── BOM/
│   └── BOM_Dispenser.csv
└── README.md
```

---

## 🏷️ GitHub Topics to Add

```
atmega2560  arduino-mega  dispenser  stepper-motor  uln2003
sim800l  gsm  rtc  pcb-design  easyeda  automation  iot
28byj-48  i2c-lcd  embedded-hardware
```

---

## ⚠️ Design Notes & Cautions

- **ULN2003AN DIP-16 ICs** are in sockets — easy to replace if burned
- **SI2302 MOSFET (Q2)** switches stepper motor supply — ensure gate drive from MCU is sufficient (3.3V logic may need a pull-up for full enhancement)
- **SIM800L needs ~2A peak current** — use a dedicated 4.2V/2A supply, not the onboard 5V LDO
- **Crystal load capacitors (C3, C4 = 22pF)** must be placed within 5mm of MCU XTAL pins to avoid oscillation failure
- **RTC battery** (CR2032) should be connected to BAT connector for timekeeping during power loss

---

## 📄 License

This project is open for educational and personal use.  
© 2024 Janardhan BV — All rights reserved.

---

## 🙋 Author

**Janardhan BV**  
Embedded Hardware Engineer | PCB Design | Power Electronics  
📍 Bengaluru, India

---
*Designed in EasyEDA | ATMEGA2560 Automated Dispenser — Personal Project*
