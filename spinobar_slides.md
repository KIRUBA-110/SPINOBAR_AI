═══════════════════════════════════════════════════════════════
                    SPINOBAR — Slide Deck
        Copy-paste each slide section into your presentation
═══════════════════════════════════════════════════════════════


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 1 — TITLE SLIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  SPINOBAR

Subtitle:
  A Low-Cost, Real-Time Pressure Monitoring System
  for Spinal Orthoses

Tagline:
  "Replacing guesswork with precision — Made in India"

[Suggested visual: A clean image of a spinal brace with a sensor patch highlighted]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 2 — WHAT IS A SPINAL ORTHOSIS?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  What is a Spinal Orthosis?

Content:
  • A spinal orthosis is a back brace — a stiff supportive device
    worn around the torso to hold the spine in the correct position.

  • Prescribed for:
      → Scoliosis (abnormal sideways curvature of the spine)
      → Post-surgical spinal recovery
      → Back injuries and disc problems

  • The brace applies corrective pressure at specific points
    on the body to gradually realign the spine.

[Suggested visual: Simple diagram of a spine + a back brace on a human torso]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 3 — THE PROBLEM (Part 1)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  The Problem — No Way to Measure Brace Pressure

Content:
  Today, doctors fit braces by hand and guesswork:
    • They tighten the brace and ask: "Does this feel okay?"
    • There is no instrument to measure the actual force applied.

  This leads to two critical failures:

  ┌──────────────────────────────┬─────────────────────────────────┐
  │  TOO MUCH PRESSURE           │  TOO LITTLE PRESSURE            │
  ├──────────────────────────────┼─────────────────────────────────┤
  │  → Painful pressure sores    │  → Zero spinal correction       │
  │  → Patient stops wearing it  │  → Months of treatment wasted   │
  │  → Treatment abandoned       │  → Condition keeps worsening    │
  └──────────────────────────────┴─────────────────────────────────┘

[Suggested visual: Split graphic — red warning for excess pressure, grey/flat for insufficient]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 4 — THE PROBLEM (Part 2)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  Why Can't We Just Buy a Solution?

Content:
  • Devices that measure orthotic pressure DO exist
    (e.g., Tekscan, Novel Pliance — USA/Germany).

  • But they cost ₹5,00,000 to ₹25,00,000 per unit.

  • Most Indian hospitals, clinics, and research labs
    simply cannot afford them.

  • No Indian-made alternative exists today:
      → High import costs and duties
      → No local support or customization
      → No Indian-specific clinical data

[Suggested visual: Price comparison bar chart — imported vs. Spinobar cost]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 5 — OUR SOLUTION — SPINOBAR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  Our Solution — Spinobar

Content:
  Spinobar is a low-cost, indigenous, real-time pressure
  monitoring system for spinal orthoses.

  It answers one critical question:
  ╔══════════════════════════════════════════════════════════╗
  ║  "How much pressure is the brace applying,              ║
  ║   where on the body, and when?"                         ║
  ╚══════════════════════════════════════════════════════════╝

  Key highlights:
    ✓  Made in India
    ✓  Costs only ₹4,000–₹6,000 to build
    ✓  Real-time data on a mobile phone
    ✓  Non-invasive and wearable
    ✓  Works with any standard spinal brace

[Suggested visual: Product overview photo/render of the device]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 6 — HOW IT WORKS (System Architecture)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  How Spinobar Works

Content:
  The system has 4 stages:

  ┌─────────────┐    ┌───────────────┐    ┌────────────┐    ┌─────────────┐
  │  1. SENSOR   │───▶│  2. PROCESSOR  │───▶│ 3. WIRELESS │───▶│  4. MOBILE   │
  │    PATCH     │    │   (ESP32)      │    │ (Bluetooth) │    │    APP       │
  └─────────────┘    └───────────────┘    └────────────┘    └─────────────┘

  1. Sensor Patch — 16 pressure sensors in a 4×4 grid,
     placed inside the brace against the patient's skin.

  2. Microcontroller (ESP32) — Reads all 16 sensors,
     processes the data, prepares it for transmission.

  3. Bluetooth (BLE) — Sends data wirelessly to a phone
     in real time — no wires, no delays.

  4. Mobile App — Displays a live colour-coded heatmap
     on a smartphone or tablet.

