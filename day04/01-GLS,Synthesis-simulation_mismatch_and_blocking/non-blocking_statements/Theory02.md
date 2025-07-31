# Synthesis-Simulation Mismatch: The Missing Sensitivity List

* **What is it?** A "synthesis-simulation mismatch" occurs when the Verilog code behaves one way in a simulator but results in a different hardware circuit after synthesis. This is a dangerous issue as it means the pre-synthesis verification is not a true representation of the final hardware.
* **Key Causes:** Three common causes for this mismatch are:
    1. A missing or incomplete sensitivity list in an `always` block.
    2. Incorrect use of blocking (`=`) vs. non-blocking (`<=`) assignments.
    3. Using non-standard or poorly written Verilog code.

## Incomplete Sensitivity List

* **How Simulators Work:** A simulator is event-driven. It only re-evaluates a block of code (like an `always` block) when there is "activity" on one of the signals specified in its **sensitivity list**.
* **Example: A Faulty Multiplexer:**
    * Consider a 2-to-1 multiplexer described with an `always` block. If the sensitivity list is written as `always @(sel)`, the block will *only* be triggered when the `sel` (select) signal changes.
    * **Simulation Behavior:** If `sel` is stable and one of the data inputs (`i0` or `i1`) changes, the simulator will **not** re-evaluate the block. The output `y` will not update, retaining its previous value. This makes the circuit behave like a **latch** in the simulation.
    * **Synthesis Behavior:** The synthesis tool, however, is smarter. It analyzes the logic inside the `always` block (`y = sel ? i1 : i0;`) and understands that it describes a multiplexer. It will build a proper multiplexer circuit where the output `y` is immediately affected by changes in `i0` and `i1` (as well as `sel`).
    * **The Mismatch:** The simulation shows a latch, but the hardware is a multiplexer. This is a critical functional error.

## Solution

* **The Correct Approach:** For describing combinational logic, the sensitivity list must include **all** signals that are read inside the `always` block.
* **The Best Practice (`always @(*)`):** The modern and recommended way to ensure this is to use `always @(*)`. This special syntax automatically makes the `always` block sensitive to any and all signals that are read within it. This guarantees that the simulation behavior will correctly model the intended combinational hardware, eliminating the risk of this type of mismatch.
