# 📅 Spinobar — 30-Day Daily Roadmap

> A precise, day-by-day task list to take Spinobar from zero to a working demo in 30 days.
> Each day has a **primary task**, **expected output**, and **time estimate**.

---

## 🗓️ WEEK 1 — Setup & Procurement (Days 1–7)

---

### Day 1 — Order All Hardware

**Goal:** Get everything ordered so parts arrive by Day 5–7.

| # | Task | Details |
|---|---|---|
| 1 | Order 18× FSR 402 sensors | From Robu.in or Stemvolt.in |
| 2 | Order 2× ESP32 DevKit v1 | From Robu.in or Probots.co.in |
| 3 | Order 2× CD74HC4067 MUX module | Robu.in or Amazon.in |
| 4 | Order 1× Load Cell (500g) + HX711 module | Robu.in |
| 5 | Order 1× LiPo 3.7V 2000mAh battery | Robu.in |
| 6 | Order 1× TP4056 charger module | Robu.in |
| 7 | Buy locally: 10kΩ resistors (×30), breadboard, jumper wires, perf board | Local electronics shop |

**⏱ Time:** 1–2 hours
**✅ Output:** All items ordered, delivery tracking noted

---

### Day 2 — Development Environment Setup

**Goal:** Have all software tools installed and working.

