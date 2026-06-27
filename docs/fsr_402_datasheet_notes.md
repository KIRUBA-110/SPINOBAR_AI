# 📦 FSR 400 Series — Datasheet Notes
**Source:** Interlink Electronics FSR® 400 Series Official Datasheet  
**Relevant Models:** FSR 402 (primary), FSR 406 (alternative)  
**Used in:** Spinobar — 4×4 pressure sensor patch for spinal orthoses  

---

## 🔖 Tags
`#spinobar` `#hardware` `#sensor` `#FSR` `#datasheet` `#pressure` `#electronics`

---

## 1. What Is an FSR?

> [!NOTE] One-liner
> A **Force Sensing Resistor (FSR)** is a thin, flexible sensor whose **electrical resistance decreases as pressure increases**. Press harder → resistance drops → voltage rises.

- **Type:** Polymer Thick Film (PTF) device
- **Manufacturer:** Interlink Electronics, Inc.
- **Family:** FSR® 400 Series (7 models — 400, 400 Short, 402, 402 Short, 404, 406, 408)
- **Standard:** RoHS Compliant, UL grade 94 V-1 materials
- **Generates no EMI, Not ESD Sensitive**

---

## 2. How It Works — Physical Principle

### Internal Layer Structure (3 Layers)

```
┌─────────────────────────────────────┐  ← Top Layer (Flexible polymer substrate)
│      [Active Sensing Area]          │     Acts as the pressing surface
├─────────────────────────────────────┤
│    [Conductive PTF Ink / Spacer]    │  ← Middle: Resistive polymer thick film
│    (separates layers at rest)       │     Contact area increases under force
├─────────────────────────────────────┤
│  [Interdigitated Electrode Pattern] │  ← Bottom Layer
│  ████ ████ ████ ████ ████ ████ ████│     Conductive but non-touching fingers
└─────────────────────────────────────┘
         ← 2-wire output terminals →
```

### Working Principle (Step by Step)

| State | What Happens | Resistance |
|---|---|---|
| **No Force** | Top layer separated from electrodes by air/spacer gap | > 10 MΩ (open circuit) |
| **Light Touch** | Polymer makes first contact with some electrode fingers | ~100 kΩ |
| **Medium Press** | More contact area created between polymer and electrodes | ~10 kΩ |
| **Full Press** | Maximum contact — almost all electrode fingers bridged | ~200 Ω – 1 kΩ |

> [!IMPORTANT] Key Insight
> It is NOT truly piezoresistive (like strain gauges). It works by **increasing physical contact area** between the conductive polymer and the interdigitated electrodes. More pressure = more contact paths = lower resistance.

---

## 3. Model Comparison — Which One to Use?

| Model | Active Area | Thickness | Switch Travel | Best For |
|---|---|---|---|---|
| **FSR 400** | Ø 5.08 mm | 0.30 mm | 0.05 mm | Fingertip / button touch |
| **FSR 400 Short** | Ø 5.62 mm | 0.30 mm | 0.05 mm | Compact fingertip |
| **FSR 402** ⭐ | Ø **14.68 mm** | 0.46 mm | **0.15 mm** | **General pressure spots** |
| **FSR 402 Short** | Ø 12.70 mm | 0.46 mm | 0.15 mm | Tighter grid spacing |
| **FSR 404** | Ø 4.35 mm (donut) | 0.53 mm | 0.05 mm | Over a pin/bolt |
| **FSR 406** | 39.6 × 39.6 mm | 0.46 mm | 0.15 mm | Large area coverage |
| **FSR 408** | 10.2 mm × Xmm strip | 0.41 mm | 0.15 mm | Linear pressure strips |

> [!TIP] Spinobar Uses FSR 402
> - Active area: **Ø 14.68 mm** — large enough to feel pressure from the brace
> - Thickness: **0.46 mm** — ultra-thin, fits inside brace without discomfort
> - Switch travel: **0.15 mm** — enough deformation range for orthotic pressures

---

## 4. Key Electrical Specifications

