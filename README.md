# 8 Bit ALU Implementation
This project focuses on the design and implementation of an 8-bit ALU on an FPGA board using the Verilog language.

Software Used: `Xilinx Vivado 2024.2`

FPGA Board Used: `Xilinx Zedboard Zynq-7000 SoC`

## Design Specifications

![Functional_Block_Diagram_ALU](https://github.com/user-attachments/assets/678c6327-ea12-4803-af71-0e76e105d84e)

<p align='center'>
    <img src='Minor-2 Images/Functional Block Diagram ALU.png' width=500 height=300>
</p>

+ Inputs:
    1. Operand A (8 bits)
    2. Operand B (8 bits)
    3. Opcode (4 bits)
    4. Clock and Reset signals (for synchronization and reset functionality)

+ Outputs:
    1. Result Magnitude (8 bits)
    2. Carry Out Bit
    3. Sign Bit

+ This ALU is capable of performing the following operations:
    1. Arithmetic (+,-,*,/,%)
    2. Logical (AND,OR,NOT,XOR)
    3. Comparision (==,>,<)
    4. Shift (Logical left, Logical right)

## Opcode Assignment

| Operation Code (op) |	Operation	| Description	| Output (Result) |
|:-------------------:|:---------:|:-----------:|:---------------:|
| 0000 (4'b0000) |	Addition	| Adds A and B	| A + B |
| 0001 (4'b0001)	| Subtraction	| Subtracts B from A |	A - B |
| 0010 (4'b0010)	| Multiplication	| Multiplies A and B	| A × B |
| 0011 (4'b0011)	| Division	| Divides A by B (if B ≠ 0)	| A ÷ B (else 0) |
| 0100 (4'b0100)	| AND	Bitwise | AND between A and B |	A & B |
| 0101 (4'b0101)	| OR	Bitwise | OR between A and B	| "A | B" |
| 0110 (4'b0110)	| NOT	Bitwise | NOT of A (B is ignored) |	~A |
| 0111 (4'b0111)	| Modulus (Modulo) |	Remainder of A divided by B (if B ≠ 0) |	A mod B (else 0) |
| 1000 (4'b1000)	| XOR	Bitwise | XOR between A and B	| A ^ B |
| 1001 (4'b1001)	| Comparison	| "Compare A and B: output 1 if A > B, 2 if A == B, 0 if A < B"	| "0, 1, or 2" |
| 1010 (4'b1010)	| Logical Left Shift	| Shifts A left by 1 bit (×2)	| A << 1 |
| 1011 (4'b1011)	| Logical Right Shift	| Shifts A right by 1 bit (÷2)	| A >> 1 |
| (default)	| Default	| "If invalid opcode, result is 0"	| 0 |




| Flag | Name       | Description |
|:----:|------------|-------------|
| C    | Carry      | Enables numbers larger than a single word to be added/subtracted by carrying a binary digit from a less significant word to the least significant bit of a more significant word as needed. |
| V    | Overflow   | Indicates that the signed result of an operation is too large to fit in the register width using two's complement representation. |
| S    | Sign       | Indicates that the result of a mathematical operation is negative. |
| Z    | Zero       | Indicates that the result of an arithmetic or logical operation (or a load) was zero. |
| P    | Parity     | Indicates whether the number of set bits of the last result is even or odd (``Odd=1``). |
| H    | Half-carry | Indicates that a bit carry was produced between the nibbles (4-bit halves of a byte operand) as a result of the last arithmetic operation. |
