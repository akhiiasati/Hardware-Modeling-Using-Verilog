# Hardware Modeling Using Verilog
# Week 2
## Contents
- [Concept of Verilog Module](#concept-of-verilog-module)
  - [Verilog Module Structure](#verilog-module-structure)

## Concept of Verilog Module

In Verilog, the fundamental building block of hardware design is called a module. Understanding the characteristics of modules is crucial for creating organized and scalable digital designs. Here are key points to consider:

- `Module Definition:` A module in Verilog represents a self-contained unit that encapsulates a specific functionality or component within a digital system.

- `Limitation on Module Definitions:` Importantly, a module cannot contain definitions of other modules directly. Each module is intended to encapsulate a specific aspect of the overall design.

- `Module Instantiation:` Despite the limitation on containing module definitions, Verilog allows modules to be instantiated within other modules. This instantiation capability facilitates a modular and hierarchical design approach.

- `Hierarchy through Instantiation:` The process of instantiation enables the creation of a hierarchy in Verilog descriptions. Designers can break down complex systems into smaller, manageable modules and instantiate them as needed, promoting a structured and organized design methodology.

Understanding these principles is essential for Verilog designers, as it allows them to efficiently manage complexity, enhance code readability, and promote a modular design approach for digital systems.

### Verilog Module Structure

```verilog

// Verilog Module Template

// Module Declaration
module module_name (list_of_ports);

  // Section 1: Input/Output Declarations
  // -----------------------------------
  // Declare input and output ports for the module.
  input/output declarations;

  // Section 2: Local Net Declarations
  // --------------------------------
  // Declare local wires or registers for internal connections.
  wire/reg declarations;

  // Section 3: Parallel Statements
  // -----------------------------
  // Write Verilog code representing the functionality of the module.
  // This section contains the actual logic of the module.
  // You can include both combinational and sequential statements here.

endmodule
```
### Examples:
```verilog
// A simple AND function

module simpleand (f, x, y);
  input x, y;
  output f;
  assign f = x & y;
endmodule
```

This Verilog code represents a behavioral description of a simple AND function. The module `simpleand` takes two input signals, `x` and `y`, and produces a single output signal `f` using the bitwise AND operation.

The synthesis tool will determine how to realize the output signal `f`:

1. Using a single AND gate.
2. Using a NAND gate followed by a NOT gate.

``` verilog
/* A 2-level combinational circuit */

module two_level (a, b, c, d, f);
input a, b, c, d;
output f;
wire t1, t2; // Intermediate lines!
assign t1 = a & b;
assign t2 = ~(c | d);
assign f = ~(t1 & t2);
endmodule
```
This Verilog code represents a behavioral description of a 2-level combinational circuit. The module `two_level` takes four inputs, `a`, `b`, `c`, and `d`, and produces a single output signal `f` based on the defined logical operations.

This is also a behavioral description. One possible gate-level realization is shown below. The intermediate lines, denoted as `t1` and `t2`, are declared as wire data types to represent the connections between gates.

![Screenshot 2023-12-20 205615](https://github.com/akhiiasati/Hardware-Modeling-Using-Verilog/assets/43675821/4416cd6e-5d08-4e0e-98bd-c66ed4c748c3)


