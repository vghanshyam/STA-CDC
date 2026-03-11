## 📡 Part 1 — What is STA and Why Do We Need It?

**Static Timing Analysis (STA)** is a technique used in digital design to verify that a circuit meets its **timing requirements** without using dynamic simulation.

STA checks whether signals propagate through the circuit **within the required clock period**, ensuring reliable operation at the target frequency.

---

### 📘 Basic Concept

In synchronous digital systems, data travels from one **flip-flop (source register)** to another **flip-flop (destination register)** through **combinational logic**.
STA (Static Timing Analysis) verifies that:

- **Data arrives on time before the capture edge** → *Setup check*  
- **Data holds long enough after the capture edge** → *Hold check*

⚠ Without STA, a design may work correctly in simulation but fail in real hardware due to timing violations.
## 📡 Part 2 — Setup Time & Hold Time — Fundamentals

### Setup Time (Ts)

Data must be **stable before the clock edge** by at least **Ts**.
### Hold Time (Th)

Data must remain **stable after the clock edge** for at least **Th**.

### ⚠ Violation Effects

- **Setup Violation** → Flip-flop does not have enough time to sample the data → wrong value captured → functional failure

- **Hold Violation** → Data changes too soon after the clock edge → flip-flop may enter a **metastable state** → unpredictable output
## 📡 Part 3 — Setup Slack Calculation — The Math
Setup Slack = Data Required Time - Data Arrival Time

- If `Slack ≥ 0` → **Timing MET** ✅  
- If `Slack < 0` → **Timing VIOLATED** ❌
### Data Arrival Time
Data Arrival Time = Launch Clock Edge + Clock-to-Q Delay + Combinational Logic Delay
                  = T_launch + T_clk2q + T_logic
### Data Required Time
Data Required Time = Capture Clock Edge - Setup Time of FF
                   = T_capture - T_setup
### Therefore
Setup Slack = (T_period − T_setup) − (T_clk2q + T_logic)
### Example

Clock period = 10 ns (100 MHz)  
T_clk2q = 0.5 ns  
T_logic = 7 ns  
T_setup = 0.3 ns  
Setup Slack = 10 − 0.3 − 0.5 − 7
= 2.2 ns

**Result:** 2.2 ns ✅ (Positive Slack → Timing Met)
## 📡 Part 4 — Hold Slack Calculation
Hold Slack = Data Arrival Time − Data Required Time (hold)
-Data Arrival Time = T_launch + T_clk2q + T_logic
-Data Required Time (hold) = T_capture + T_hold
                           = T_launch + T_hold (same clock edge for hold)
Hold Slack = T_clk2q + T_logic − T_hold

- If `Slack ≥ 0` → **Timing MET** ✅  
- If `Slack < 0` → **Timing VIOLATED** ❌
