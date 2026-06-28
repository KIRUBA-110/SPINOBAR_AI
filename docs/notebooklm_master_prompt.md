# 🗒️ NotebookLM Master Prompt — Spinobar Project

> **How to use:** Copy everything in the block below and paste it as a **new note/source** in NotebookLM.
> Then ask NotebookLM any question — it will use this as its knowledge base about your project.

---

````
=============================================================
PROJECT KNOWLEDGE BASE — SPINOBAR
For use with NotebookLM as a primary source document.
=============================================================

## SECTION 1: PROJECT IDENTITY

Project Name: Spinobar
Type: Final Year Engineering Project (Biomedical + Embedded Systems)
Stage: Early Prototype / Pre-Build (documentation complete, hardware not yet assembled)
One-Line Summary: Spinobar is a ₹4,000 device that tells doctors — in real time — exactly how hard a back brace is pushing on a patient's body, so they can adjust it perfectly instead of guessing.
Country: India 🇮🇳 (Made in India, all components locally sourced)
Domain: Biomedical Engineering / IoT / Embedded Systems / Mobile App Development

---

## SECTION 2: THE PROBLEM (Clinical Background)

### What is a Spinal Orthosis?
A spinal orthosis is a back brace — a rigid or semi-rigid device worn around the torso to hold the spine in the correct position. Doctors prescribe it for:
- Scoliosis (abnormal sideways curvature of the spine — the spine bends like an "S" or "C")
- Post-surgical spinal recovery
- Back injuries and disc problems (damaged cushions between spine bones)
- The brace works by applying corrective pressure at specific anatomical points on the body to gradually realign or support the spine.

### The Core Problem
When a doctor fits a spinal brace on a patient, they tighten it and then judge the fit purely by feel — by pressing with their hand and asking the patient "does this feel okay?" There is NO instrument available to measure the actual corrective force applied by the brace.

### 5 Specific Problems:
1. GUESSWORK: Doctors fit braces by hand/feel with zero objective measurement tool.
2. TOO MUCH PRESSURE: If the brace pushes too hard → painful pressure sores form on skin → patient stops wearing brace → treatment fails completely.
3. TOO LITTLE PRESSURE: If brace doesn't push hard enough → zero spinal correction → months of wearing the brace achieve nothing → condition worsens.
4. EXISTING TOOLS ARE UNAFFORDABLE: Devices that can measure brace pressure exist (Tekscan, Novel Pliance — USA/Germany) but cost ₹5,00,000 to ₹25,00,000 per unit. No average Indian hospital can afford this.
5. NO INDIAN SOLUTION: There is currently zero Indian-made device for this purpose. Everything must be imported, raising costs via import duties, shipping, and maintenance. No Indian-specific clinical data exists.

### Who Is Most Affected?
- Children with scoliosis are the most common patients. They must wear the brace 16 to 23 hours per day. Correct fit is absolutely critical.
- Indian hospitals and physiotherapy clinics cannot afford imported measurement tools.
- Researchers have no Indian dataset of real orthotic pressure values to study brace design.

---

## SECTION 3: THE SOLUTION — SPINOBAR

### What Spinobar Does
Spinobar is a low-cost, indigenous, real-time pressure monitoring system that answers the question: "How much pressure is the brace applying, where on the body, and when?"

### How It Works — 4 Stages

STAGE 1 — SENSOR PATCH:
- A thin, flexible patch (roughly palm-sized, ~10cm × 10cm) is placed inside the brace, between the brace and the patient's skin.
- The patch contains 16 FSR (Force Sensitive Resistor) sensors arranged in a 4×4 grid.
- FSR = Force Sensitive Resistor: a thin polymer sensor whose electrical resistance decreases as force/pressure increases.
- Each sensor is placed 2.5 cm apart to cover the brace contact area.

