# 🤖 AI Datasheet Reading Prompt Templates

Use these prompts to efficiently extract maximum knowledge from datasheets with minimum token usage.

---

## 🚀 Master Prompt — For Any Datasheet (Paste This First)

```
You are an expert electronics engineer helping me build [PROJECT NAME].

I am going to give you a datasheet. Extract ONLY the following and format as Obsidian markdown:

**Project Context:** [Brief description — e.g. "ESP32 + 16 FSR sensors + BLE + Flutter heatmap app for measuring spinal brace pressure"]

**Output format (use exactly this structure):**

1. ONE-LINER: What this component does (1 sentence max)
2. KEY SPECS TABLE: Only specs relevant to my project (markdown table)
3. HOW IT WORKS: Physical/electrical principle (max 150 words + ASCII diagram if needed)
4. CIRCUIT: Exact wiring for my microcontroller (pinout + formula if needed)
5. GOTCHAS: Top 3 limitations that will affect my project
6. CODE SNIPPET: Minimal working code to read/use this component (language: [Arduino C++ / Python / etc])
7. PART NUMBER: Which exact variant to buy for my use case
8. CALIBRATION: How to calibrate if needed (steps only, no fluff)

Rules:
- Skip history, marketing, applications unrelated to my project
- Skip absolute maximum ratings table (I'll look it up if needed)
- No long paragraphs — bullet points and tables only
- If something is not relevant to my project, skip it entirely
- Flag with ⚠️ anything that could cause a bug or hardware failure

Datasheet text follows:
[PASTE DATASHEET TEXT HERE]
```

---

## 📄 Prompt Variants by Component Type

### For SENSORS (FSR, Load Cell, IMU, Temperature)

```
Read this sensor datasheet for my [PROJECT].

My setup: [MCU] + [power supply voltage] + [communication: I2C/SPI/analog]

Give me:
1. Sensing range and resolution (what it can detect)
2. Output type (analog voltage / digital / I2C register)
3. Exact circuit to connect to [MCU] (ASCII diagram)
4. V_out formula or I2C address
5. Calibration steps if needed
6. 3 biggest gotchas for my setup
7. Recommended pull-down/pull-up resistor value

Skip: history, block diagrams, ESD ratings, storage conditions.
Output: Obsidian markdown, tables where possible.

Datasheet:
```

---

### For MICROCONTROLLERS / SoCs (ESP32, STM32, Arduino)

```
Read this MCU datasheet for my [PROJECT].

I need to know:
1. Which ADC pins are safe to use (list only usable ones)
2. ADC resolution and reference voltage
3. Which pins to AVOID and why
4. BLE/WiFi power requirements
5. Boot strapping pins (which ones must not be pulled low/high)
6. Recommended decoupling capacitor values
7. Maximum current per GPIO pin

Skip: electrical absolute max table, packaging info, ordering info.
Output: Obsidian markdown with callouts for warnings (> [!WARNING]).
```

---

### For MULTIPLEXERS / ICs (CD74HC4067, etc.)

```
Read this IC datasheet for my [PROJECT].

I need:
1. What this chip does (1 line)
2. Control pins and how to address channels (binary table: S0 S1 S2 S3 → channel)
3. Signal pin — what connects to my MCU ADC
4. Operating voltage (is it 3.3V safe?)
5. On-resistance — does it affect my sensor readings?
6. Enable pin behavior
7. How long to wait after channel switch before reading (settling time)
8. Minimal circuit diagram (ASCII)

Skip everything else.
```

---

### For WIRELESS MODULES (BLE, WiFi, LoRa)

```
Read this wireless module datasheet for my [PROJECT].

Output only:
1. Frequency / protocol / range
2. Power consumption in TX / RX / sleep mode
3. Interface to MCU (SPI/UART/I2C/native)
4. Antenna type — do I need external antenna?
5. AT commands or library to use
6. Maximum data packet size
7. Latency (how fast does data arrive end-to-end)
8. Any conflict with other peripherals on same MCU

Skip: regulatory certifications, mechanical drawings.
```