| Parameter | Value | Notes |
|---|---|---|
| **Actuation Force (min)** | ~0.2 N | Minimum force to register any reading |
| **Force Sensitivity Range** | 0.2 N – 20 N | Useful measurement window |
| **Force Resolution** | Continuous (analog) | Not stepped — smooth gradient |
| **Non-Actuated Resistance** | > 10 MΩ | Effectively open circuit when not pressed |
| **Actuated Resistance** | ~200 Ω – 10 kΩ | Varies with force applied |
| **Response Time** | < 3 microseconds | Essentially instantaneous |
| **Repeatability (single part)** | ± 2% | Excellent consistency per sensor |
| **Repeatability (part to part)** | ± 6% | Between different units of same batch |
| **Hysteresis** | +10% average | Reading differs slightly loading vs. unloading |
| **Long Term Drift** | < 5% × log₁₀(time) | At 1 kg load over 35 days |
| **Durability** | 10 million actuations | At 1 kg, 4 Hz tap |
| **Standing Load Durability** | -5% drift after 2.5 kg for 24 hrs | Good for sustained brace wear |

---

## 5. Temperature Performance

| Condition | Duration | Resistance Change |
|---|---|---|
| Cold: -40°C | 1 hour | -5% average |
| Hot: +85°C | 1 hour | -15% average |
| Hot Humid: +85°C 95%RH | 1 hour | +10% average |
| Storage Cold: -25°C | 120 hours | -10% average |
| Storage Hot: +85°C | 120 hours | -5% average |
| Storage Humid: +85°C 95%RH | 240 hours | +30% average |

> [!WARNING] Humidity Effect
> Humidity causes the **largest drift (+30%)**. For a wearable device in contact with skin, sweat = humidity. Always calibrate in wearing conditions and account for this in software.

---

## 6. The Voltage Divider Circuit — How to Read FSR with ESP32

### Circuit Diagram

```
        3.3V (from ESP32)
          │
          │
        [FSR]   ← Force Sensing Resistor (R_FSR)
          │
          ├──────────── GPIO 34 (ADC1 pin on ESP32)
          │             reads V_OUT
        [10kΩ]  ← Measuring Resistor (R_M)
          │
         GND
```

### Output Voltage Formula

$$V_{OUT} = V_{+} \times \frac{R_M}{R_M + R_{FSR}}$$

Where:
- `V+` = 3.3V (ESP32 supply)
- `R_M` = 10,000 Ω (fixed resistor)
- `R_FSR` = variable (decreases with force)

### What Happens in Practice

| Force Applied | R_FSR | R_M/(R_M + R_FSR) | V_OUT | ADC Value (12-bit) |
|---|---|---|---|---|
| None (no press) | > 10 MΩ | ≈ 0.001 | ≈ 0.003V | ≈ 0 |
| Light touch | ~100 kΩ | 0.091 | 0.30V | ~370 |
| Medium press | ~10 kΩ | 0.50 | 1.65V | ~2048 |
| Hard press | ~1 kΩ | 0.91 | 3.0V | ~3724 |
| Maximum force | ~200 Ω | 0.98 | 3.24V | ~4020 |

### Choosing R_M Value

| R_M Value | Best For | Trade-off |
|---|---|---|
| 1 kΩ | Low-force sensing (< 1N) | Saturates quickly at high force |
| **10 kΩ** ⭐ | **Mid-range (1N–10N) — ideal for Spinobar** | **Best balance** |
| 100 kΩ | High-force detection (> 10N) | Low resolution at light forces |

> [!TIP] Spinobar Resistor Choice
> Use **10 kΩ** pull-down. Orthotic brace pressure typically falls in the 1N–8N range per cm² — exactly where 10kΩ gives the best resolution.

---

## 7. Resistance vs. Force — The Response Curve

The FSR response is **logarithmic**, not linear:

```
Resistance
(Ohms, log scale)
    │
10M ┤ ●  (no force)
    │  \
1M  ┤   \
    │    \
100k┤     \
    │      \
10k ┤       ●
    │        \
1k  ┤         \
    │           ●
100 ┤            ●──●
    └────────────────────
    0   2   5   10   20   Force (N)
```

**Key takeaway:** A 10× change in force produces roughly a 10× change in resistance (decade behavior on log scale).

### Why This Matters for Spinobar

The ADC reads voltage, not resistance. After the voltage divider:
- At low forces: ADC changes a lot per Newton (high sensitivity)
- At high forces: ADC changes less per Newton (lower sensitivity)

