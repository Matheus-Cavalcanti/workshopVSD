# Synthesis of `if` and `case` Statements in Verilog

## `if-else` Structures

* **Sequential Evaluation:** An `if-else if` chain is synthesized into hardware that behaves like a priority encoder. The conditions are evaluated sequentially. The first condition that evaluates to true determines the output, and all subsequent conditions in the chain are ignored.
* **Hardware Implication:** This creates a circuit where the first `if` condition has the highest priority. This is a key difference from a `case` statement, which typically synthesizes to a more balanced multiplexer structure without inherent priority.

* **Verilog Example (Priority Logic):**
    ```verilog
    // This code describes a 4-to-1 multiplexer with priority.
    // If 'sel[0]' is high, 'y' will be 'i0', regardless of other 'sel' bits.
    always @(*) begin
      if (sel[0]) begin
        y = i0;
      end
      else if (sel[1]) begin
        y = i1;
      end
      else if (sel[2]) begin
        y = i2;
      end
      else begin
        y = i3; // Default case
      end
    end
    ```

## The Danger of Inferred Latches

* **What is an Inferred Latch?** One of the most common pitfalls when describing combinational logic occurs if an `if` or `case` statement does not assign a value to an output for every possible combination of inputs.
* **Unintended Memory:** If a path through the logic exists where an output is not assigned, the synthesis tool must assume the output should hold its previous value. To achieve this, it infers (creates) a **latch** to store that state.
* **Why is it a Problem?** Unintended latches are dangerous in combinational designs. They are transparent and can lead to timing glitches, race conditions, and make static timing analysis much more difficult. Unless a latch is explicitly desired, inferring one is considered poor coding practice.

* **Verilog Example (Inferred Latch):**
    ```verilog
    // WARNING: This code will infer a latch for 'y'.
    // If neither 'sel[0]' nor 'sel[1]' is high, the code
    // does not specify what 'y' should be. The synthesis
    // tool will add a latch to hold the last value of 'y'.
    always @(*) begin
      if (sel[0]) begin
        y = i0;
      end
      else if (sel[1]) begin
        y = i1;
      end
      // Missing 'else' statement causes the latch.
    end
    ```

* **The Fix (Avoiding the Latch):**
    To prevent the latch, ensure that the output is assigned a value in all possible execution paths. This is typically done by adding a final `else` or by assigning a default value to the output at the beginning of the `always` block.

    ```verilog
    // CORRECTED: No latch will be inferred.
    // A default value is assigned to 'y' in the final 'else' branch,
    // covering all other possibilities.
    always @(*) begin
      if (sel[0]) begin
        y = i0;
      end
      else if (sel[1]) begin
        y = i1;
      end
      else begin
        y = 1'b0; // Assign a default value to cover all cases.
      end
    end
    ```