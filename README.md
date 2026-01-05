# Barometric Anchoring for Occupancy Detection

**A Thermodynamic Jacobian Approach to Mitigating Anthropogenic Noise in Edge-AI Environmental Sensors**

[![Licence: MIT](https://img.shields.io/badge/Licence-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Licence: CC BY 4.0](https://img.shields.io/badge/Licence-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

## 🚀 Project Overview
Traditional occupancy sensors (PIR, CO2, or Gas/VOC) often fail in unconditioned, high-stress environments due to baseline drift and sensor latency. This project introduces **Barometric Anchoring**—a methodology that treats the environment as a thermodynamic system rather than a set of static thresholds. 

By applying a **Jacobian state-vector analysis**, we can isolate the "Metabolic Pulse" of a human occupant (respiration and latent heat) from natural environmental fluctuations.

## 🧠 Core Innovation: The Triple-Anchor Framework
The Sentinel V1 platform achieves high-fidelity detection by anchoring its state against three global constants:

1.  **Aviation Anchor:** Real-time synchronisation with regional **METAR data (EGFF - Cardiff International)** to cancel out atmospheric pressure fronts.
2.  **Geodetic Anchor:** Physical normalisation for site-specific elevation (**74m ASL**) at **51.787°N, -4.002°W**, ensuring <0.1% pressure variance against aviation standards.
3.  **Jacobian $J_{11}$ Analysis:** A mathematical framework that decouples moisture flux (Dew Point) from thermal flux (Temperature) to detect presence with zero-latency.



## 📊 Phase I Validation: The 45-Minute Pulse
Phase I (Completed January 2026) utilised strictly logged data to prove the $J_{11}$ methodology in sub-zero conditions ($0.66^\circ\text{C}$). 

* **Zero-Latency Trigger:** Captured a mathematical singularity ($J_{11} \to \infty$) at the exact moment of entry (**07:29**).
* **Chemical Override:** Successfully maintained an "Occupied" state during periods where VOC gas sensors reported false vacancies due to rising resistance.
* **Departure Recovery:** Identified departure at **08:15** and reset to equilibrium 40 minutes faster than traditional chemical sensing.

> **Read the full technical breakdown:** [Phase I Technical Report](./Phase_I_Preprint_v1.md)

## 📂 Repository Structure
* `/data`: Raw CSV logs from the Sentinel V1 (BME688 / ESP32).
* `/scripts`: Python implementations of $J_{11}$ calculations and Hypsometric normalisation.
* `Phase_I_Preprint_v1.md`: Detailed scientific methodology and METAR correlation data.

## 📈 Phase II: The UPVC Evolution
We are currently moving from hygroscopic cardboard housing to **non-porous UPVC enclosures**. This phase aims to isolate the sensor's recovery latency from material-based moisture retention, further refining the $J_{11}$ decay constant.

## 📜 Citation & Intellectual Property
The **Barometric Anchoring** methodology and the **$J_{11}$ Jacobian Framework** presented here are the intellectual property of **Kris Seunarine**.

If you utilise this methodology in research or commercial applications, please cite as follows:

> Seunarine, K. (2026). *Barometric Anchoring for Occupancy Detection: A Thermodynamic Jacobian Approach to Mitigating Anthropogenic Noise in Edge-AI Environmental Sensors*. Phase I Technical Report. https://github.com/kris2475/Barometric-Anchoring-for-Occupancy-Detection

## ⚖️ Licence
* **Software/Code:** [MIT Licence](./LICENSE)
* **Methodology/Documentation:** [Creative Commons Attribution 4.0 International (CC-BY-4.0)](https://creativecommons.org/licenses/by/4.0/)


