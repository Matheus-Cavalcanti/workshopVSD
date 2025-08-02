# Synthesis of `case` Statements and Sequential Logic

## Intentional "Memory" in Sequential Circuits

* **Counters and State Retention:** In sequential logic, like a counter, the goal is often to hold or "latch" the current value until the next clock edge. This is expected behavior.
* **Incomplete `if` is Acceptable Here:** When coding a counter, an `if (enable)` statement without a corresponding `else` is perfectly acceptable.
    * **Intended Behavior:** If the `enable` signal is low, the counter should retain its current value. The synthesis tool correctly infers that this "memory" function should be implemented using the existing flip-flops of the counter. This is not a bug; it's the desired functionality.
    * **Contrast with Combinational Logic:** This is the opposite of a combinational circuit, where inferring a latch from an incomplete `if` statement is considered a design flaw because combinational logic is not supposed to have memory.

* **Verilog Example (Counter):**
    ```verilog
    // In this sequential block, the missing 'else' is intentional.
    // It tells the synthesizer to hold the value of 'count'
    // when 'enable' is not asserted.
    always @(posedge clk, posedge rst) begin
      if (rst) begin
        count <= 3'b000;
      end
      else if (enable) begin
        count <= count + 1;
      end
      // No 'else' needed here. The flip-flop naturally holds its state.
    end
    ```

## The `case` Statement

* **Purpose:** A `case` statement is used to check if an expression matches one of several possible values. It's often a cleaner and more readable alternative to a long `if-else if` chain, especially for implementing multiplexers or state machines.
* **Synthesis:** A `case` statement typically synthesizes to a balanced multiplexer, where all inputs are treated with equal priority.
* **Requirement:** Like `if` statements, `case` statements are used inside `always` blocks, and the variable being assigned to must be declared as a `reg`.

## The Danger: Inferred Latches in `case` Statements

* **Incomplete Specification:** The same danger of inferring latches exists with `case` statements. If you do not specify an output for every possible value of the case expression, the synthesis tool will infer a latch to hold the previous value for the unspecified conditions.

* **Verilog Example (Inferred Latch with `case`):**
    ```verilog
    // WARNING: This code will infer a latch for 'y'.
    // The case statement only covers sel values 0, 1, and 2.
    // It does not specify what to do for sel = 3.
    // A latch will be created to hold 'y' when sel is 3.
    always @(*) begin
      case (sel) // sel is a 2-bit signal
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        // Missing case for 2'b11
      endcase
    end
    ```

* **The Solution: The `default` Case**
    * To prevent inferred latches, always include a `default` case. The `default` statement covers all possible values of the case expression that were not explicitly listed. This ensures the logic is fully specified and purely combinational.

* **Verilog Example (Corrected `case` statement):**
    ```verilog
    // CORRECTED: No latch will be inferred.
    // The 'default' case handles all unspecified values of 'sel'.
    always @(*) begin
      case (sel) // sel is a 2-bit signal
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        default: y = 1'b0; // Assign a default value.
      endcase
    end
    ```