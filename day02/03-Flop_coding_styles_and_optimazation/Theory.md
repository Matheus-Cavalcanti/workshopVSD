# Flip-Flops in Digital Circuits

## Glitches in Combinational Circuits

* **What are glitches?** In purely combinational logic circuits, changes at the inputs do not propagate instantaneously. Due to different delays in the logic gates, the circuit can produce temporarily incorrect outputs, known as "glitches," before settling to the correct final value.
* **Cascading effect:** If multiple combinational circuits are connected in sequence (cascaded), these glitches can propagate from one stage to the next. This creates a cycle of instability where the overall circuit output never settles to a stable and reliable value.

## The Role of Flip-Flops

* **Storage and Synchronization:** Flip-flops are memory storage elements that solve the problem of glitches. Their main characteristic is that they only update their output value at a specific moment, determined by the edge of a clock signal (either rising or falling).
* **"Shielding" against Instability:** By placing flip-flops between stages of combinational logic, they act as a barrier or "shield." Even if the input to a flip-flop is unstable and full of glitches, its output will remain stable. It captures the input value only at the clock instant, providing a clean and stable signal to the next block of combinational logic. This ensures that each stage has enough time to stabilize.

## Initialization and Control of Flip-Flops

* **The Importance of Initialization:** For a circuit to function correctly from the start, flip-flops need to begin in a known and defined state. An uninitialized flip-flop will have an undefined (garbage) output value, which can lead to unpredictable behavior throughout the entire circuit.
* **Control Mechanisms:** Initialization is performed using special control pins, such as `reset` (to force the output to 0) or `set` (to force the output to 1).
* **Types of Control:** These controls can be of two types:
    * **Synchronous:** The reset or set action only occurs on the clock edge, in sync with the rest of the circuit.
    * **Asynchronous:** The action is immediate and independent of the clock signal.
