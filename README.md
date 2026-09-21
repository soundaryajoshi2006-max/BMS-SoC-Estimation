# Open-Loop Battery Management System (BMS) with State of Charge (SoC) Estimation

A MATLAB/Simulink model demonstrating basic State of Charge (SoC) estimation for a Lithium-ion cell using the **Coulomb Counting** method. The model evaluates tracking performance by comparing an ideal plant cell against an estimation algorithm initialized with an intentional error bias.

## 🚀 Key Features
* **Dual-Track Simulation**: Houses a true physical cell model alongside an estimation block to track performance side-by-side.
* **Coulomb Counting Algorithm**: Uses native mathematical gain and integrator configurations to estimate cell capacity utilization.
* **Dynamic Testing Profile**: Implements an alternating pulse current source to test charge/discharge behavior over time.

---

## 📊 System Architecture Layout

The model splits the input current across two separate, open-loop tracks:

1. **Actual Cell State (Top Track)**: Models the physical battery cell. It integrates incoming current via a dedicated capacity gain block (`2.5 Ah`) to output `True SoC` starting from a full charge baseline (`1.0`).
2. **BMS Algorithm (Bottom Track)**: Runs the estimation software (`BMS Guess`). It implements identical tracking math but initiates with a **10% error bias** (starting at `0.9` instead of `1.0`).
3. **Signal Integration & Visualization**: A `Mux Block` groups both true and estimated state lines together, routing them into a single `Scope: SoC Tracking` display block.

---

## 🛠️ Parameters Blueprint

| Subsystem / Block | Parameter Name | Value / Setting | Description |
| :--- | :--- | :--- | :--- |
| **Pulse Generator** | Pulse Amp <br> Period <br> Duty Cycle | `2A` <br> `10s` <br> `50%` | Inputs alternating current sequences into the cell blocks. |
| **Plant / Cell Model** | Gain Formula <br> Capacity | `-1 / (Capacity * 3600)` <br> `2.5 Ah` | Converts physical current integration into SoC units. |
| | Integrator (True SoC) | Initial Condition: `1.0` | Represents a completely full battery cell (100%). |
| | Lookup Table | OCV Map | Generates Terminal Voltage from the True SoC value. |
| **BMS Algorithm** | Gain Formula <br> Capacity | `-1 / (BMS_Cap * 3600)` <br> `2.5 Ah` | Computes open-loop Coulomb Counting software math. |
| | Integrator (Est. SoC) | Initial Condition: `0.9` | Simulates an incorrect initial software guess (90%). |

---

## 🏃 How to Run the Simulation

1. Open your downloaded `.slx` file in **MATLAB/Simulink**.
2. Make sure the model properties reflect the **18650 Li-ion** battery profile specifications noted in the configuration profile table.
3. Click the green **Run** button on the main toolbar.
4. Double-click the **Scope: SoC Tracking** block.

*Observe how the open-loop estimated line tracks parallel to the actual state line, preserving the original 10% offset bias due to the lack of error feedback correction.*
