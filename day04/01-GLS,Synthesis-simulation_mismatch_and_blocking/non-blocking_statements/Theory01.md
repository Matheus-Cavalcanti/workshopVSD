# Introduction to Gate-Level Simulation (GLS)

## What is GLS?

* **Post-Synthesis Simulation:** GLS is a type of simulation that is performed **after** the synthesis process, which is when the high-level code (RTL) is translated into an actual hardware representation, called a **Netlist**.
* **Change in the DUT (Design Under Test):** Unlike RTL simulation, where the DUT is the Verilog/VHDL code itself, in GLS, the DUT is the **Netlist**. The Netlist is a list of logic gates (like AND, OR, NOT) and the connections between them.
* **Testbench Compatibility:** Since the Netlist is logically equivalent to the original RTL code (it has the same inputs and outputs), the same testbench used for RTL simulation can be reused for GLS.

## Why is GLS Necessary?

GLS is a critical step in the design flow for two main reasons:

1. **Verification of Logical Correctness:** The primary goal is to verify that the synthesis tool has correctly translated the intent of the RTL code into a circuit of logic gates. It is a safeguard against potential bugs or misinterpretations by the tool.
2. **Timing Validation (with Delay Annotation):** Although not the main focus of this specific lecture, GLS is also used to verify that the circuit meets its timing requirements. To do this, the models of the logic gates need to contain delay information (delay annotation), allowing for a much more accurate simulation of the hardware's temporal behavior.

## How GLS Works (Example with Icarus Verilog)

The simulation flow is similar to RTL simulation, but with one important addition:

1. **Inputs:** The simulator (like Icarus Verilog) takes the **Netlist** (the design) and the **Testbench** as inputs.
2. **Gate Level Models:** It is crucial to provide the simulator with the **Verilog models of the logic gates (Gate Level Models)**. These models are files that describe the functional behavior of each standard cell used in the Netlist (for example, what a cell named "AND2_X1" actually does). Without them, the simulator would not know how to interpret the Netlist.
3. **Output:** The simulator generates an output file (like a VCD - Value Change Dump) that can be viewed in a waveform viewer (like GTKWave) to analyze the circuit's behavior.

## The Problem of Synthesis-Simulation Mismatch

The question "why do we need to verify the Netlist if it's supposed to be a faithful representation of the RTL?" is answered by the risk of **synthesis-simulation mismatches**. Certain code constructs in Verilog can be interpreted one way by the simulator and another way by the synthesis tool, leading to different behavior between what was simulated and what the actual hardware will be. GLS is the primary way to detect these issues.
