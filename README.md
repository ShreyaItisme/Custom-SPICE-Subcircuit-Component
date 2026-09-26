# Custom-SPICE-Subcircuit-Component
## LTspice Custom Diode Clamp Subcircuit (.SUBCKT)
This repository contains a custom SPICE subcircuit macro-model (my_diode_clamp.sub) designed for voltage clamping and rectification applications in [LTspice](https://www.analog.com/en/resources/design-tools-and-calculators/ltspice-simulator.html). It includes both the subcircuit definition file and an example test bench implementation showing how to deploy it across a voltage source.
------------------------------
## 📄 Subcircuit Code Analysis
The netlist model is defined within the my_diode_clamp.sub file as follows:

* File: my_diode_clamp.sub
.SUBCKT my_diode_clamp IN OUT GND
D1 IN OUT DMOD
R1 OUT GND 10k
.MODEL DMOD D(Is=1e-14 Rs=0.5)
.ENDS my_diode_clamp

## Component Details

* Pin Configuration: The macro block exposes three nodes to your main schematic: IN (Input signal), OUT (Clamped output signal), and GND (Reference ground).
* Diode ($D_1$): Formed between the IN and OUT nodes utilizing a custom diode model (DMOD).
* Diode Model (DMOD): Configured as a silicon diode with a saturation current ($I_s$) of 1e-14 A and a low parasitic series resistance ($R_s$) of 0.5 Ω for highly realistic switching transitions.
* Bleeder Resistor ($R_1$): Integrated directly into the output terminal (10 kΩ tied to GND) to provide a discharge path and ensure predictable node initialization during simulation.

------------------------------
## 🔌 Implementation & Test Bench Setup
To implement this block across a raw AC voltage source within your main LTspice schematic (.asc), follow this standard test bench layout:
## 1. Linking the Subcircuit File
You must tell LTspice where to look for your subcircuit file. Place a SPICE directive directly on your schematic workspace:

.include my_diode_clamp.sub

(Make sure the .sub file rests inside the exact same folder path as your main .asc schematic file).
## 2. Wiring the Test Bench

* Voltage Source ($V_1$): Add an independent voltage source component. Configure it to output a Sine wave (e.g., SINE(0 5 1k) for a 5V peak, 1 kHz signal). Connect its positive terminal to node IN and its negative terminal to common GND.
* Subcircuit Instance Block: Instantiate the macro block. In text netlists, this appears as an X component:

X1 IN OUT 0 my_diode_clamp

(If using a visual block component symbol, map your symbol pins to match the node order: IN, OUT, GND respectively).

------------------------------
## 🚀 How to Simulate

   1. Clone this repository to your computer.
   2. Ensure both the .asc test schematic and my_diode_clamp.sub are saved together in the same directory.
   3. Open the schematic in LTspice and run your transient simulation command (e.g., .tran 5m to capture a few full cycles at 1 kHz).
   4. Probe the input node (IN) to observe your raw AC reference signal, then click the output node (OUT) to view the altered waveform behavior through the subcircuit.


