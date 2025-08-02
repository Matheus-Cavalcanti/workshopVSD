### Advanced `case` Statement Caveats

#### The Danger of Partial Assignments

* **The Problem:** When you have multiple outputs being controlled by a single `case` statement, you must assign a value to **every output** in **every single case branch**. If you fail to assign a value to an output in any one branch, the synthesis tool will infer a latch for that specific output to hold its previous value.

* **Verilog Example (Partial Assignment):**
    ```verilog
    // WARNING: This code will infer a latch for 'y'.
    reg [1:0] sel;
    reg x, y;

    always @(*) begin
        case (sel)
            2'b00: begin
                x = a;
                y = b;
            end
            2'b01: begin
                x = c;
                // 'y' is not assigned a value in this branch.
                // A latch will be inferred for 'y' to hold its
                // state when sel is 2'b01.
            end
            default: begin
                x = d;
                y = b;
            end
        endcase
    end
    ```
* **The Solution:** The most robust solution is to assign a default value to all outputs at the beginning of the `always` block. This ensures that even if you forget an assignment in a specific branch, the outputs will have a defined value, preventing a latch.

    ```verilog
    // CORRECTED: No latch will be inferred.
    always @(*) begin
        // Assign default values first
        x = 1'b0;
        y = 1'b0;

        case (sel)
            2'b00: begin
                x = a;
                y = b;
            end
            2'b01: begin
                x = c;
                // 'y' will now correctly take its default value of 0.
            end
            default: begin
                x = d;
                y = b;
            end
        endcase
    end
    ```

#### The Danger of Overlapping `case` Items

* **`if-else if` vs. `case`:** An `if-else if` structure has inherent priority; only the first true condition is executed. A `case` statement, however, is meant to be parallel. The synthesis tool evaluates it as if all items are checked simultaneously.

* **The Problem:** If you write a `case` statement with overlapping conditions (e.g., using don't-cares like `?`), it creates ambiguity. If an input matches multiple case items, the synthesis tool doesn't know which branch to choose, leading to unpredictable hardware. This is a critical design flaw.

* **Verilog Example (Overlapping Conditions):**
    ```verilog
    // WARNING: This code has overlapping case items.
    case (sel) // sel is 2 bits
        2'b10:  y = a;
        2'b1?:  y = b; // This overlaps with 2'b10 and 2'b11
    endcase
    ```
    * **Ambiguity:** If `sel` is `2'b10`, it matches both `2'b10` and `2'b1?`. The simulator might pick one based on its internal rules, but the synthesis result is not guaranteed. The tool will flag this as a major issue because it cannot build deterministic hardware from ambiguous logic.

* **The Solution:** Ensure that all items in a `case` statement are mutually exclusive. Never write conditions that can be true at the same time. If you need priority logic, you must use an `if-else if` structure.