**This is actually good** for our use case — we want sensitivity at the therapeutic pressure range (2N–6N), not at extreme forces.

---

## 8. Calibration Strategy for Spinobar

### The Problem
Each FSR unit varies ±6% from each other. Raw ADC values ≠ Newtons. We need a calibration map per sensor.

### Step-by-Step Calibration Protocol

```
Step 1: Setup
  - Place FSR on hard flat surface
  - Put a small rigid disc (₹5 coin works) on active area
  - Connect to ESP32 voltage divider circuit

Step 2: Apply Known Weights
  Apply: 0g, 50g, 100g, 200g, 500g, 1000g
  Record: ADC raw value for each weight
  Convert: grams → Newtons (÷ 101.97)

Step 3: Build Calibration Table
  Example output:
  | Force (N) | Grams | ADC Raw |
  |-----------|-------|---------|
  | 0.0       | 0     | 0       |
  | 0.49      | 50    | 380     |
  | 0.98      | 100   | 720     |
  | 1.96      | 200   | 1200    |
  | 4.90      | 500   | 2400    |
  | 9.81      | 1000  | 3600    |

Step 4: Implement in Firmware (Linear Interpolation)
```

### ESP32 Calibration Firmware Code

```cpp
// Calibration lookup table (fill after measuring your sensors)
const int NUM_POINTS = 6;
const int   adcVals[NUM_POINTS]    = {0, 380, 720, 1200, 2400, 3600};
const float newtonVals[NUM_POINTS] = {0.0, 0.49, 0.98, 1.96, 4.90, 9.81};

float adcToNewtons(int rawAdc) {
    if (rawAdc <= adcVals[0]) return newtonVals[0];
    if (rawAdc >= adcVals[NUM_POINTS-1]) return newtonVals[NUM_POINTS-1];
    
    for (int i = 0; i < NUM_POINTS - 1; i++) {
        if (rawAdc >= adcVals[i] && rawAdc <= adcVals[i+1]) {
            // Linear interpolation between two known points
            return newtonVals[i] + (float)(rawAdc - adcVals[i]) *
                   (newtonVals[i+1] - newtonVals[i]) /
                   (adcVals[i+1] - adcVals[i]);
        }
    }
    return 0.0;
}
```

> [!CAUTION] One Table Per Sensor!
> Because FSRs vary ±6% between units, you need **16 separate calibration tables** for 16 sensors. Label each FSR S01–S16 and store each table separately.

---

## 9. Reading All 16 Sensors via Multiplexer (CD74HC4067)

### Full Spinobar Reading Loop

```cpp
// Pin definitions
#define MUX_SIG  34   // ADC1 pin (safe with BLE/WiFi)
#define MUX_S0   27
#define MUX_S1   26
#define MUX_S2   25
#define MUX_S3   33

// Select MUX channel (0-15) and read FSR
int readSensor(int channel) {
    // Set 4-bit address on S0-S3
    digitalWrite(MUX_S0, (channel >> 0) & 1);
    digitalWrite(MUX_S1, (channel >> 1) & 1);
    digitalWrite(MUX_S2, (channel >> 2) & 1);
    digitalWrite(MUX_S3, (channel >> 3) & 1);
    
    delayMicroseconds(50); // Allow signal to settle
    return analogRead(MUX_SIG);
}

// In loop():
float pressureMap[4][4];  // 4x4 grid
int ch = 0;
for (int row = 0; row < 4; row++) {
    for (int col = 0; col < 4; col++) {
        int raw = readSensor(ch);
        pressureMap[row][col] = adcToNewtons(raw); // per-sensor calibration
        ch++;
    }
}
```

---

## 10. Spinobar-Specific Application Notes

### Physical Mounting Tips
- Always mount FSR on a **hard, smooth backing surface** (rigid plastic or aluminum plate)
- Use a **small rigid disc** (Ø ~12mm) as actuator over each FSR — spreads force evenly
- Never fold or crease the tail/wire area — the sensor will break
- Avoid point loading (sharp objects pressing directly) — use a soft silicone layer on top

### Pressure-Zone Color Mapping for Heatmap

