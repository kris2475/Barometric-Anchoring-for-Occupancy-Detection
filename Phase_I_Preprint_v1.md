# Phase I Technical Report: Barometric Anchoring for Occupancy Detection

**Date:** 5 January 2026  
**Author:** Kris Seunarine  
**Status:** Phase I Complete (Empirical Validation)  
**Version:** 1.0

## 1. Abstract
Traditional occupancy detection at the edge (PIR, VOC, or CO2) is prone to baseline drift and high latency in unconditioned environments. This report introduces the **Triple-Anchor Jacobian Framework**. By anchoring localised sensor data against barometric pressure, geographic elevation, and thermodynamic dew point flux, we can isolate human "Metabolic Pulses" from natural environmental drift. 

In Phase I testing (Sentinel V1), the methodology successfully identified human occupancy at **0.66°C** with a mathematical singularity at the moment of entry and demonstrated a 40-minute lead over VOC recovery during departure.

## 2. Methodology: The Triple-Anchor Strategy

To stabilise the "Moving Baseline" inherent in unheated spaces, the Sentinel V1 platform utilises three distinct anchors:

1.  **Primary Anchor (Atmospheric):** Local station pressure is synchronised with regional **METAR** reports and the **World Weather Online API** to filter out regional weather fronts.
2.  **Secondary Anchor (Geographic):** Using GPS coordinates (**51.787°N, 4.002°W**), readings are normalised for **74m ASL** elevation using the Hypsometric formula.
3.  **Tertiary Anchor (Thermodynamic):** Dew Point ($T_d$) is utilised as the metabolic constant to decouple human moisture output from temperature-dependent Relative Humidity fluctuations.



## 3. Mathematical Framework: The Jacobian $J_{11}$

The environmental state is defined by the vector $u = [T, T_d]^T$. The system transformation is described by the Jacobian matrix $\mathbf{J}$:

$$\mathbf{J} = \begin{bmatrix} \frac{\partial T_d}{\partial T} & \frac{\partial T_d}{\partial RH} \\ \frac{\partial T}{\partial T_d} & \frac{\partial T}{\partial RH} \end{bmatrix}$$

The primary detection trigger is defined as the sensitivity element **$J_{11}$**:

$$J_{11} = \frac{\Delta T_d}{\Delta T}$$

* **Equilibrium ($J_{11} \approx 1.0$):** Indicates environmental stasis (vacant).
* **Anthropogenic Breach ($J_{11} > 1.2$ or $\infty$):** Indicates moisture flux decoupling from thermal flux (occupied).

## 4. Phase I Empirical Results

### 4.1 Nocturnal Baseline Stability
Data collected between 00:00 and 06:00 (5 Jan) showed a stable Barometric Anchor of **1010.25 ± 1.27 hPa**. The $J_{11}$ averaged **1.037**, proving that the "Anchor" successfully prevents false positives during natural cooling cycles.

### 4.2 Metabolic Pulse Analysis (Human Entry)
At **07:29**, the occupant entered the space ($0.66^\circ\text{C}$). 
* **Immediate Detection:** The Jacobian recorded a singularity (infinite sensitivity) as moisture rose before thermal change.
* **Verification:** Manual checks at 07:53 and 08:08 confirmed sustained $J_{11}$ values of **1.67** and **1.44**.
* **VOC Comparison:** During the same period, the VOC sensor resistance was rising ($dVOC/dt < 0$), which would signal "Vacant" in traditional systems. The $J_{11}$ correctly identified the human presence.

### 4.3 Recovery and Latency
The Jacobian returned to the 1.0 baseline by **09:26**, approximately 40 minutes faster than the chemical VOC sensor, which suffered from significant desorption hysteresis ("lag").

| Metric | Entry (07:29) | Mid-Occupancy | Recovery (09:26) |
| :--- | :--- | :--- | :--- |
| **Jacobian $J_{11}$** | **Singularity** | **1.67 (High)** | **1.05 (Reset)** |
| **VOC Resistance** | No Change | Rising (False Vacant) | Lagging Recovery |



## 5. Conclusions
Phase I validates that **Barometric Anchoring** provides a superior signal-to-noise ratio for occupancy detection in high-stress, low-temperature environments. The Jacobian $J_{11}$ effectively "sees" through thermal drift that blinds traditional sensors.

## 6. Phase II Outlook
The next phase will involve transitioning from the cardboard prototype to a **non-porous UPVC enclosure** to test the impact of material hygroscopy on $J_{11}$ recovery rates.

---
**Licence:** MIT for Code | CC-BY-4.0 for Methodology  
**Citation:** Seunarine, K. (2026). *Barometric Anchoring for Occupancy Detection*. GitHub Repository.
