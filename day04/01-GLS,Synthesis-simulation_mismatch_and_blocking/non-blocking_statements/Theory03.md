# Caveats of Blocking (`=`) vs. Non-Blocking (`<=`) Assignments

## Blocking (`=`) Assignment Behavior

* **Sequential Execution:** Blocking assignments are executed in the order they appear in the code, much like in a standard programming language like C. The result of one assignment is immediately available to the next line within the same simulation time step.
* **Simulation Example (Shift Register):** If you try to create a two-stage shift register (`q1 = d; q2 = q1;`) using blocking assignments, the simulation will execute `q1 = d` first. Then, for the second line, it will use the **new** value of `q1` and assign it to `q2`. The result is that both `q1` and `q2` get the value of `d` in the same clock cycle, which is not the behavior of a shift register.

## Non-Blocking (`<=`) Assignment Behavior

* **Parallel Execution:** Non-blocking assignments are designed to model the parallel nature of hardware. All the right-hand side expressions are evaluated first, using the values the signals had at the **beginning** of the time step. Only after all evaluations are complete are the left-hand side signals updated.
* **Simulation Example (Shift Register):** If you write the same logic with non-blocking assignments (`q1 <= d; q2 <= q1;`), the simulator first evaluates the right-hand sides. It sees that `d` should go to `q1` and the **old** value of `q1` should go to `q2`. Then, it performs both updates. This correctly models the behavior of a two-flop shift register, where data moves one stage per clock cycle.

## Why the Mismatch Occurs

The simulator strictly follows the sequential vs. parallel rules. The synthesis tool, however, analyzes the data dependencies to infer the hardware structure.. When it sees code for a shift register, it will likely build a proper shift register regardless of whether blocking or non-blocking assignments were used. The danger is that your simulation (using blocking assignments) will show incorrect behavior (e.g., not a shift register), while the synthesis tool creates the correct hardware. This means your pre-synthesis verification is invalid and cannot be trusted.

## The Solution and Best Practices

To avoid this mismatch, always use **non-blocking assignments (`<=`)** when modeling sequential logic (any logic that uses a clock, like flip-flops and registers). Because this mismatch is a potential risk, it is essential to run a **Gate-Level Simulation (GLS)** on the final netlist to confirm that the synthesized hardware behaves exactly as intended and matches the expected functionality.