STAGE 2 — ELECTRONICS BRAIN (ESP32):
- The 16 sensors connect to a CD74HC4067 16-channel analog multiplexer.
- The multiplexer allows one analog pin on the ESP32 to read all 16 sensors one at a time.
- The ESP32 microcontroller reads all 16 values, applies calibration, and prepares a data packet.
- ESP32 specs: Dual-core CPU at 240 MHz, 4MB Flash, built-in Bluetooth BLE 4.2 + WiFi.

STAGE 3 — WIRELESS TRANSMISSION:
- The ESP32 transmits the pressure data over Bluetooth Low Energy (BLE) at 10 Hz (10 times per second) to a nearby smartphone.
- Real-time means: as the patient sits, stands, or walks, the values update live on the doctor's phone screen — no delay.

STAGE 4 — MOBILE APP:
- A Flutter app (Android + iOS) displays all 16 sensor readings as a live color-coded heatmap on the phone screen.
- Color coding: Green = safe (0–1.5N), Yellow = monitor (1.5–4N), Orange = high (4–7N), Red = danger/pressure sore risk (>7N).
- The doctor watches the heatmap in real time while adjusting the brace. Red zone = loosen. Green in correction zone = tighten.
- The app also logs session data as a CSV file for research purposes.

---

## SECTION 4: HARDWARE — BILL OF MATERIALS

### Primary Components:

COMPONENT 1 — FSR 402 (Interlink Electronics)
- Full name: Force Sensitive Resistor, Model 402
- Manufacturer: Interlink Electronics Inc., Westlake Village, CA, USA
- Part Number: 34-00012 (with female contacts), 44-29103 (bare), 30-81794 (solder tabs)
- Active sensing area: Ø 14.68 mm (circular)
- Thickness: 0.46 mm (ultra-thin, fits inside brace without causing discomfort)
- Force range: 0.2 N to 20 N
- Force resolution: Continuous (analog — no steps)
- Non-actuated resistance: >10 MΩ (effectively open circuit)
- Actuated resistance: ~200 Ω to 10 kΩ (varies with force)
- Repeatability (same unit): ±2%
- Repeatability (unit to unit): ±6% per batch
- Response time: <3 microseconds
- Hysteresis: 10% average
- Long-term drift: <5% × log₁₀(time in days) at 1 kg load
- Durability: 10 million actuations at 1 kg, 4 Hz
- Standing load: -5% drift after 2.5 kg for 24 hours
- Temperature range: -40°C to +85°C
- RoHS compliant, UL grade 94 V-1
- NOT precision measurement (not like a load cell). Good for relative force trends and pressure distribution.
- Quantity needed: 18 (16 for 4×4 grid + 2 spares)
- Indian price: ₹150 to ₹350 per unit
- Where to buy in India: Robu.in, Stemvolt.in, SunRobotics.in, ElectronicsComp.com
- Datasheet URL: https://www.interlinkelectronics.com/fsr-402

COMPONENT 2 — ESP32-WROOM-32 DevKit
- Manufacturer: Espressif Systems
- CPU: Dual-core Xtensa LX6 @ 80–240 MHz
- Flash: 4 MB
- RAM: 520 KB SRAM
- WiFi: 802.11 b/g/n
- Bluetooth: v4.2 BR/EDR + BLE
- ADC: 12-bit, but ONLY ADC1 pins are safe to use when BLE/WiFi is active (GPIO 32–39)
- Operating voltage: 3.0 V to 3.6 V
- Price in India: ₹350–₹600
- Quantity: 2 (1 + spare)
- Critical note: ADC2 pins (GPIO 0,2,4,12-15,25-27) DO NOT WORK reliably when WiFi or BLE is active. Always use ADC1 pins (GPIO 32-39) for sensor reading.
- Datasheet URL: https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf

COMPONENT 3 — CD74HC4067 (16-channel Analog Multiplexer)
- Manufacturer: Texas Instruments / Nexperia
- Function: 16-to-1 analog multiplexer. Allows 1 ADC pin to read 16 different analog sensors sequentially.
- Control: 4 digital pins (S0, S1, S2, S3) select channel 0–15 via binary addressing.
- Enable: Active LOW. Drive HIGH to disconnect all channels.
- Operating voltage: 2.0 V to 10 V (use at 3.3 V for ESP32 compatibility)
- On-resistance: ~70 Ω at 5V (negligible for voltage divider readings)
- Channels: 16 (C0 through C15)
- Switching: Break-before-make (safe for sensors)
- Settling time: Wait ~50 microseconds after switching channel before reading ADC
- Price in India: ₹40–₹80 per module
- Quantity: 2 (1 + spare)
- TI Datasheet: https://www.ti.com/lit/ds/symlink/cd74hc4067.pdf

COMPONENT 4 — 10 kΩ Resistors (pull-down)
- One per FSR sensor (16 total + 4 spares = 20)
- Used to form a voltage divider with each FSR
- Circuit: 3.3V → FSR → Node A → 10kΩ → GND; Node A connects to MUX channel input
- Price: ₹1–₹2 each

COMPONENT 5 — LiPo Battery (3.7V, 2000mAh)
- Powers ESP32 wirelessly
- Price: ₹300–₹500

COMPONENT 6 — TP4056 Charging Module
- Charges LiPo via USB
- Price: ₹30–₹60

COMPONENT 7 — HX711 + Load Cell (Calibration Reference Only)
- Used during calibration phase to apply known forces to FSR sensors
- Load cell range: 500g or 1kg
- HX711: 24-bit ADC module for load cells
- Price: ₹150–₹400 for the set
- HX711 Datasheet: https://cdn.sparkfun.com/datasheets/Sensors/ForceFlex/hx711_english.pdf

### Total BOM Cost:
Minimum: ₹4,260 | Maximum: ₹8,220
Target: ₹4,000–₹6,000 (stated project goal)
For comparison: Imported alternatives cost ₹5,00,000 to ₹25,00,000

---

## SECTION 5: CIRCUIT DESIGN

### Voltage Divider (per FSR):
3.3V → [FSR] → Node A → [10kΩ] → GND
Node A connects to MUX channel input (Cx)
V_OUT = 3.3V × (10,000) / (10,000 + R_FSR)
When no force: R_FSR > 10MΩ → V_OUT ≈ 0V → ADC reads ≈ 0
When pressed hard: R_FSR ≈ 200Ω → V_OUT ≈ 3.27V → ADC reads ≈ 4050

### Multiplexer to ESP32:
MUX S0 → ESP32 GPIO 27
MUX S1 → ESP32 GPIO 26
MUX S2 → ESP32 GPIO 25
MUX S3 → ESP32 GPIO 33
MUX SIG → ESP32 GPIO 34 (ADC1)
All 16 FSR voltage dividers → MUX C0 through C15

### Power:
LiPo 3.7V → TP4056 charger → ESP32 VIN
ESP32 outputs regulated 3.3V for FSR circuit

---

## SECTION 6: SOFTWARE ARCHITECTURE

### Firmware (Arduino C++ on ESP32):
1. Initialize MUX pins (S0–S3) as digital output, SIG pin (GPIO 34) as ADC input
2. Every 100ms (10 Hz): loop through channels 0–15, read ADC, apply calibration → 16 float values
3. Pack 16 values as 32-byte array (2 bytes per value: high byte + low byte)
4. Send via BLE notification to connected Flutter app
5. Use BLE GATT Server: Service UUID + Characteristic UUID with NOTIFY property

### Calibration:
- For each of the 16 sensors: apply 6 known weights (0g, 50g, 100g, 200g, 500g, 1000g) using load cell as reference
- Record ADC value at each weight
- Build a lookup table: ADC_value → Force_in_Newtons
- Use linear interpolation between table points
- One calibration table per sensor (16 tables total because units vary ±6%)

