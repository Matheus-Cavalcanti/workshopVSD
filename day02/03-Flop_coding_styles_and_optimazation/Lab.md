# Labs

# Lab 06

1. Simulate dff_async_res with iverilog and view the waveform with gtkwave:

![alt text](../../Prints/Day02/D2-L6-P1.png)

2. Simulate dff_async_set:

![alt text](../../Prints/Day02/D2-L6-P2.png)

3. Simulate dff sync res:

![alt text](../../Prints/Day02/D2-L6-P3.png)

4. Synthesize dff_async_res with yosys. note: Run `dfflibmap -liberty <lib-path>` after `synth` to map the DFF cells to sequential cells of the lib (run `abc` normally after that).

![alt text](../../Prints/Day02/D2-L6-P4.png)

5. Synthesize dff_async_set:

![alt text](../../Prints/Day02/D2-L6-P5.png)

6. Synthesize dff_synC_res:

![alt text](../../Prints/Day02/D2-L6-P6.png)
