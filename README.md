# Week 2
## Contents
- [Concept of Verilog Module](#concept-of-verilog-module)
  - [Verilog Module Structure](#verilog-module-structure)
  - [Examples](#examples)
- [Verilog Data Types](#verilog-data-types)
  -  []()
 

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

Note:

The "assign" statement in Verilog signifies continuous assignment, updating the variable on the left-hand side (LHS) whenever the expression on the right-hand side (RHS) changes.

```verilog
assign variable = expression;
```
- `LHS Requirement:` The LHS must be a "net" type variable, usually a "wire" in Verilog, representing signals or interconnections.
- `RHS Content:` The RHS can consist of both "register" and "net" type variables, allowing for the construction of complex expressions and logic.
- `Placement in Modules:` Verilog modules can contain any number of "assign" statements. Typically, they are positioned at the beginning after the port declarations, contributing to a structured code organization.
- `Behavioral Design Style:` The "assign" statement embodies a behavioral design style, commonly used for modeling combinational circuits. It succinctly describes how signals are updated based on logical conditions within the module.

### Instances:

- In Verilog, a module serves as a template for creating actual objects known as instances.
- When a module is invoked, Verilog generates a unique object with its own name, variables, parameters, and I/O interface.
- This process of creating objects from a module template is called instantiation, and the objects are referred to as instances.
- Example 2-1 demonstrates module instantiation for a "ripple carry counter" top-level block, creating four instances from the T-flipflop (T_FF) template.
  - Each T_FF instance instantiates a D_FF (D-flipflop) and an inverter gate.
  - Each instance is given a unique name, and the interconnections are shown in Section 2.2, 4-bit Ripple Carry Counter.

```verilog

module ripple_carry_counter(q, clk, reset);
  output [3:0] q;
  input clk, reset;
  
  T_FF tff0(q[0], clk, reset);
  T_FF tff1(q[1], q[0], reset);
  T_FF tff2(q[2], q[1], reset);
  T_FF tff3(q[3], q[2], reset);
  
endmodule

module T_FF(q, clk, reset);
  output q;
  input clk, reset;
  wire d;
  
  D_FF dff0(q, d, clk, reset);
  not n1(d, q); 
endmodule
```

- It's crucial to note that nesting modules (defining one module within another) is illegal in Verilog.
- Modules can be incorporated into other module definitions by instantiation.
- Module definitions specify how a module functions, its internal workings, and its interface.
- Instances are created for use in the design.

Example 2-2 demonstrates an illegal attempt to nest modules, where the module T_FF is defined inside the module definition of the ripple carry counter. This violates Verilog syntax rules.

```verilog
module ripple_carry_counter(q, clk, reset);
  output [3:0] q;
  input clk, reset;
  
  // ILLEGAL MODULE NESTING
  module T_FF(q, clock, reset);
    // ... <module T_FF internals> ...
  endmodule // END OF ILLEGAL MODULE NESTING
  
endmodule  
```

### Points to remember:

- In Verilog, when you instantiate a module, you essentially create a copy of that module's logic for each instance. Each instance is self-contained, and this replication contributes to an increase in the overall size of the hardware design.

- On the contrary, in high-level languages like C, when you call a function, you are not duplicating the entire function's code for each call. Instead, the function's logic is typically stored in one location in memory, and each call references that same logic. This is a more memory-efficient approach compared to the instantiation process in hardware description languages.

- The distinction arises from the fundamentally different nature of hardware design and software programming. In hardware, each instance needs its own set of physical components, leading to replication of logic. In software, functions are generally stored centrally in memory, and function calls reference this centralized logic, resulting in a more compact memory footprint.

- This difference in behavior reflects the divergent requirements and constraints of hardware and software design. Hardware design often necessitates explicit replication for parallelism and concurrent processing, while software design prioritizes memory efficiency and reuse of code.

## Verilog Data Types

### Net Type:

- Requires continuous driving, ensuring a constant flow of information.
- Lacks the capability to store values locally.
- Primarily utilized for describing and modeling connections between continuous assignments and instances. It plays a crucial role in establishing communication pathways.

### Register Type:

- Acts as a storage element that retains the last assigned value.
- Frequently employed to represent memory or sequential elements in a design.
- Suitable for scenarios where it's necessary to preserve and recall specific information over time.
- Offers a means of capturing and holding state information within the circuit.  

## Net Data Type 

The Net data type in Verilog plays a pivotal role in hardware modeling, acting as a representation of connections between different elements within a digital circuit. These nets are continuously driven by the outputs of the devices they are connected to, dynamically responding to changes in the circuit. By default, nets are treated as 1-bit values unless explicitly declared as vectors, and their default value is "z," indicating a high-impedance or undefined state when not actively driven. Verilog supports various net data types, each tailored for specific use cases in hardware modeling. The "wire" data type stands out as the most common and versatile, widely employed for modeling diverse interconnections in digital circuits.

### Key Points:

- Nets in Verilog represent connections between hardware elements in digital circuits.
- They are continuously driven by the outputs of connected devices, allowing real-time adaptation to circuit changes.
- Nets default to 1-bit values, with a default value of "z" when not actively driven.
- Verilog supports various net data types, including:
  - ```wire```
  - ```wor```
  - ```wand```
  - ```tri```
  - ```supply0```
  - ```supply1```
  - ```... and more.```
    
- The ```"wire"``` data type in Verilog is a versatile choice widely used for modeling diverse interconnections in digital circuits.
- Equivalence between ```"wire"``` and ```"tri"``` allows seamless handling of multiple drivers, with their outputs shorted together when concurrent driving occurs.
- ```"wor"``` and ```"wand"``` variants enhance logical modeling by inserting OR and AND gates at the connection point, providing flexibility in altering logical operations.
- Dedicated models ```"supply0"``` and ```"supply1"``` in Verilog precisely represent power supply connections with constant logic values of 0 and 1, respectively. They play a crucial role in accurate power distribution simulation.

### Examples:

```verilog
// Module: use_wire
// Description: Demonstrates the usage of a wire for signal connection and the consequences of conflicting assignments.

module use_wire (A, B, C, D, f);
  input A, B, C, D; // Input ports A, B, C, D
  output f;         // Output port f
  wire f;           // Declaration of wire f

  // Conflicting assignments to f, attempting to simultaneously perform AND and OR operations
  assign f = A & B;
  assign f = C | D;
endmodule
```

- This module attempts to assign conflicting values to the output signal f.
- The conflicting assignments lead to undesired behavior, making the output f indeterminate under certain conditions (A=B=1, C=D=0).

### Behavioral Design:

![Screenshot 2023-12-25 141402](https://github.com/akhiiasati/Hardware-Modeling-Using-Verilog/assets/43675821/c20da4a9-d614-4df8-b3ec-acd09d24a09e)


```verilog
// Module: use_wand
// Description: Similar to use_wire but introduces the wand type for f, attempting to address the issue.

module use_wand (A, B, C, D, f);
  input A, B, C, D; // Input ports A, B, C, D
  output f;         // Output port f
  wand f;           // Declaration of wand f

  // Conflicting assignments to f, attempting to simultaneously perform AND and OR operations
  assign f = A & B;
  assign f = C | D;
endmodule
```

- This module is similar to use_wire but declares f as a wand (wire with specified logic behavior).
- The conflicting assignments still lead to undesired behavior.

### Behavioral Design:
![Screenshot 2023-12-25 141838](https://github.com/akhiiasati/Hardware-Modeling-Using-Verilog/assets/43675821/af4b5633-d86f-40e2-b707-5769ae805fd0)


```verilog
// Module: using_supply_wire
// Description: Introduces supply lines (supply0 and supply1) and demonstrates their use in logic gate instantiations.

module using_supply_wire (A, B, C, f);
  input A, B, C;    // Input ports A, B, C
  output f;         // Output port f
  supply0 gnd;      // Ground supply
  supply1 vdd;      // Power supply
  wire t1, t2;      // Intermediate wires t1 and t2
  wire f;           // Declaration of wire f

  // NAND gate instantiation with vdd as a power supply
  nand G1 (t1, vdd, A, B);
  // XOR gate instantiation with gnd as a ground supply
  xor G2 (t2, C, gnd);
  // AND gate instantiation combining t1 and t2
  and G3 (f, t1, t2);
endmodule
```

- This module demonstrates the use of supply lines (supply0 and supply1) in Verilog.
- Instantiates NAND, XOR, and AND gates with connections to supply lines.
- Supply0 and Supply1 are used for ground and power connections, respectively.

### Behavioral Design:
![Screenshot 2023-12-25 142130](https://github.com/akhiiasati/Hardware-Modeling-Using-Verilog/assets/43675821/9feedb48-834a-4c09-acc1-d4205c2c2021)