| # | Task | Command / Link |
|---|---|---|
| 1 | Install Arduino IDE 2.x | https://www.arduino.cc/en/software |
| 2 | Add ESP32 board support to Arduino IDE | Paste URL in Board Manager: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json` |
| 3 | Install Flutter SDK | https://flutter.dev/docs/get-started/install/linux |
| 4 | Install Android Studio + set up Android emulator | For Flutter app development |
| 5 | Connect ESP32 to laptop via USB — verify COM port appears | |
| 6 | Flash "Blink" sketch on ESP32 — LED blinks = board works ✅ | File → Examples → 01.Basics → Blink |

**⏱ Time:** 3–4 hours
**✅ Output:** ESP32 blinks, Flutter `flutter doctor` shows all green

---

### Day 3 — Learn FSR Basics + Wire First Sensor

**Goal:** Understand how one FSR sensor works in a circuit.

| # | Task |
|---|---|
| 1 | Read: [SparkFun FSR Hookup Guide](https://learn.sparkfun.com/tutorials/force-sensitive-resistor-hookup-guide) (30 min) |
| 2 | Wire 1× FSR + 10kΩ resistor as voltage divider on breadboard |
| 3 | Connect output to GPIO 34 (ADC1 pin) on ESP32 |
| 4 | Write sketch: read ADC → print to Serial Monitor every 500ms |
| 5 | Press FSR with finger → observe values change in Serial Monitor |
| 6 | Record min (no press) and max (full press) raw ADC values |

**⏱ Time:** 2–3 hours
**✅ Output:** Serial Monitor shows live changing numbers when FSR is pressed

---

### Day 4 — Understand Multiplexer (CD74HC4067)

**Goal:** Learn how the 16-channel MUX works.

| # | Task |
|---|---|
| 1 | Read: [CD74HC4067 TI Datasheet](https://www.ti.com/lit/ds/symlink/cd74hc4067.pdf) — pages 1–6 only |
| 2 | Read: [SparkFun Multiplexer Hookup Guide](https://learn.sparkfun.com/tutorials/multiplexer-breakout-hookup-guide) |
| 3 | Wire MUX to ESP32: S0→GPIO 27, S1→GPIO 26, S2→GPIO 25, S3→GPIO 33, SIG→GPIO 34 |
| 4 | Wire 3 FSRs to MUX channels C0, C1, C2 |
| 5 | Write sketch: loop through channels 0–2, read ADC, print values |
| 6 | Press each FSR and confirm correct channel reads |

**⏱ Time:** 3–4 hours
**✅ Output:** 3 FSR values printed per loop, each responds independently

---

### Day 5 — Parts Arrive (Catch-Up / Rest Day)

**Goal:** Receive and verify parts; use day for reading if parts haven't arrived.

| # | Task |
|---|---|
| 1 | Check all orders — verify quantities and condition |
| 2 | Test each FSR individually on breadboard |
| 3 | If parts haven't arrived: Study [ESP32 BLE tutorial](https://randomnerdtutorials.com/esp32-bluetooth-low-energy-ble-arduino-ide/) |
| 4 | Optional: Start Flutter "Hello World" app |

**⏱ Time:** Flexible
**✅ Output:** All 18 FSRs visually verified; ESP32s confirmed working

---

### Day 6 — Wire All 16 FSRs through MUX

**Goal:** Full 4×4 sensor array connected and readable.

| # | Task |
|---|---|
| 1 | Wire all 16 FSRs, each with a 10kΩ pull-down, to MUX channels C0–C15 |
| 2 | Update firmware to cycle through channels 0–15 |
| 3 | Read all 16 ADC values in a loop |
| 4 | Print as a comma-separated row in Serial Monitor |
| 5 | Press each sensor one by one — confirm all 16 respond |

**⏱ Time:** 4–5 hours (mostly wiring)
**✅ Output:** Serial Monitor shows 16 values per line, each changes on press

---

### Day 7 — Week 1 Review & Cleanup

**Goal:** Clean up wiring, document the circuit, plan Week 2.

| # | Task |
|---|---|
| 1 | Re-wire neatly using colour-coded jumper wires |
| 2 | Draw a simple circuit schematic by hand or in [draw.io](https://draw.io) |
| 3 | Save schematic as `docs/circuit_diagram.png` |
| 4 | Take a photo of the breadboard setup |
| 5 | Write a short note: what worked, what was tricky |

**⏱ Time:** 2–3 hours
**✅ Output:** Neat breadboard, saved circuit diagram, notes written

---

## 🗓️ WEEK 2 — Calibration & Physical Patch (Days 8–14)

---

### Day 8 — Load Cell Setup (Calibration Reference)

**Goal:** Get a reference measurement tool working.

| # | Task |
|---|---|
| 1 | Read: [HX711 + Load Cell Hookup Guide](https://learn.sparkfun.com/tutorials/load-cell-amplifier-hx711-breakup-hookup-guide) |
| 2 | Wire HX711 to ESP32: DT→GPIO 16, SCK→GPIO 17 |
| 3 | Install HX711 library in Arduino IDE |
| 4 | Run calibration sketch — place known weights (50g, 100g, 200g) on load cell |
| 5 | Record the HX711 raw value for each known weight |
| 6 | Create a simple lookup: raw_value → grams |

**⏱ Time:** 3–4 hours
**✅ Output:** Load cell accurately reports weight of a 100g object ±5g

---

### Day 9 — Calibrate FSR Sensors

**Goal:** Map each FSR's ADC output to real force values.

| # | Task |
|---|---|
| 1 | Place FSR flat on a rigid surface |
| 2 | Place load cell + flat plunger on top |
| 3 | Apply weights: 0g, 50g, 100g, 200g, 500g |
| 4 | Record ADC value from FSR AND actual grams from load cell at each step |
| 5 | Repeat for all 16 FSRs |
| 6 | Create a calibration table in `docs/calibration_data.csv` |

**⏱ Time:** 4–6 hours
**✅ Output:** CSV file with calibration curve data for all 16 sensors

---

### Day 10 — Calibration Curve Fitting

**Goal:** Convert raw ADC → pressure in N/cm² mathematically.

| # | Task |
|---|---|
| 1 | Open calibration data in Excel / Google Sheets |
| 2 | Plot ADC value vs. force (grams) for each sensor |
| 3 | Fit a polynomial or use a lookup table approach |
| 4 | Write a C++ function `float adcToPressure(int channel, int adcVal)` |
| 5 | Add this function to ESP32 firmware |
| 6 | Test: press FSR with known weight → firmware should print correct grams |

**⏱ Time:** 3–4 hours
**✅ Output:** Firmware prints pressure in Newtons (not raw ADC values)

---

### Day 11 — Design the Physical Sensor Patch

**Goal:** Plan the layout of the 4×4 patch on paper before building.

| # | Task |
|---|---|
| 1 | Draw a 4×4 grid on paper, scaled to actual size |
| 2 | Spacing: each FSR ~2.5 cm apart → patch ~10cm × 10cm total |
| 3 | Plan wire routing so wires don't overlap sensors |
| 4 | Choose base material: silicone sheet, neoprene, or thin foam |
| 5 | Buy fabric/foam from local shop if not already available |
| 6 | Label each FSR position: Row 1–4, Col 1–4 |

**⏱ Time:** 2–3 hours
**✅ Output:** Physical patch layout drawn, materials ready

---

### Day 12 — Build Physical Sensor Patch (Part 1)

**Goal:** Mount sensors on patch material.

| # | Task |
|---|---|
| 1 | Cut silicone/foam sheet to 12cm × 12cm |
| 2 | Mark 16 positions in a 4×4 grid |
| 3 | Attach each FSR with thin double-sided tape |
| 4 | Solder thin flexible wire to each FSR terminal |
| 5 | Label each wire: S01 through S16 |
| 6 | Keep wires bundled and running to one edge |

**⏱ Time:** 4–5 hours
**✅ Output:** 16 FSRs physically mounted on patch, each wire labeled

---

### Day 13 — Build Physical Sensor Patch (Part 2) + Perf Board

**Goal:** Connect patch wires to a central board with MUX.

| # | Task |
|---|---|
| 1 | Solder MUX (CD74HC4067) to perf board |
| 2 | Solder 16 × 10kΩ resistors (one per sensor channel) |
| 3 | Connect patch wires to MUX channels |
| 4 | Connect ESP32 via a ribbon cable or connector |
| 5 | Power the circuit from USB and verify all 16 sensors still read |
| 6 | Fix any broken solder joints |

**⏱ Time:** 4–6 hours
**✅ Output:** Soldered compact board, all 16 sensors working

---

### Day 14 — Week 2 Integration Test

**Goal:** Full hardware system test — patch + board + ESP32.

| # | Task |
|---|---|
| 1 | Place the sensor patch flat on a surface |
| 2 | Press different spots with a finger |
| 3 | Verify correct sensor lights up in Serial Monitor |
| 4 | Press all 16 positions and confirm each responds |
| 5 | Measure power consumption with USB current meter |
| 6 | Note any dead sensors for re-soldering |

**⏱ Time:** 2–3 hours
**✅ Output:** All 16 sensors reliably detected, patch behaves like a heatmap

---

## 🗓️ WEEK 3 — BLE Firmware + Flutter App (Days 15–21)

---

### Day 15 — ESP32 BLE Server Setup

**Goal:** Get ESP32 broadcasting sensor data over Bluetooth.

| # | Task |
|---|---|
| 1 | Read: [Random Nerd Tutorials — ESP32 BLE](https://randomnerdtutorials.com/esp32-bluetooth-low-energy-ble-arduino-ide/) |
| 2 | Create BLE Server with a custom Service UUID |
| 3 | Create one Characteristic with **NOTIFY** property |
| 4 | Pack 16 sensor values as byte array (2 bytes per sensor = 32 bytes) |
| 5 | Notify every 100 ms (10 Hz update rate) |
| 6 | Use nRF Connect (free Android app) to verify BLE broadcast |

**⏱ Time:** 3–4 hours
**✅ Output:** nRF Connect app on phone sees Spinobar BLE device and receives data

---

### Day 16 — Flutter Project Setup

**Goal:** Create the Flutter app skeleton.

| # | Task | Command |
|---|---|---|
| 1 | Create Flutter project | `flutter create spinobar_app` |
| 2 | Add packages to pubspec.yaml | `flutter pub add flutter_blue_plus provider` |
| 3 | Configure Android permissions in `AndroidManifest.xml` | Add BLUETOOTH, BLUETOOTH_SCAN, BLUETOOTH_CONNECT, ACCESS_FINE_LOCATION |
| 4 | Build and run app on emulator — verify no errors | `flutter run` |
| 5 | Create basic app structure: `lib/screens/`, `lib/services/`, `lib/widgets/` | |

**⏱ Time:** 2–3 hours
**✅ Output:** Flutter app runs on phone/emulator, folder structure created

---

### Day 17 — BLE Scan & Connect in Flutter

**Goal:** Flutter app finds and connects to ESP32.

| # | Task |
|---|---|
| 1 | Create `BleService` class in `lib/services/ble_service.dart` |
| 2 | Implement: scan → find "Spinobar" device → connect |
| 3 | Show device list on a Scan Screen |
| 4 | Tap device → connect and navigate to main screen |
| 5 | Show connection status (Connected / Disconnected) in UI |
| 6 | Test: app connects to ESP32 within 5 seconds |

**⏱ Time:** 3–4 hours
**✅ Output:** Flutter app connects to ESP32 over BLE reliably

---

### Day 18 — Parse BLE Data in Flutter

**Goal:** Receive and decode the 32-byte sensor data packet.

| # | Task |
|---|---|
| 1 | Discover service UUID and characteristic UUID |
| 2 | Subscribe to characteristic notifications |
| 3 | Parse each incoming byte array: extract 16 × 16-bit integers |
| 4 | Store as a `List<int> sensorValues` (length 16) |
| 5 | Print all 16 values to the Flutter console |
| 6 | Press sensors → verify values change in Flutter debug console |

**⏱ Time:** 3–4 hours
**✅ Output:** 16 live sensor values visible in Flutter debug console

---

### Day 19 — Build the Heatmap Widget

**Goal:** Visualize 16 values as a 4×4 color-coded grid.

| # | Task |
|---|---|
| 1 | Create `HeatmapWidget` using Flutter `GridView.builder` (4 columns) |
| 2 | Color logic: 0–300 → Green, 301–700 → Yellow, 701–4095 → Red |
| 3 | Show numeric value inside each cell |
| 4 | Show sensor number (S1–S16) as a small label |
| 5 | Use `StreamBuilder` to rebuild grid on every new BLE packet |
| 6 | Test: press sensors → see grid cells change color in real time |

**⏱ Time:** 4–5 hours
**✅ Output:** Live 4×4 colored grid on phone screen that reacts to pressure

---

### Day 20 — App UI Polish

**Goal:** Make the app look presentable for demos.

| # | Task |
|---|---|
| 1 | Add app icon and name "Spinobar" |
| 2 | Add top bar showing: device name, connection status, battery level |
| 3 | Add a max pressure indicator ("Peak: 850 / 4095") |
| 4 | Add color legend: Green = Safe, Yellow = Caution, Red = Risk |
| 5 | Add a "Disconnect" button |
| 6 | Test full flow: open app → scan → connect → see live heatmap |

**⏱ Time:** 3–4 hours
**✅ Output:** Professional-looking app with a working live heatmap

---

### Day 21 — End-to-End System Test

**Goal:** Patch on brace → phone shows live heatmap.

| # | Task |
|---|---|
| 1 | Place sensor patch inside a real spinal brace (or simulate with belt) |
| 2 | Wear brace (or have someone wear it) |
| 3 | Open Spinobar app → connect |
| 4 | Observe heatmap change as brace is tightened |
| 5 | Video record this test |
| 6 | Note any issues (lag, wrong sensor positions, dead pixels) |

**⏱ Time:** 2–3 hours
**✅ Output:** First real-world end-to-end demo video recorded

---

## 🗓️ WEEK 4 — Validation, Data Logging & Demo Prep (Days 22–30)

---

### Day 22 — Data Logging Feature

**Goal:** Save sensor sessions to a CSV file.

| # | Task |
|---|---|
| 1 | Add `path_provider` and `share_plus` packages to Flutter |
| 2 | Create "Record" button — starts a timed session |
| 3 | Write each BLE packet as a row: `timestamp, s1, s2, ..., s16` |
| 4 | "Stop & Save" exports CSV to phone storage |
| 5 | Test: record 60-second session, open CSV in Excel |

**⏱ Time:** 3–4 hours
**✅ Output:** CSV file with timestamped pressure readings for a full session

---

### Day 23 — Accuracy Validation

**Goal:** Prove the device measures correctly.

| # | Task |
|---|---|
| 1 | Place FSR patch on load cell, apply 5 known weights |
| 2 | Record Spinobar reading vs. actual weight at each step |
| 3 | Calculate error percentage: `error% = (|spinobar - actual| / actual) × 100` |
| 4 | Target: < 15% error for each sensor |
| 5 | Document results in `docs/validation_results.md` |

**⏱ Time:** 3–4 hours
**✅ Output:** Validation table showing accuracy data for the device

---

### Day 24 — Repeatability Testing

**Goal:** Prove the device gives consistent readings.

| # | Task |
|---|---|
| 1 | Press the same sensor 20 times with the same weight |
| 2 | Record 20 readings |
| 3 | Calculate standard deviation and coefficient of variation |
| 4 | Target: CV < 10% (i.e., consistent within 10%) |
| 5 | Repeat for 4 different sensors (corners of the grid) |
| 6 | Add to `docs/validation_results.md` |

**⏱ Time:** 2–3 hours
**✅ Output:** Repeatability data table proving device consistency

---

### Day 25 — Firmware Optimization & Power Testing

**Goal:** Make firmware clean and test battery life.

| # | Task |
|---|---|
| 1 | Remove all Serial.print() debug statements from final firmware |
| 2 | Optimize loop: read sensors → pack → notify → delay 100ms |
| 3 | Enable ESP32 light sleep between BLE notifications |
| 4 | Charge LiPo to full, disconnect USB |
| 5 | Run continuously until battery dies — record runtime |
| 6 | Target: > 4 hours continuous operation |

**⏱ Time:** 2–3 hours + waiting
**✅ Output:** Final clean firmware, battery life measured

---

### Day 26 — Enclosure / Packaging

**Goal:** Make the hardware look clean and wearable.

| # | Task |
|---|---|
| 1 | 3D print or buy a small plastic enclosure for ESP32 + board |
| 2 | Mount LiPo inside enclosure with TP4056 accessible via USB port |
| 3 | Route sensor ribbon cable through a slot |
| 4 | Label the enclosure "Spinobar v1.0" |
| 5 | Attach the sensor patch to the enclosure with a connector |
| 6 | Take clean product photos |

**⏱ Time:** 4–6 hours (including print time)
**✅ Output:** Device looks like a real product, not a breadboard mess

---

### Day 27 — Documentation Update

**Goal:** Complete all project documentation.

| # | Task |
|---|---|
| 1 | Update `README.md` with: project overview, circuit diagram, how to build, how to run app |
| 2 | Update `docs/build_plan.md` with any changes made during build |
| 3 | Add final photos to `docs/` folder |
| 4 | Write `docs/user_manual.md`: how a doctor uses Spinobar in clinic |
| 5 | Commit everything to Git: `git add . && git commit -m "v1.0 complete"` |

**⏱ Time:** 3–4 hours
**✅ Output:** GitHub repo fully documented with photos and instructions

---

### Day 28 — Presentation Preparation

**Goal:** Create a polished presentation using `spinobar_slides.md`.

| # | Task |
|---|---|
| 1 | Open `spinobar_slides.md` and create slides in Google Slides / PowerPoint |
| 2 | Add photos of your actual prototype to each slide |
| 3 | Replace suggested visuals with real screenshots of your Flutter app |
| 4 | Add the validation data table to the Results slide |
| 5 | Rehearse the 5-minute demo presentation |

**⏱ Time:** 4–5 hours
**✅ Output:** Polished slide deck with real project photos and data

---

### Day 29 — Demo Video Recording

**Goal:** Create a 2–3 minute demo video for review/submission.

| # | Task |
|---|---|
| 1 | Script the demo: problem → device → live demo → results |
| 2 | Set up good lighting |
| 3 | Record: show device, open app, connect, press sensors, see heatmap |
| 4 | Show the validation data on screen |
| 5 | Edit video (free tools: DaVinci Resolve, CapCut) |
| 6 | Upload to Google Drive / YouTube (unlisted) |

**⏱ Time:** 4–5 hours
**✅ Output:** Professional 2-minute demo video

---

### Day 30 — Final Review & Buffer

**Goal:** Fix anything broken, polish, submit.

| # | Task |
|---|---|
| 1 | Run complete system test from scratch (cold start) |
| 2 | Fix any remaining bugs |
| 3 | Final Git commit: `git commit -m "Final submission v1.0"` |
| 4 | Submit documentation, demo video, and presentation |
| 5 | 🎉 Celebrate — you built Spinobar! |

**⏱ Time:** 2–4 hours
**✅ Output:** Submission-ready Spinobar prototype

---

## 📊 Week-by-Week Summary

| Week | Focus | Key Milestone |
|---|---|---|
| **Week 1** (Days 1–7) | Setup + all hardware wired | 16 sensor values on Serial Monitor |
| **Week 2** (Days 8–14) | Calibration + physical patch | Calibrated patch on perf board |
| **Week 3** (Days 15–21) | BLE firmware + Flutter heatmap app | Live heatmap on phone from real sensors |
| **Week 4** (Days 22–30) | Validation + polish + demo | Submission-ready prototype + video |

---

## 🧠 Daily Rules to Stay on Track

1. **Commit to Git every single day** — even if it's just notes
2. **Take a photo of your breadboard/circuit every day** — you'll need it for the report
3. **If stuck > 1 hour on a problem** — move to the next task and come back later
4. **Order Day 1 means Day 1** — do not delay the purchase
5. **Test early, test often** — don't build everything and then debug all at once

---

> 💬 **30-day one-liner:**
> *"Day 1: order parts. Day 7: 16 sensors on serial. Day 14: calibrated patch. Day 21: live heatmap on phone. Day 30: demo video submitted."*
