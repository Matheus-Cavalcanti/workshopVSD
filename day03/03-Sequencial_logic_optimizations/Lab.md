# Labs

## Lab 08

In this lab, I only did the exercises indicated by the instructor.

1. dff_const4.v waveform in gtkwave:

![alt text](../../Prints/Day03/D3-L8-P1.png)

   In the simulation, the output 'q' is constant independent of 'clk' and 'reset'.
   We should see only a wire connecting 'q' to a 1 bit (high) in the netlist.
   dff_const4.v netlist obtained:

![alt text](../../Prints/Day03/D3-L8-P2.png)

2. dff_const5.v waveform in gtkwave:

![alt text](../../Prints/Day03/D3-L8-P3.png)

   In the simulation, the output 'q' is NOT constant, therefore, there should not be many optimizations to be made in the synthesis
   dff_const5.v netlist obtained:

![alt text](../../Prints/Day03/D3-L8-P4.png)
