# ±5 V Sensor to 3.3 V MCU ADC Interface (Passive, Protected)

## Overview
This project provides a **robust, passive analog front-end** for interfacing a **−5 V to +5 V single-ended sensor output** (e.g. eddy-current or gap sensor) with a **3.3 V microcontroller ADC** (such as STM32).

The circuit safely maps the sensor signal into the ADC input range while protecting the MCU against **over-voltage, under-voltage, transients, wiring faults, and ESD-like events**, without using active components (op-amps).

---

## Key Features
- **Input Range:** −5 V to +5 V (single-ended, referenced to GND)
- **Output Range:** Safe for 0–3.3 V MCU ADC input
- **Passive Design:** Resistors, Schottky diodes, capacitor only
- **Over-/Under-Voltage Protection:** External Schottky clamp diodes
- **Current Limiting:** Series resistor to protect MCU pins
- **Fast Response:** Microsecond-scale electrical response
- **Noise Reduction:** Optional RC filtering without ms-level delay
- **Fail-Safe:** Protects MCU even during abnormal conditions

---

## Typical Use Cases
- Eddy-current displacement / gap sensors (±5 V output)
- Industrial or automotive test benches
- Engagement / disengagement detection systems
- Safety-critical ADC measurements
- MCU protection during sensor faults or miswiring

---

## Circuit Description

### 1. Voltage Mapping
A resistor network creates a **DC offset and scaling**, converting the ±5 V sensor signal into a mid-range voltage suitable for a 3.3 V ADC.

Normal operation keeps the ADC pin well within the valid input range, so the protection diodes remain **inactive**.

### 2. Protection Strategy
- **Series Resistor (R4):**
  - Limits current into the ADC pin and clamp diodes
  - Prevents excessive injection current during faults
- **Schottky Clamp Diodes:**
  - Upper clamp: limits ADC pin to ~3.3 V + Vf (~3.6 V)
  - Lower clamp: limits ADC pin to ~−0.3 V
  - External clamps conduct before internal MCU diodes
- **Optional Capacitor (C1):**
  - Filters high-frequency noise
  - Does not affect ms-level response

### 3. Fault Behavior
During abnormal input conditions (e.g. > ±5 V, ESD, cable issues):
- Voltage is clamped safely
- Current is limited to a few mA
- MCU pin remains within absolute maximum ratings

---

## Electrical Characteristics (Typical)

| Parameter | Value |
|---------|------|
| Sensor Input Range | −5 V to +5 V |
| ADC Safe Range | ~0.8 V to ~2.4 V (normal operation) |
| Clamp Voltage (High) | ~3.5–3.6 V |
| Clamp Voltage (Low) | ~−0.2 to −0.3 V |
| RC Time Constant | ~2–3 µs (with 2.2 kΩ + 1 nF) |
| Suitable ADC Sampling | 5–10 kHz (or higher) |

---

## Design Notes
- Grounds **must be common** between sensor and MCU
- Clamp diodes should be placed **close to the MCU ADC pin**
- Add local **0.1 µF decoupling** near the clamp connection to 3.3 V
- For harsh environments, a small series resistor to the clamp rail can be added
- This circuit prioritizes **robustness and safety** over precision amplification

---

## Limitations
- Passive scaling: not suitable if high accuracy or gain adjustment is required
- ADC reference noise directly affects measurement accuracy
- Not a differential input solution

For higher precision or isolation, consider an op-amp buffer or isolated ADC front-end.

---

## File Structure (Suggested)
