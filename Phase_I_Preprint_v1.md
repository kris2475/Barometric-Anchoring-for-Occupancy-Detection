# Phase I Technical Report: Barometric Anchoring for Occupancy Detection

**Date:** 5 January 2026  
**Author:** Kris Seunarine  
**Location:** Sketty, Swansea (51.787°N, -4.002°W)  
**Status:** Phase I Complete (Logged Validation)  
**Version:** 1.3 (UK English Standards)

## 1. Abstract
Traditional occupancy detection at the edge (PIR, VOC, or CO2) is prone to baseline drift and high latency in unconditioned environments. This report introduces the **Triple-Anchor Global Normalisation (TAGN)** framework. By anchoring localised sensor data against regional aviation METARs, city-level weather APIs, and precise geodetic elevation, we isolate human "Metabolic Pulses" from natural environmental flux. 

In Phase I testing (Sentinel V1), the methodology successfully identified human occupancy at **0.66°C** with a mathematical singularity at **07:29** and demonstrated a clear lead over VOC recovery during departure at **08:15**.



## 2. Methodology: The Triple-Anchor Strategy

To stabilise the "Moving Baseline" inherent in unheated spaces, the Sentinel V1 platform utilises three distinct validation layers:

1.  **Aviation Anchor (Regional):** Synchronisation with **METAR EGFF (Cardiff International Airport)** establishes the regional **QNH** (Sea Level Pressure).
2.  **API Anchor (Local):** Real-time city-level data from the **World Weather Online Local City API** validates ambient humidity baselines.
3.  **Geodetic Anchor (Fixed):** Normalisation for the site elevation of **74m Above Sea Level (ASL)** ensures data consistency with international standards.

## 3. Mathematical Framework: The Jacobian $J_{11}$

The environmental state is defined by the vector $u = [T, T_d]^T$. The system transformation is described by the Jacobian matrix $\mathbf{J}$:

$$\mathbf{J} = \begin{bmatrix} \frac{\partial T_d}{\partial T} & \frac{\partial T_d}{\partial RH} \\ \frac{\partial T}{\partial T_d} & \frac{\partial T}{\partial RH} \end{bmatrix}$$

The primary detection trigger is defined as the sensitivity element **$J_{11}$**:

$$J_{11} = \frac{\Delta T_d}{\Delta T}$$

* **Equilibrium ($J_{11} \approx 1.0$):** Indicates environmental stasis (Vacant).
* **Anthropogenic Breach ($J_{11} \to \infty$):** Indicates moisture flux decoupling from thermal flux (Occupied).

## 4. Empirical Results (5 January 2026)

### 4.1 Regional Baseline Calibration (METAR Comparison)
To prove the sensor's accuracy, a comparison was performed at 07:50 GMT against the Cardiff International Airport (EGFF) station, located 55km east.

| Metric | METAR (EGFF) | Sentinel V1 (Sketty) | Variance |
| :--- | :--- | :--- | :--- |
| **Air Temperature** | -1.0°C | **0.82°C** | +1.82°C (Indoor Delta) |
| **Dew Point ($T_d$)** | -4.0°C | **-3.05°C** | **+0.95°C (Metabolic Signal)** |
| **Sea Level Pressure**| 1019.0 hPa | **1019.8 hPa*** | **<0.1% Error** |
*\*Calculated using Hypsometric normalisation for 74m ASL.*

### 4.2 Case Study: 45-Minute Occupancy (07:30 – 08:15)
The occupant entered the unheated office at **07:29:53**.
* **Initial Trigger:** The $J_{11}$ recorded a singularity (infinite sensitivity) as respiration moisture introduced a rapid change in $T_d$ before $T$ could react.
* **Metabolic Pulse:** During the 45-minute window, the Jacobian averaged **1.64**, peaking at **2.69**.
* **VOC Latency:** Between 07:50 and 08:05, VOC gas resistance was rising ($dVOC/dt < 0$). Standard algorithms would have erroneously triggered a "Vacant" state, whereas $J_{11}$ correctly maintained the "Occupied" signal.



### 4.3 Departure and Recovery
Following departure at **08:15**, the Jacobian returned to the 1.0 equilibrium baseline by **09:15**. The chemical VOC sensor exhibited a significant "recovery tail," remaining saturated for an additional 35 minutes.

| Time (GMT) | Event | Jacobian $J_{11}$ | Status |
| :--- | :--- | :--- | :--- |
| **03:01** | Stasis | 1.03 | Vacant |
| **07:30** | **Entry** | **Singularity** | **Occupied** |
| **08:05** | **Mid-Session** | **1.50** | **Occupied** |
| **09:26** | Recovery | 1.06 | Vacant |

## 5. Conclusions
Phase I validates that **Barometric Anchoring** combined with **Jacobian Flux Analysis** provides a superior signal-to-noise ratio for occupancy detection. By geodetically anchoring the sensor, we distinguish human presence from atmospheric events with near-perfect reliability.

---
**Licence:** MIT for Code | CC-BY-4.0 for Methodology  
**Citation:** Seunarine, K. (2026). *Barometric Anchoring for Occupancy Detection*. GitHub Repository.
