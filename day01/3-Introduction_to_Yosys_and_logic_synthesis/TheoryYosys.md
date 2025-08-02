# Fundamental Concepts

## What is a Synthesizer?

* **Definition:** A synthesizer is a software tool that translates RTL (Register-Transfer Level) code, which describes the behavior of a circuit, into a "netlist".

* **Netlist:** The netlist is a description of the circuit in terms of logic gates and standard cells (like AND, OR, NOT, flip-flops) that are available in a specific technology library.

## Yosys

* **Tool used:** We will use Yosys, a popular open-source synthesizer, to perform this conversion.

### The Synthesis Flow with Yosys

The synthesis process with Yosys involves the following steps:

1.  **Inputs:** Yosys requires two main inputs:
    * **The Design (RTL):** The Verilog code that describes your circuit.
    * **The Standard Cell Library (.lib file):** A file that describes the characteristics (function, timing, etc.) of the basic logic cells that can be used to build the circuit.

2.  **Synthesis Process:** Yosys processes the RTL design and maps it to the cells available in the library.

3.  **Output:** The output of Yosys is a **netlist** file, also in Verilog format. This netlist is functionally equivalent to the original RTL, but it is now described in terms of concrete logic gates from the library.

### Post-Synthesis Verification (Logic Equivalence Checking)

After generating the netlist, it is crucial to verify that it behaves exactly like the original RTL. Since the synthesized netlist maintains the same primary inputs and outputs as the RTL design, the same test bench used to simulate the RTL can be reused to simulate the netlist.

* **Verification Flow:**
    1.  The **netlist** and the **test bench** are provided to the IVerilog simulator (`iverilog`).
    2.  `iverilog` runs the simulation and generates a **VCD (Value Change Dump)** file.
    3.  This VCD file is viewed in **GTKWave**.

* **Success Criterion:** The synthesis is considered correct if the waveforms observed in GTKWave for the netlist simulation are identical to the waveforms from the original RTL simulation, for the same set of stimuli.