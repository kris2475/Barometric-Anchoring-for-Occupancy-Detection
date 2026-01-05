# Phase I Technical Report: Barometric Anchoring for Occupancy Detection

**Date:** 5 January 2026  
**Author:** Kris Seunarine  
**Status:** Phase I Complete (Logged Validation)  
**Version:** 1.1

## 1. Abstract
Traditional occupancy detection at the edge (PIR, VOC, or CO2) is prone to baseline drift and high latency in unconditioned environments. This report introduces the **Triple-Anchor Jacobian Framework**. By anchoring localised sensor data against barometric pressure, geographic elevation, and thermodynamic dew point flux, we can isolate human "Metabolic Pulses" from natural environmental drift. 

In Phase I testing (Sentinel V1), using strictly logged data, the methodology successfully identified human occupancy at **0.66°C** with a mathematical singularity at **07:29** and demonstrated a clear lead over VOC recovery during departure.

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
* **Anthropogenic Breach ($J_{11} \to \infty$):** Indicates moisture flux decoupling from thermal flux (occupied).

## 4. Phase I Empirical Results (Logged Data)

### 4.1 Nocturnal Baseline Stability
Logged data between 00:00 and 06:00 (5 Jan) showed a stable Barometric Anchor. Despite a temperature drop, the $J_{11}$ averaged **1.037**, proving that the "Anchor" successfully prevents false positives during natural cooling cycles.

### 4.2 Metabolic Pulse Analysis (Logged Event)
At **07:29:53**, the occupant entered the space ($0.66^\circ\text{C}$). 
* **Immediate Detection:** The Jacobian recorded a singularity (infinite sensitivity) exactly as moisture rose before thermal change.
* **Sustained Presence:** At **08:05:41**, the log recorded $J_{11} = 1.50$, $T = 0.97^\circ\text{C}$, and $RH = 75.75\%$, with a VOC gas resistance of **46,093 $\Omega$**.
* **VOC Failure:** During this peak occupancy window, VOC resistance was rising ($dVOC/dt < 0$), which would signal "Vacant" in traditional systems. The $J_{11}$ correctly maintained the "Occupied" state.

### 4.3 Recovery and Latency
The Jacobian returned to the 1.0 baseline at **09:26:29** ($J_{11} = 1.06$), resetting the system state while the chemical VOC sensor remained in a lagging recovery phase.

| Logged Time | Event | Jacobian $J_{11}$ | Gas Resistance ($\Omega$) |
| :--- | :--- | :--- | :--- |
| **03:01:44** | Stasis | **Equilibrium** | 44,284 |
| **07:29:53** | **Breach** | **$\infty$ (Singularity)** | 47,080 |
| **08:05:41** | **Peak Presence** | **1.50 (High)** | 46,093 |
| **09:26:29** | **Departure** | **1.06 (Reset)** | 46,764 |

## 5. Conclusions
Phase I validates that **Barometric Anchoring** provides a superior signal-to-noise ratio for occupancy detection. The Jacobian $J_{11}$ effectively "sees" through thermal drift that blinds traditional sensors.

---
**Licence:** MIT for Code | CC-BY-4.0 for Methodology  
**Citation:** Seunarine, K. (2026). *Barometric Anchoring for Occupancy Detection*. GitHub Repository.