[Suggested visual: Flow diagram matching the 4 stages above]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 7 — THE HEATMAP DISPLAY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  Real-Time Heatmap — What the Doctor Sees

Content:
  The mobile app shows a 4×4 colour-coded pressure map:

     ┌──────┬──────┬──────┬──────┐
     │ 🟢   │ 🟢   │ 🟡   │ 🟢   │
     ├──────┼──────┼──────┼──────┤
     │ 🟢   │ 🟡   │ 🔴   │ 🟡   │
     ├──────┼──────┼──────┼──────┤
     │ 🟢   │ 🟢   │ 🟡   │ 🟢   │
     ├──────┼──────┼──────┼──────┤
     │ 🟢   │ 🟢   │ 🟢   │ 🟢   │
     └──────┴──────┴──────┴──────┘

     🟢 Green  = Low pressure (safe, comfortable)
     🟡 Yellow = Medium pressure (monitor)
     🔴 Red    = High pressure (risk of skin injury!)

  → Doctor sees a red zone? Loosen that area of the brace.
  → Correction zone is green? Tighten to apply proper force.
  → Updates live as the patient moves, sits, or walks.

[Suggested visual: Screenshot/mockup of the mobile app heatmap]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 8 — WHO BENEFITS?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  Who Benefits from Spinobar?

Content:

  👤 PATIENTS
     Correct brace fit → no sores → comfortable →
     they actually wear it → spine improves

  👨‍⚕️ DOCTORS & PHYSIOTHERAPISTS
     No more guessing → precise, objective pressure data →
     better clinical decisions

  🧒 CHILDREN WITH SCOLIOSIS
     Most common brace users — wear it 16–23 hours/day —
     correct fit is absolutely critical

  🏥 HOSPITALS & CLINICS
     An affordable device (₹4,000–6,000) they can
     actually purchase and deploy

  🔬 RESEARCHERS
     First-ever Indian dataset of orthotic pressure values →
     enables better brace design for Indian patients

[Suggested visual: Icons for each stakeholder group arranged around the device]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 9 — COST COMPARISON
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  Spinobar vs. Imported Solutions

Content:

  ┌────────────────────────┬──────────────────┬──────────────────┐
  │  Feature               │  Imported Devices │  SPINOBAR        │
  ├────────────────────────┼──────────────────┼──────────────────┤
  │  Cost                  │  ₹5–25 Lakh      │  ₹4,000–6,000   │
  │  Made in India         │  ✗ No             │  ✓ Yes           │
  │  Real-time display     │  ✓ Yes            │  ✓ Yes           │
  │  Wireless              │  Varies           │  ✓ Bluetooth     │
  │  Mobile app            │  ✗ Proprietary    │  ✓ Android/iOS   │
  │  Local support         │  ✗ No             │  ✓ Yes           │
  │  Indian clinical data  │  ✗ No             │  ✓ Yes           │
  │  Affordable for clinics│  ✗ No             │  ✓ Yes           │
  └────────────────────────┴──────────────────┴──────────────────┘

  Spinobar delivers comparable functionality at
  less than 1% of the cost of imported alternatives.

[Suggested visual: Side-by-side comparison or bar chart]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLIDE 10 — SUMMARY & CLOSING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:
  In One Line

Content:

  ╔══════════════════════════════════════════════════════════════╗
  ║                                                              ║
  ║  "Spinobar is a ₹4,000 device that tells doctors —          ║
  ║   in real time — exactly how hard a back brace is            ║
  ║   pushing on a patient's body, so they can adjust            ║
  ║   it perfectly instead of guessing."                         ║
  ║                                                              ║
  ╚══════════════════════════════════════════════════════════════╝

  Key Takeaways:
    ✦  Real problem — doctors have no tool to measure brace pressure
    ✦  Real consequences — skin injuries or failed treatment
    ✦  Real solution — 16-sensor patch + ESP32 + Bluetooth + mobile app
    ✦  Real impact — affordable, indigenous, first of its kind in India


  Thank You
  Questions?

═══════════════════════════════════════════════════════════════
                      END OF SLIDE DECK
═══════════════════════════════════════════════════════════════
