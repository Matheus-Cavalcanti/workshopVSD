# Fundamental Concepts

## Simulator

* **What is it?** A simulator is a software program that mimics the behavior of a digital circuit described in a language like Verilog. Its main function is to verify if the design (the circuit's code) meets the functional specifications.

* **How does it work?** The simulator constantly monitors the input signals of your design. Whenever an input signal changes its value (e.g., from 0 to 1), the simulator recalculates the corresponding outputs based on the logic described in your code. If the inputs do not change, the outputs remain stable.

* **Tool used:** Icarus Verilog (often referred to as iverilog), which is a widely used, open-source Verilog simulator.

## Design (RTL - Register-Transfer Level)

* **What is it?** The "design" is the Verilog code itself that describes the functionality of the circuit you are creating. It can consist of one or more Verilog files that, together, implement the desired logic (e.g., an adder, a processor, etc.).

* **Level of Abstraction:** The code is written at the register-transfer level (RTL), which describes how data moves between storage registers (like flip-flops) and through combinational logic.

## Test Bench

* **What is it?** A "test bench" is a testing environment, also written in Verilog, created specifically to verify your design. It is not part of the final circuit, but it is crucial for simulation.

* **What is its function?** The main function of the test bench is to generate and apply a set of stimuli (called "test vectors") to the inputs of your design. Afterward, it can (optionally) check if the outputs produced by the design match the expected results.

* **Structure:** A test bench instantiates (creates a copy of) the design to be tested within itself. It is important to note that, unlike the design, the test bench itself has no external input or output ports; it is a self-contained simulation environment.

### The Simulation Flow with Icarus Verilog

The practical simulation process follows these steps:

1.  **Input:** You provide two sets of files to the Icarus Verilog simulator:
    * The Design files (your circuit in Verilog).
    * The Test Bench file (the code that will test your design).

2.  **Simulation:** Icarus Verilog compiles and runs the test bench. The test bench, in turn, applies the stimuli to the design.

3.  **Output:** The result of the simulation is a file called a VCD (Value Change Dump). This file is a detailed record of all value changes (0 to 1, 1 to 0, etc.) in all signals (inputs, outputs, and internal signals) of your design throughout the simulation time.

4.  **Visualization and Verification:** To analyze the results, you use a waveform viewer tool, such as GTKWave. By opening the VCD file in GTKWave, you can graphically see the waveforms of all signals, allowing you to visually verify if your design behaved as expected in response to the stimuli from the test bench.