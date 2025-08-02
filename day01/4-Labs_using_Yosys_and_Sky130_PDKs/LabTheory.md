# How to Use Yosys (Flow and Commands)

The synthesis process in Yosys is executed through an interactive console where specific commands are entered in sequence.

1.  **Start Yosys:** The process begins by initializing Yosys in the terminal.
    * **Command:** `yosys`

2.  **Load the Cell Library (`read_liberty`):** The first step is to inform Yosys what "parts" it can use. This is done by loading the standard cell library (`.lib`), which contains the definition of all available logic gates.
    * **Command:** `read_liberty -lib <path_to_the_file.lib>`

3.  **Load the RTL Design (`read_verilog`):** Next, you load your Verilog design file, which describes the behavior of the circuit you want to synthesize.
    * **Command:** `read_verilog <path_to_the_file.v>`

4.  **Run Synthesis (`synth`):** This is the main command; it executes a standard synthesis script that optimizes the design and maps it to the cells from the loaded library.
    * **Command:** `synth -top <name_of_the_top_module>`
    * The `-top` argument specifies which module in your design is the main one.

5.  **Map to Cells (`abc`):** The `abc` command is a tool integrated into Yosys that performs the final technology mapping, ensuring the design is implemented using only the cells from the specified library.
    * **Command:** `abc -liberty <path_to_the_file.lib>`

6.  **Generate the Netlist (`write_verilog`):** Finally, you export the synthesis result to a new Verilog file, which is the netlist.
    * **Command:** `write_verilog -noattr <output_filename.v>`
    * **`-noattr` Flag:** It is highly recommended to use the `-noattr` flag. Without it, Yosys includes many attributes and metadata in the output file, making it verbose and difficult to read. The `-noattr` flag generates a clean netlist, focusing only on the circuit's structure.