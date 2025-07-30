# Hierarchical vs. Flat Synthesis

The choice between a flat or hierarchical methodology determines how a synthesis tool "sees" and processes a design. It's a fundamental choice between treating the design as a single massive entity or as an organized collection of smaller, interconnected blocks.

## Flat Synthesis

In a flat synthesis approach, the tool dissolves all the boundaries between the different modules (or sub-modules) defined in the RTL code. It "flattens" the entire design into a single, top-level module that contains only primitive logic gates and the connections between them.

## Hierarchical Synthesis

This is a "divide and conquer" strategy. The design is kept as a collection of distinct sub-modules, just as it was written in the RTL. The synthesis tool works on each block independently, optimizing it based on its specific inputs, outputs, and constraints. The fully synthesized blocks are then integrated at the top level.


## The Importance of Synthesizing Sub-Modules Separately

Adopting a hierarchical flow where sub-modules are synthesized independently is better then the flat aproach. Synthesizing smaller blocks is much faster than tackling the entire chip at once. This "divide and conquer" method enables parallel work and rapid design iterations, as changes only require re-synthesis of the single affected block. Modules that are independently synthesized are easier to debug and reuse in future projects.