### Flutter Mobile App:
- Language: Dart, Framework: Flutter (Google)
- BLE library: flutter_blue_plus
- State management: Provider
- Data export: share_plus + path_provider
- Main screen: 4×4 GridView updated via StreamBuilder from BLE notification stream
- Color mapping: 0–300 ADC = Green, 301–700 = Yellow, 701–1800 = Orange, 1801–4095 = Red
- Record button: saves session as CSV with timestamp and 16 sensor values per row
- Target platforms: Android (primary), iOS (secondary)

---

## SECTION 7: HEATMAP COLOR SCALE (Clinical Thresholds)

Green:  0.0 – 1.5 N  (0 – 6 kPa)   = Safe, comfortable for skin
Yellow: 1.5 – 4.0 N  (6 – 20 kPa)  = Therapeutic range — monitor closely
Orange: 4.0 – 7.0 N  (20 – 35 kPa) = High — check patient comfort immediately
Red:    > 7.0 N      (> 35 kPa)    = Danger — pressure sore risk, loosen brace

Note: FSR 402 active area = π × (0.00734m)² = 1.69 × 10⁻⁴ m²
      So 1N on FSR 402 ≈ 5,900 Pa ≈ 5.9 kPa

---

## SECTION 8: 30-DAY BUILD TIMELINE

Week 1 (Days 1–7): Order all hardware → Set up Arduino IDE + Flutter → Wire and test single FSR → Wire all 16 FSRs through multiplexer → Verify 16 readings in Serial Monitor.
Milestone: 16 sensor values visible on Serial Monitor.

Week 2 (Days 8–14): Set up load cell calibration reference → Calibrate all 16 FSR sensors → Build calibration lookup tables → Assemble physical 4×4 sensor patch on foam/silicone → Solder perf board with MUX.
Milestone: Calibrated physical patch producing Newton values.

Week 3 (Days 15–21): Write ESP32 BLE server firmware → Create Flutter app → Implement BLE scan/connect → Parse 32-byte data packet → Build live heatmap widget with color coding.
Milestone: Live heatmap on phone screen, updating from real sensor data.

Week 4 (Days 22–30): Add data logging (CSV export) → Validate accuracy against load cell → Test repeatability → Build enclosure → Polish app UI → Film demo video → Finalize documentation.
Milestone: Submission-ready prototype with demo video.

---

## SECTION 9: KNOWN LIMITATIONS & MITIGATIONS

1. FSR non-linear response (logarithmic) → Use calibration lookup table + linear interpolation
2. ±6% part-to-part variation → Calibrate each of the 16 sensors individually
3. 10% hysteresis (loading ≠ unloading) → Average 3 readings per measurement
4. Humidity drift (+30% worst case) → Re-zero sensor offset periodically in firmware
5. ESP32 ADC2 breaks with BLE → Only use ADC1 pins (GPIO 32-39)
6. BLE MTU limit (default 20 bytes for 16 sensors = 32 bytes) → Request MTU 64 during connection negotiation
7. Android 12+ requires BLUETOOTH_SCAN and BLUETOOTH_CONNECT permissions → Declare in AndroidManifest.xml

---

## SECTION 10: GLOSSARY

FSR (Force Sensitive Resistor): Thin flexible sensor whose resistance decreases when pressed.
PTF (Polymer Thick Film): The material technology used inside FSRs.
ADC (Analog to Digital Converter): Converts a voltage (0–3.3V) into a number (0–4095 for 12-bit).
BLE (Bluetooth Low Energy): Low power wireless protocol for short-range data transmission.
ESP32: A dual-core microcontroller chip by Espressif with built-in WiFi and BLE.
MUX / Multiplexer: An IC that routes one of many input signals to a single output. CD74HC4067 is 16-to-1.
GATT: Bluetooth protocol layer for data exchange (Generic Attribute Profile).
Flutter: Google's framework to build apps for Android and iOS from a single codebase (Dart language).
Calibration: Teaching the device what a "known" force feels like so it can measure unknowns accurately.
Load Cell: A precision weight sensor used as a reference during FSR calibration.
Heatmap: A color-coded grid where color represents intensity — here, pressure values.
Scoliosis: A medical condition where the spine curves sideways (like an S or C shape).
Spinal Orthosis: A medical back brace used to correct or support the spine.
Voltage Divider: A circuit using two resistors (one fixed, one variable like FSR) to produce a variable output voltage.
Interdigitated Electrodes: The comb-like finger pattern on the bottom layer of an FSR that creates more contact points under pressure.

