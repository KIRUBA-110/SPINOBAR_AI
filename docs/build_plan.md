 🚀 Spinobar — Complete Build Plan

> **Goal:** Build a working prototype of Spinobar — a 16-sensor pressure monitoring system for spinal orthoses.

---

## 🧰 Bill of Materials (BOM) — Complete with Datasheets & Sources

### 1. Pressure Sensors

#### Option A — FSR 402 (Recommended for accuracy)

| Parameter | Value |
|---|---|
| **Type** | Force Sensitive Resistor (Polymer Thick Film) |
| **Sensing Area** | 14.7 mm circular active area |
| **Force Range** | 0.1 N – 10.0 N |
| **Thickness** | 0.45 mm (ultra-thin, fits inside brace) |
| **Resistance (no load)** | > 10 MΩ |
| **Resistance (full load)** | ~1 kΩ |
| **Qty needed** | 16 (for 4×4 grid) + 2 spares = **18 units** |
| **Price in India** | ₹150–₹350 per sensor |
| **Total cost** | ~₹3,000–₹5,500 for 18 sensors |

📄 **Datasheets & Resources:**
- [Official Interlink Electronics FSR 402 Datasheet](https://www.interlinkelectronics.com/fsr-402)
- [SparkFun FSR 402 Hookup Guide](https://learn.sparkfun.com/tutorials/force-sensitive-resistor-hookup-guide)
- [Adafruit FSR Overview + Circuit Guide](https://learn.adafruit.com/force-sensitive-resistor-fsr/using-an-fsr)

🛒 **Buy in India:**
- [Robu.in — FSR 402](https://robu.in)
- [Electronicscomp.com — Grove FSR402 Module](https://electronicscomp.com)
- [SunRobotics.in](https://sunrobotics.in)
- [Stemvolt.in](https://stemvolt.in)
- IndiaMART (for bulk orders)

> ⚠️ **Note:** FSRs are logarithmic, not linear. You **must** calibrate each one individually against a known reference (load cell). Expect ±10–20% variation between units.

---

#### Option B — Velostat Sheet (Cheapest DIY alternative)

| Parameter | Value |
|---|---|
| **Type** | Carbon-impregnated polyolefin film (piezoresistive) |
| **Thickness** | ~0.1 mm |
| **Volume Resistivity** | < 500 ohm-cm |
| **Surface Resistivity** | < 31,000 ohms/sq |
| **Temp Range** | -45°C to 65°C |
| **Cost** | ~₹200–₹400 for a full sheet (cut 16 patches yourself) |
| **Drawback** | Higher variability; needs careful calibration |

📄 **Datasheets & Resources:**
- [Adafruit Velostat Product Page + Specs](https://www.adafruit.com/product/1361)
- [Adafruit DIY Sensor Guide using Velostat](https://learn.adafruit.com/force-sensitive-resistor-fsr/fabricating-sensors)

🛒 **Buy in India:**
- [Thingbits Electronics (Mumbai)](https://thingbits.in)
- [Fab.to.Lab](https://fabtolab.com)
- [MG Super Labs](https://mgsuperlabs.co.in)
- [Element14 India — Adafruit #1361](https://element14.com)

> 💡 **Recommendation:** Use FSR 402 for your prototype (cleaner data, faster calibration). Switch to Velostat in mass production to cut cost.

---

### 2. Microcontroller — ESP32-WROOM-32

| Parameter | Value |
|---|---|
| **CPU** | Dual-core Xtensa LX6 @ 240 MHz |
| **Flash** | 4 MB |
| **RAM** | 520 KB SRAM |
| **Wi-Fi** | 802.11 b/g/n |
| **Bluetooth** | v4.2 BLE + Classic |
| **ADC** | 12-bit, 18 channels (use ADC1 pins only: GPIO 32–39) |
| **Operating Voltage** | 3.0 V – 3.6 V |
| **Price in India** | ₹350–₹600 (DevKit board) |
| **Qty needed** | 1 + 1 spare = **2 units** |

📄 **Datasheets & Resources:**
- [Official Espressif ESP32-WROOM-32 Datasheet (PDF)](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf)
- [ESP32 Technical Reference Manual (PDF)](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)
- [Last Minute Engineers — ESP32 Pinout Reference](https://lastminuteengineers.com/esp32-pinout-reference/)

🛒 **Buy in India:**
- [Robu.in — ESP32 DevKit](https://robu.in)
- [Probots.co.in](https://probots.co.in)
- [ElectronicsComp.com](https://electronicscomp.com)
- Amazon.in — Search "ESP32 WROOM DevKit"

---

### 3. Analog Multiplexer — CD74HC4067 (16-channel)

> **Why you need this:** ESP32 has only ~6 reliable ADC pins. The MUX routes all 16 FSRs through a single ADC pin — ESP32 selects and reads them one at a time.

| Parameter | Value |
|---|---|
| **Type** | 16-channel analog multiplexer/demultiplexer |
| **Control Pins** | 4 digital pins (S0–S3) for binary channel select (0–15) |
| **SIG Pin** | 1 analog output connected to ESP32 ADC |
| **Operating Voltage** | 2.0 V – 10 V (use at 3.3 V for ESP32 compatibility) |
| **On-Resistance** | ~70 Ω @ 5V |
| **Price in India** | ₹40–₹80 per module |
| **Qty needed** | 1 + 1 spare = **2 units** |

📄 **Datasheets & Resources:**
- [Texas Instruments CD74HC4067 Datasheet (official PDF)](https://www.ti.com/lit/ds/symlink/cd74hc4067.pdf)
- [Nexperia 74HC4067 Datasheet (PDF)](https://assets.nexperia.com/documents/data-sheet/74HC_HCT4067.pdf)
- [SparkFun CD74HC4067 Breakout Board Guide](https://learn.sparkfun.com/tutorials/multiplexer-breakout-hookup-guide)

🛒 **Buy in India:**
- Robu.in — search "16 channel analog mux"
- ElectronicsComp.com
- Amazon.in — search "CD74HC4067 breakout module"

---

### 4. Supporting Components

| Component | Purpose | Qty | Est. Price | Where to Buy |
|---|---|---|---|---|
| **10 kΩ Resistors** | Voltage divider for each FSR | 20 | ₹1–₹2 each | Local electronics shop |
| **LiPo Battery (3.7V, 2000mAh)** | Wireless power for ESP32 | 1 | ₹300–₹500 | Robu.in, Amazon |
| **TP4056 Charging Module** | Charge LiPo via USB-C | 1 | ₹30–₹60 | Robu.in |
| **Flexible Ribbon Cable** | Connect sensor patch to board | 1 set | ₹100–₹300 | PCBWay, JLCPCB |
| **Foam / Silicone Pad** | House sensors in patch form | 1 | ₹50–₹150 | Local hardware store |
| **Breadboard + Jumper Wires** | Prototyping | 1 set | ₹150–₹200 | Local electronics shop |
| **Perf Board / Small PCB** | Final circuit assembly | 1 | ₹50–₹100 | Robu.in, local |
| **Load Cell (500g/1kg) + HX711** | Calibration reference standard | 1 set | ₹150–₹400 | Robu.in |

📄 **HX711 Load Cell Resources:**
- [HX711 Datasheet (PDF)](https://cdn.sparkfun.com/datasheets/Sensors/ForceFlex/hx711_english.pdf)
- [SparkFun Load Cell + HX711 Hookup Guide](https://learn.sparkfun.com/tutorials/load-cell-amplifier-hx711-breakup-hookup-guide)

---

### 💸 Total BOM Cost Estimate

| Category | Cost (INR) |
|---|---|
| 18× FSR 402 sensors | ₹2,700–₹5,400 |
| 2× ESP32 DevKit | ₹700–₹1,200 |
| 2× CD74HC4067 MUX module | ₹80–₹160 |
| Resistors, wires, misc | ₹300–₹500 |
| LiPo battery + TP4056 charger | ₹330–₹560 |
| Load cell + HX711 (calibration) | ₹150–₹400 |
| **TOTAL** | **₹4,260–₹8,220** |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     SENSOR PATCH (4×4)                      │
│  FSR01  FSR02  FSR03  FSR04                                 │
│  FSR05  FSR06  FSR07  FSR08   ──→  CD74HC4067 MUX          │
│  FSR09  FSR10  FSR11  FSR12        (16-channel analog MUX)  │
│  FSR13  FSR14  FSR15  FSR16   ←──  Select: S0,S1,S2,S3     │
└─────────────────────────────────────────────────────────────┘
                        │ SIG (1 analog line)
                        ▼
              ┌─────────────────┐
              │   ESP32-WROOM   │
              │  - Reads ADC    │
              │  - Cycles MUX   │
              │  - Packages     │
              │  - BLE Notify → │
              └────────┬────────┘
                       │ Bluetooth Low Energy (BLE)
                       ▼
              ┌─────────────────┐
              │  Flutter App    │
              │  (Android/iOS)  │
              │  Live Heatmap   │
              │  Data Logging   │
              └─────────────────┘
```

### Circuit: Voltage Divider for Each FSR

```
3.3V ──── [FSR] ──── Node A ──── [10kΩ] ──── GND
                         │
                    ADC Pin on ESP32
```

**Logic:** When FSR is pressed → resistance drops → voltage at Node A rises → ADC reads higher value → maps to higher pressure on heatmap.

---

## 🛠️ Software Stack

| Layer | Technology | Resource |
|---|---|---|
| **Firmware** | Arduino C++ on ESP32 | [arduino-esp32 GitHub](https://github.com/espressif/arduino-esp32) |
| **BLE Library** | ESP32 BLE Arduino (built-in) | Arduino IDE ESP32 examples |
| **Mobile App** | Flutter (Dart) | [flutter.dev](https://flutter.dev) |
| **BLE Plugin** | flutter_blue_plus | [pub.dev/packages/flutter_blue_plus](https://pub.dev/packages/flutter_blue_plus) |
| **State Management** | Provider / Riverpod | [pub.dev/packages/provider](https://pub.dev/packages/provider) |
| **Data Export** | share_plus + path_provider | [pub.dev/packages/share_plus](https://pub.dev/packages/share_plus) |

---

## ⚠️ Critical Risks & Mitigations

| Risk | Mitigation |
|---|---|
| FSR sensors vary ±10–20% | Calibrate each sensor individually against load cell |
| ESP32 ADC2 breaks when Wi-Fi is on | Use only ADC1 pins (GPIO 32–39) for the MUX SIG line |
| BLE MTU limit (default 20 bytes) | Request MTU 64+ bytes during connection negotiation |
| Parts take >1 week to arrive | Order on Day 1, start software setup immediately |
| Sensor wires breaking in flex patch | Use silicone-coated wire; heat-shrink all connections |
| Flutter BLE permissions on Android 12+ | Add correct permissions in AndroidManifest.xml |

---

## 📚 Learning Resources (In Recommended Order)

1. **ESP32 + FSR + Multiplexer** — [Random Nerd Tutorials](https://randomnerdtutorials.com)
2. **ESP32 BLE Server** — [Random Nerd Tutorials BLE Guide](https://randomnerdtutorials.com/esp32-bluetooth-low-energy-ble-arduino-ide/)
3. **Flutter BLE App** — [flutter_blue_plus GitHub examples](https://github.com/boskokg/flutter_blue_plus)
4. **Custom Heatmap in Flutter** — Flutter `CustomPainter` tutorial on YouTube
5. **FSR Calibration** — [SparkFun Load Cell Hookup Guide](https://learn.sparkfun.com/tutorials/load-cell-amplifier-hx711-breakup-hookup-guide)
