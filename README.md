# 8 Bit ALU Implementation
This project focuses on the design and implementation of an 8-bit ALU on an FPGA board using the Verilog language.

Software Used: `Xilinx Vivado 2024.2`

FPGA Board Used: `Xilinx Zedboard Zynq-7000 SoC`

---

## Design Specifications

<p align='center'>
    <img src='Minor-2 Images/Functional Block Diagram ALU.png' width=500>
</p>

+ Inputs:
    1. **A[7:0]:** First 8-bit input operand.
    2. **B[7:0]:** Second 8-bit input operand.
    3. **ALU_Sel[3:0]:** 4-bit operation selector (e.g., +, AND, etc.).
    4. **clk:** Button-press clock for shifting bits into the shift registers for A & B.
    5. **rst:** Reset signal (clears both A and B registers).


+ Outputs:
    1. **Result[9:0]** (Sign, Carry-out, 8-bit magnitude)


+ This ALU is capable of performing the following operations:
    1. Arithmetic (+,-,*,/,%)
    2. Logical (AND,OR,NOT,XOR)
    3. Comparision (==,>,<)
    4. Shift (Logical left, Logical right)

---

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

---

## Modules Included
1. `ALU_8bit`: Core computaion (Implements 8-bit arithmetic and logic operations using a 4-bit selector)
2. `ALU_8bit_loader`: Input loader (Loads 8-bit inputs A and B using switches and clock (bit-serial loading))
3. `ALU_8bit_tb`: Testbench (Runs simulations for all ALU operations (does not synthesize onto FPGA))
4. `top_module`: System integrator (Integrates `ALU_8bit` and `ALU_8bit_loader` with physical pins (switches, LEDs, buttons))
5. `constraints.xdc`: Pin mapping (Maps logical ports to physical pins on Zynq-7000 (switches, LEDs, buttons, etc.))

---

## How to Use

+ ### Simulation Steps:
    1. Open Xilinx Vivado and create a new project, select the Zedboard.
    2. Add the `alu.v` and `top_alu.v` file to the project under design sources.
    3. Save the files.
    4. Set `alu_tb.v` file as Top.
    5. Run behavioral simulation to verify the ALU functionality by generating the waveform.
    6. You can set variable values for A, B and ALU_Sel using `Force Constant` option.

+ ### Synthesis, Implementation & Bitsream Generation Steps:
    1. Add the `pin_mapping.xdc` file in contraints.
    2. Set `ALU_8bit_loader` file as Top.
    3. Synthesize the `alu.v` file.
    4. Connect the system to the Zedboard using a USB JTAG cable.
    5. Run implementation.
    6. Generate the bitstream for implementation.
    7. Test the design on hardware by setting A, B, ALU_Sel using innternal switches present on Zedboard.

+ ### Loading A and B on Hardware:
    1. Set `sw_a_bit` (Switch H19) to either 0 or 1 for the first (MSB) bit `A[7]`.
    2. Set `sw_b_bit` (Switch H18) to either 0 or 1 for the first (MSB) bit `B[7]`.
    3. Press `clk` (Button P16) once → `A[7]` & `B[7]` bits shift into registers A & B respectively.
    4. Set switches H19 & H18 to 0 or 1 for bits `A[6]` & `B[6]` respectively.
    5. Press button P16 once → these bits shift into registers.
    6. Repeat 6 times till LSB bits are entered.

+ ### Loading ALU_Sel on Hardware:
    Set 4 ALU selector switches (`ALU_Sel[3:0]`) to desired operation code:

       Example: 0000 → Add, 0001 → Subtract, etc.

    By setting it's value on switches F21, H22, G22, F22.

+ ### Observing Output on Hardware:
    1. `LEDs[7:0]` (Pins U14 ,U19 ,W22 ,V22 ,U21 ,U22 ,T21 ,T22)  → show magnitude result (8 bits).
    2. Connect external LED to pin AA11 to observe `Carry_out` bit → High if carry occurred in add/mul.
    3. Connect external LED to pin Y11 to observe `Sign_bit` → High if result is negative (used only in subtraction).

---

## Outputs

Output generated using [testbench](https://github.com/jhalak-agarwal/8_Bit_ALU_Implementation/blob/ee04436b145d43ec2e68ceecdaf63ff4a88b0594/alu.v%20file#L86)

<p align = 'center'>
    <img src='Minor-2 Images/Simulation Waveform.png' width=800 height=450>
</p>


Output generated using [top_module](top_alu.v file) and [constraints](pin_mapping.xdc file)

<p align = 'center'>
    <img src='Minor-2 Images/FPGA Bitstream Generated.png' width=800 height=450>
</p>










