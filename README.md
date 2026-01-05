# Barometric Anchoring for Occupancy Detection
### Sentinel V1 Technical Repository

This project details a method for detecting human occupancy in unconditioned spaces (sub-zero temperatures) where standard PIR and VOC sensors drift or lag. By geodetically anchoring a BME688 sensor to regional aviation data (METAR) and elevation, we can isolate metabolic pulses using a Jacobian state-vector analysis.



## The Problem
In my unheated office, the baseline environment is never static. Standard Edge-AI models for occupancy detection assume a stable baseline. When the temperature is 0.6°C and falling, chemical sensors (VOCs) become sluggish, and PIR sensors struggle with the thermal contrast.

## The Solution: Triple-Anchor Global Normalisation
Instead of looking for absolute thresholds, I anchor the sensor data against three external "Ground Truths":

1.  **Atmospheric Anchor:** Regional pressure from **Cardiff Airport (EGFF) METARs**.
2.  **Geodetic Anchor:** Elevation normalisation for **74m ASL** (Sketty, Swansea).
3.  **Jacobian $J_{11}$:** A moisture-over-temperature sensitivity calculation that flags respiration before the room even begins to warm up.



## Phase I: Proof of Concept (5 Jan 2026)
Tested in a 45-minute occupancy window (07:30 – 08:15). 
* **Detection:** The Jacobian recorded a singularity (instant trigger) at **07:29**.
* **Reliability:** $J_{11}$ averaged **1.64** during occupancy, while the VOC resistance was actually rising—which would have fooled a standard sensor into thinking the room was empty.
* **Recovery:** The system reset 40 minutes faster than the chemical sensor after departure at 08:15.

## Project Structure
* `/data`: Raw CSV logs from the ESP32/BME688.
* `Phase_I_Preprint_v1.md`: The full methodology, math, and METAR correlation table.
* `LICENSE`: MIT (Code) & CC-BY-4.0 (Math/Method).

## What's Next? (Phase II)
The cardboard housing is too hygroscopic. I’m moving the build into a **non-porous UPVC enclosure** to see if we can sharpen the recovery tail and further refine the Jacobian decay constant.

---
**Citation**
If you use this methodology, please cite:
Seunarine, K. (2026). *Barometric Anchoring for Occupancy Detection*. GitHub: kris2475/Barometric-Anchoring-for-Occupancy-Detection