---

## SECTION 11: STAKEHOLDER IMPACT

Patients: Correct brace fit → no pressure sores → comfortable → they wear it consistently → spine improves
Doctors/Physiotherapists: Objective data instead of guesswork → better clinical decisions → improved outcomes
Children with Scoliosis: Most vulnerable group; wear braces 16–23 hours/day; correct fit is absolutely critical
Hospitals & Clinics: Affordable device (₹4,000–₹6,000) they can actually purchase and deploy
Researchers: First-ever Indian dataset of real orthotic pressure values → enables better brace design for Indian anatomy

---

## SECTION 12: COMPETITIVE COMPARISON

Tekscan / Novel Pliance (USA/Germany):
- Cost: ₹5,00,000 to ₹25,00,000
- Made in India: No
- Mobile app: No (proprietary software)
- Local support: No
- Indian clinical data: No
- Affordable for Indian clinics: No

Spinobar:
- Cost: ₹4,000–₹6,000
- Made in India: Yes
- Mobile app: Yes (Android + iOS via Flutter)
- Local support: Yes
- Indian clinical data: Yes (first time)
- Affordable for Indian clinics: Yes

Spinobar delivers comparable core functionality at less than 1% of the cost of imported alternatives.

---

## SECTION 13: PROJECT FILES

readme.md — Project overview, architecture, hardware, software, getting started guide
spinobar.md — Full plain-language explanation of the project for non-technical staff/examiners
spinobar_slides.md — 10-slide presentation content ready to paste into PowerPoint/Google Slides
FSR_Sensors_datasheet.pdf — Official Interlink Electronics FSR 400 Series datasheet
docs/build_plan.md — Full BOM with prices, datasheets, circuit notes, software stack
docs/30_day_roadmap.md — Day-by-day task list for 30-day build
docs/fsr_402_datasheet_notes.md — Obsidian notes from FSR datasheet with circuits and code
docs/ai_datasheet_prompts.md — Prompt templates for reading component datasheets with AI

---

END OF SPINOBAR KNOWLEDGE BASE
=============================================================
````

---

## 💡 How to Use This in NotebookLM

### Option A — As a Source (Recommended)
1. Open [NotebookLM](https://notebooklm.google.com)
2. Create a new notebook called **"Spinobar"**
3. Click **"Add Source"** → choose **"Copied Text"** or **"Paste text"**
4. Copy everything inside the ``` block above and paste it
5. NotebookLM will index it — now ask anything

### Option B — As a Chat Message
1. Copy the block and paste it directly into the chat
2. Follow with your question immediately

---

## 🔥 Suggested Questions to Ask NotebookLM After Adding This

```
1. Generate a 5-minute oral presentation script for my project review based on this.

2. Write 10 expected viva questions with model answers for this project.

3. Create a research paper abstract (250 words) for Spinobar.

4. What are the gaps in this project? What would examiners criticize?

5. Compare Spinobar with existing literature on wearable pressure monitoring systems.

6. Write a methodology section (400 words) suitable for a project report.

7. List all the components and explain why each was chosen over alternatives.

8. Suggest 3 future scope improvements for Spinobar beyond the prototype.

9. Write a conclusion paragraph (150 words) for the project report.

10. Create a risk register table for Spinobar (Risk | Likelihood | Impact | Mitigation).
```
