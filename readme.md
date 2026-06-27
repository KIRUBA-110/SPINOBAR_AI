<div align="center">

# ⠀⠀SPINOBAR

### A Low-Cost, Real-Time Pressure Monitoring System for Spinal Orthoses

**Replacing guesswork with precision — Made in India 🇮🇳**

---

[![Status](https://img.shields.io/badge/Status-Prototype%20Phase-orange?style=for-the-badge)](/)
[![Platform](https://img.shields.io/badge/Hardware-ESP32-blue?style=for-the-badge&logo=espressif)](/)
[![App](https://img.shields.io/badge/App-Flutter-02569B?style=for-the-badge&logo=flutter)](/)
[![Wireless](https://img.shields.io/badge/Wireless-BLE%204.2-6E3AFA?style=for-the-badge)](/)
[![Cost](https://img.shields.io/badge/Build%20Cost-₹4%2C000–₹6%2C000-green?style=for-the-badge)](/)

</div>

---

## 📖 What Is Spinobar?

**Spinobar** is an indigenous, affordable pressure measurement system designed to help doctors precisely fit spinal orthoses (back braces) on patients.

When a patient wears a spinal brace — for scoliosis, post-surgical recovery, or disc problems — the brace applies corrective pressure at specific points on the body. Currently, doctors have **no way to objectively measure this pressure**. They tighten the brace and guess. This leads to:

| Problem | Consequence |
|---|---|
| 🔴 Too much pressure | Painful pressure sores → patient stops wearing brace → treatment fails |
| ⬜ Too little pressure | Zero spinal correction → months of treatment wasted |
| 💸 Existing solutions | Cost ₹5–₹25 Lakh (Tekscan, Novel Pliance) — unaffordable for Indian hospitals |

**Spinobar answers one critical question:**

> *"How much pressure is the brace applying, where on the body, and when?"*

For **₹4,000–₹6,000** to build.

---

## ✨ Features

- 📍 **16-point pressure mapping** — 4×4 grid of FSR sensors
- 📱 **Live heatmap** on Android/iOS via Flutter app
- 📡 **Bluetooth Low Energy (BLE)** — completely wireless
- 🧮 **Calibrated readings** in Newtons (N) — not raw ADC values
- 💾 **Session recording** — export timestamped CSV data
- 🔋 **Battery-powered** — wearable during normal patient activity
- 🇮🇳 **Made in India** — all components sourced locally, < ₹6,000 total

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────────┐
│              SENSOR PATCH (Wearable)              │
│                                                   │
│   S01  S02  S03  S04   ← 16x FSR 402 sensors    │
│   S05  S06  S07  S08     arranged in 4×4 grid   │
│   S09  S10  S11  S12     inside spinal brace    │
│   S13  S14  S15  S16                             │
│                    │                              │
│            [CD74HC4067]  ← 16-to-1 analog MUX   │
│             16-channel                            │
└──────────────────────┬───────────────────────────┘
                       │ 1 analog wire + 4 control pins
                       ▼
            ┌──────────────────┐
            │   ESP32-WROOM    │  ← Reads ADC, calibrates,
            │   Microcontroller│    packages data, transmits
            │   + LiPo Battery │    via BLE @ 10 Hz
            └────────┬─────────┘
                     │ Bluetooth Low Energy
                     ▼
            ┌──────────────────┐
            │  Spinobar App    │  ← Flutter (Android + iOS)
            │  Live Heatmap    │    Green / Yellow / Red
            │  Data Logger     │    pressure zones
            └──────────────────┘
```

---

## 🧰 Hardware Components

| Component | Model | Qty | Est. Price | Datasheet |
|---|---|---|---|---|
| Pressure Sensor | Interlink FSR 402 | 18 (16 + spares) | ₹150–350 each | [FSR 400 Series](https://www.interlinkelectronics.com/fsr-402) |
| Microcontroller | ESP32-WROOM-32 | 2 (1 + spare) | ₹400–600 | [Espressif Docs](https://www.espressif.com/en/products/socs/esp32) |
| Analog Multiplexer | CD74HC4067 | 2 (1 + spare) | ₹40–80 | [TI Datasheet](https://www.ti.com/lit/ds/symlink/cd74hc4067.pdf) |
| Pull-down Resistors | 10 kΩ | 20 | ₹1–2 each | — |
| Battery | LiPo 3.7V 2000mAh | 1 | ₹300–500 | — |
| Charger | TP4056 Module | 1 | ₹30–60 | — |
| Calibration Ref | Load Cell + HX711 | 1 set | ₹150–400 | [HX711 PDF](https://cdn.sparkfun.com/datasheets/Sensors/ForceFlex/hx711_english.pdf) |
| **Total** | | | **₹4,260–₹8,220** | |

---

## 📱 Software Stack

| Layer | Technology |
|---|---|
| Firmware | Arduino C++ on ESP32 |
| BLE Library | ESP32 BLE Arduino (built-in) |
| Mobile App | Flutter (Dart) — Android + iOS |
| BLE Plugin | `flutter_blue_plus` |
| State Management | Provider |
| Data Export | `share_plus` + `path_provider` |

---

## 🔌 Circuit

### Voltage Divider (per FSR sensor)

```
    3.3V
      │
    [FSR 402]         R_FSR decreases when pressed
      │
      ├──────── GPIO 34 (ADC1, ESP32)
      │         Reads V_OUT
    [10 kΩ]
      │
     GND

V_OUT = 3.3V × (10kΩ) / (10kΩ + R_FSR)
```

### Multiplexer Wiring (CD74HC4067)

```
ESP32 GPIO 27 → S0  ┐
ESP32 GPIO 26 → S1  ├ Channel select (binary 0–15)
ESP32 GPIO 25 → S2  │
ESP32 GPIO 33 → S3  ┘
ESP32 GPIO 34 → SIG   (single analog read line)
All 16 FSRs   → C0–C15 (one per channel)
```

---

## 🎨 Heatmap Color Scale

| Color | Force Range | Pressure | Clinical Meaning |
|---|---|---|---|
| 🟢 **Green** | 0 – 1.5 N | 0 – 6 kPa | Safe, comfortable for skin |
| 🟡 **Yellow** | 1.5 – 4.0 N | 6 – 20 kPa | Therapeutic range — monitor |
| 🟠 **Orange** | 4.0 – 7.0 N | 20 – 35 kPa | High — check patient comfort |
| 🔴 **Red** | > 7.0 N | > 35 kPa | Danger — pressure sore risk |

---

## 🚀 Getting Started

### Prerequisites

- [Arduino IDE 2.x](https://www.arduino.cc/en/software) with ESP32 board support
- [Flutter SDK](https://flutter.dev/docs/get-started/install)
- Android device (Android 8.0+) or iOS device (iOS 13+)

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/spinobar.git
cd spinobar
```

### 2. Flash the Firmware

```bash
# Open in Arduino IDE
# Board: ESP32 Dev Module
# Flash: firmware/spinobar_firmware.ino
# Port: your ESP32 COM port
```

### 3. Run the Flutter App

```bash
cd app/spinobar_app
flutter pub get
flutter run
```

### 4. Connect & Use

1. Power on the Spinobar device
2. Open the app → tap **Scan**
3. Select **Spinobar** from the device list
4. Place sensor patch inside the brace
5. Watch the live heatmap update as pressure is applied

---

## 📁 Repository Structure

```
spinobar/
│
├── 📄 readme.md                        ← You are here
├── 📄 spinobar.md                      ← Full project explanation (plain language)
├── 📄 spinobar_slides.md               ← 10-slide presentation content
├── 📄 FSR_Sensors_datasheet.pdf        ← FSR 400 Series official datasheet
│
├── 📂 docs/
│   ├── build_plan.md                   ← Full BOM, circuit, software stack
│   ├── 30_day_roadmap.md              ← Day-by-day build timeline
│   ├── fsr_402_datasheet_notes.md      ← Obsidian notes: FSR datasheet
│   └── ai_datasheet_prompts.md         ← Prompt templates for reading datasheets
│
├── 📂 firmware/                        ← ESP32 Arduino firmware (coming soon)
│   └── spinobar_firmware/
│
└── 📂 app/                             ← Flutter mobile app (coming soon)
    └── spinobar_app/
```

---

## 📊 Specifications Summary

| Parameter | Value |
|---|---|
| Sensor count | 16 (4×4 grid) |
| Sensor model | Interlink FSR 402 |
| Active area per sensor | Ø 14.68 mm |
| Force range | 0.2 N – 20 N |
| Force resolution | Continuous analog |
| Wireless | Bluetooth LE 4.2 |
| Update rate | 10 Hz (100ms per full scan) |
| ADC resolution | 12-bit (0–4095) |
| Battery | LiPo 3.7V 2000mAh |
| Target battery life | > 4 hours continuous |
| App platform | Android + iOS (Flutter) |
| Build cost | ₹4,000 – ₹6,000 |
| Comparable imported device | ₹5,00,000 – ₹25,00,000 |

---

## 🗺️ Roadmap

- [x] Project concept & problem definition
- [x] Literature review & component selection
- [x] Bill of materials & cost analysis
- [x] 30-day build plan
- [x] FSR datasheet analysis
- [ ] Firmware v1 — single FSR read
- [ ] Firmware v2 — 16-channel MUX scan
- [ ] Firmware v3 — calibration + BLE broadcast
- [ ] Flutter app v1 — BLE connect + raw values
- [ ] Flutter app v2 — live 4×4 heatmap
- [ ] Flutter app v3 — data logging + CSV export
- [ ] Physical patch assembly
- [ ] Full system integration test
- [ ] Validation against load cell reference
- [ ] Final demo & submission

---

## 🎯 Impact

| Stakeholder | How Spinobar Helps |
|---|---|
| 👤 Patients | Correct brace fit → no pressure sores → comfortable → they actually wear it → spine improves |
| 👨‍⚕️ Doctors | No more guessing → objective data → better clinical decisions |
| 🧒 Children with scoliosis | Most common brace users (16–23 hrs/day) — correct fit is critical |
| 🏥 Hospitals & Clinics | Affordable (₹4,000) device they can actually buy |
| 🔬 Researchers | First Indian dataset of real orthotic pressure values |

---

## 📚 Documentation

| Document | Description |
|---|---|
| [Build Plan](docs/build_plan.md) | Full hardware BOM, datasheets, circuit diagrams, software stack |
| [30-Day Roadmap](docs/30_day_roadmap.md) | Day-by-day task list to build Spinobar in one month |
| [FSR Datasheet Notes](docs/fsr_402_datasheet_notes.md) | Deep-dive notes on FSR 402 sensor with code snippets |
| [AI Prompt Templates](docs/ai_datasheet_prompts.md) | Prompts to efficiently read component datasheets with AI |

---

## 🙏 Acknowledgements

- **Interlink Electronics** — FSR 400 Series sensors and datasheet
- **Espressif Systems** — ESP32 microcontroller platform
- **Flutter / Google** — Cross-platform mobile app framework
- **SparkFun & Adafruit** — Component guides and tutorials

---

<div align="center">

**One-line summary:**

*"Spinobar is a ₹4,000 device that tells doctors — in real time — exactly how hard a back brace is pushing on a patient's body, so they can adjust it perfectly instead of guessing."*

---

Made with ❤️ in India

</div>