| Force (N) | Pressure (kPa approx.) | Color | Clinical Meaning |
|---|---|---|---|
| 0.0 – 1.5 N | 0 – 6 kPa | 🟢 Green | Safe — comfortable for skin |
| 1.5 – 4.0 N | 6 – 20 kPa | 🟡 Yellow | Therapeutic — monitor |
| 4.0 – 7.0 N | 20 – 35 kPa | 🟠 Orange | High — check patient comfort |
| > 7.0 N | > 35 kPa | 🔴 Red | Danger — pressure sore risk |

> [!NOTE] Pressure to Force Conversion
> Force (N) ÷ Active Area (m²) = Pressure (Pa)
> FSR 402 active area = π × (0.00734)² = 1.69 × 10⁻⁴ m²
> So 1N on FSR 402 ≈ 5,900 Pa ≈ 5.9 kPa

### Wiring Best Practices for Patch

```
✅ Use thin silicone-coated wire (28–30 AWG)
✅ Heat-shrink all solder joints
✅ Label each wire: "S01" through "S16"
✅ Run wires along edges — not over sensors
✅ Use strain relief at connector points
❌ Do not use solid core wire (it will crack under flex)
❌ Do not connect FSR tail directly to breadboard socket repeatedly
```

---

## 11. Limitations & How to Handle Them in Spinobar

| Limitation | Effect | Mitigation |
|---|---|---|
| **Non-linear response** | ADC values ≠ Newtons | Use calibration lookup table + interpolation |
| **±6% part-to-part variation** | Sensors read differently under same force | Calibrate each sensor individually |
| **Hysteresis (10%)** | Loading ≠ unloading readings | Average 3 readings per measurement |
| **Creep under sustained load** | Reading drifts over minutes | Re-zero sensor every 30s in firmware |
| **Temperature sensitivity** | Humidity causes +30% drift | Store baseline offset, update periodically |
| **ESP32 ADC non-linearity** | ADC not perfectly linear 0–4095 | Use `esp_adc_cal` library or offset correction |

---

## 12. Part Numbers for Ordering

| Model | PN (solder tabs) | PN (female contacts) | PN (bare) |
|---|---|---|---|
| FSR 402 | 30-81794 | 34-00012 | 44-29103 |
| FSR 402 Short | 34-00015 | 34-00017 | 34-00016 |
| FSR 406 | 30-73258 | 34-00013 | 34-00009 |

**Recommended for Spinobar:** `PN: 34-00012` (FSR 402 with female contacts — easiest to wire)

---

## 13. Quick Reference Cheat Sheet

```
┌─────────────────────────────────────────────┐
│           FSR 402 QUICK REFERENCE           │
├─────────────────────────────────────────────┤
│ Active Area:    Ø 14.68 mm                  │
│ Thickness:      0.46 mm                     │
│ Force Range:    0.2 N – 20 N                │
│ At Rest:        >10 MΩ                      │
│ At Full Press:  ~200 Ω                      │
│ Response Time:  <3 µs                       │
│ Repeatability:  ±2% (same unit)             │
│                 ±6% (between units)         │
│ Circuit:        Voltage divider, R_M=10kΩ  │
│ ADC Pin:        GPIO 34 (ESP32 ADC1 only)   │
│ Supply Voltage: 3.3V                        │
│ V_out formula:  3.3 × 10k/(10k + R_FSR)    │
└─────────────────────────────────────────────┘
```

---

## 14. External Links & Resources

| Resource | Link |
|---|---|
| Official Datasheet PDF | https://www.interlinkelectronics.com/fsr-402 |
| FSR Integration Guide | https://www.interlinkelectronics.com/integration-guide |
| SparkFun Hookup Guide | https://learn.sparkfun.com/tutorials/force-sensitive-resistor-hookup-guide |
| Adafruit FSR Tutorial | https://learn.adafruit.com/force-sensitive-resistor-fsr |
| DigiKey Application Note | https://www.digikey.com/en/articles/using-force-sensing-resistors |
| Random Nerd Tutorials (ESP32) | https://randomnerdtutorials.com/esp32-with-multiple-analog-sensors/ |

---

*Notes created for Spinobar project — FSR 400 Series Datasheet Review*  
*Date: 2026-06-27*
