# Closed-Loop Battery Management System (BMS) in Simulink

A beginner-friendly **Battery Management System (BMS)** designed in MATLAB/Simulink. This project upgrades traditional open-loop Coulomb Counting into a self-correcting, **Closed-Loop Feedback System** using a classic PI controller.

## 🚀 Key Features
* **Closed-Loop Error Correction**: Automatically corrects tracking errors caused by poor initial state-of-charge (SoC) guesses.
* **Beginner-Friendly Architecture**: Built entirely using native Simulink math blocks, avoiding complex AI or script dependencies.
* **Dual-Plot Tracking Dashboard**: Separates true states and estimated parameters onto distinct, non-overlapping axes for clean visualization.

---

## 📊 System Architecture Layout

The model splits system dynamics across two isolated simulation tracks coupled by a proportional-integral tracking controller:

1. **Actual Cell Plant Model (Top Track)**: Computes the real battery's state, integrating input current to generate `True SoC` and processing it through an Open Circuit Voltage (OCV) lookup table.
2. **BMS Estimation Model (Bottom Track)**: Runs the calculation software. It starts with an intentional **10% error bias** (initial guess of 90% vs. 100% actual capacity).
3. **Feedback Error Loop (Center)**: Continually subtracts Estimated Voltage from True Voltage. The resulting error drives a **PI Controller** to dynamically inject error-correcting adjustments back into the estimator.

---

## 🛠️ Parameters Blueprint

| Subsystem / Block | Parameter | Target Setting | Purpose |
| :--- | :--- | :--- | :--- |
| **Pulse Generator** | Amplitude / Period / Duty | `2.0 A` / `10.0 s` / `50%` | Dynamic test profile current |
| **Plant / Cell Model**| Nominal Capacity | `2.5 Ah` | Cell core capacity rating |
| | Integrator Initial SoC | `1.0 (100%)` | True battery state baseline |
| **BMS Algorithm** | Integrator Initial SoC | `0.9 (90%)` | Simulated sensor error bias |
| **PI Controller** | Proportional Gain (\(K_p\)) | `0.05` | Controls correction response speed |
| | Integral Gain (\(K_i\)) | `0.001` | Wipes out lingering steady-state error |
| **1-D Lookup Table** | Breakpoints (SoC) | `[0, 0.1, 0.25, 0.5, 0.75, 0.9, 1.0]` | Shared battery OCV map points |
| | Table Data (Voltage) | `[3.0, 3.3, 3.6, 3.7, 3.8, 4.0, 4.2]` | Shared battery chemistry voltages |

---

## 🏃 How to Run the Simulation

1. Clone or download this repository and open the `.slx` model file in **MATLAB/Simulink**.
2. Locate the **Stop Time** input field on the top Simulink toolbar and set it to `100`.
3. Click the green **Run** button to execute the simulation.
4. Double-click the **Scope block** to pull open the multi-axis dashboard. 

*Observe how the Estimated SoC curve automatically climbs from its flawed `0.9` startup point to perfectly lock onto the True SoC line over time.*