---

### For POWER COMPONENTS (LiPo, TP4056, voltage regulators)

```
Read this power component datasheet.

My system: [voltage needed] + [current draw estimate: X mA]

Extract:
1. Input/output voltage range
2. Maximum current output
3. Protection features (overcharge, short circuit, over-temp)
4. Charging current and how to set it (resistor value?)
5. Status LEDs / indicator pins
6. Thermal limits
7. Wiring diagram (ASCII)

Output: Obsidian markdown with > [!CAUTION] for anything dangerous.
```

---

## 🏃 Speed Mode — Absolute Minimum Prompt (for simple components)

Use when you just need the basics fast:

```
Datasheet → 5-bullet summary for [COMPONENT] used in [PROJECT]:
1. What it does
2. Key spec (range/voltage/current)
3. How to wire to ESP32 (ASCII or one-line)
4. Code to read it (10 lines max)
5. One thing that will trip me up

Datasheet:
```

---

## 📋 How to Use These Prompts (Workflow)

### Step 1 — Extract Text from PDF
```bash
# On Linux/Mac:
pdftotext your_datasheet.pdf - | head -n 500

# Or open PDF in browser → Ctrl+A → Ctrl+C → paste into prompt
```

### Step 2 — Choose the Right Prompt
| Component | Use Prompt |
|---|---|
| FSR, pressure sensor | Sensors prompt |
| ESP32, Arduino | MCU prompt |
| CD74HC4067, MUX | Multiplexer/IC prompt |
| BLE module, radio | Wireless prompt |
| Battery, charger | Power prompt |
| Unknown/misc | Master prompt |

### Step 3 — Fill In Blanks
Replace all `[BRACKETS]` with your actual project details before pasting.

### Step 4 — Paste the Datasheet
- Paste the full extracted text after the prompt
- If the datasheet is very long (>5000 words), paste only these sections:
  - Features / Description
  - Electrical Characteristics table
  - Application Circuit / Typical Application
  - Pin Descriptions
  - Skip: Packaging information, absolute maximum ratings (unless debugging)

### Step 5 — Save Output to Obsidian
- Copy AI output → paste into new Obsidian note
- File naming convention: `[component]_datasheet_notes.md`
- Add tags: `#datasheet #spinobar #hardware #[component-name]`

---

## 🗂️ Spinobar Datasheets Queue

Process these in order (most critical first):

| # | Component | Datasheet | Notes File | Status |
|---|---|---|---|---|
| 1 | FSR 402 | `FSR_Sensors_datasheet.pdf` | `fsr_402_datasheet_notes.md` | ✅ Done |
| 2 | ESP32-WROOM-32 | [Download from Espressif](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf) | `esp32_datasheet_notes.md` | ⬜ Pending |
| 3 | CD74HC4067 | [Download from TI](https://www.ti.com/lit/ds/symlink/cd74hc4067.pdf) | `mux_cd74hc4067_notes.md` | ⬜ Pending |
| 4 | HX711 (Load Cell) | [Download from SparkFun](https://cdn.sparkfun.com/datasheets/Sensors/ForceFlex/hx711_english.pdf) | `hx711_loadcell_notes.md` | ⬜ Pending |
| 5 | TP4056 (Charger) | Search: "TP4056 datasheet PDF" | `tp4056_charger_notes.md` | ⬜ Pending |

---

## 💡 Token-Saving Tips

1. **Never paste the whole PDF blindly** — extract text first, then paste only relevant sections
2. **Use the Speed Mode prompt** for simple components (resistors, caps, LEDs)
3. **Give your project context upfront** — the AI skips irrelevant content automatically
4. **Ask for tables** — they compress information better than paragraphs
5. **Say "skip X"** explicitly — saves 30–40% of output tokens
6. **Request ASCII diagrams** instead of asking for image descriptions
7. **Batch simple questions**: "Read all 3 of these short datasheets and give me a combined comparison table"

---

*Part of the Spinobar project documentation*  
*Prompt templates — use for all component datasheets*
