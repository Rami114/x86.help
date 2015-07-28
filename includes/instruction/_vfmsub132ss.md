## VFMSUB132SS/VFMSUB213SS/VFMSUB231SS  -  Fused Multiply-Subtract of Scalar SinglePrecision Floating-Point Values

> Operation
``` slim

In the operations below, \"+\", \"-\", and \"\*\" symbols represent addition, subtraction, and multiplication operations
with infinite precision inputs and outputs (no rounding).
VFMSUB132SS DEST, SRC2, SRC3
DEST[31:0] <- RoundFPControl_MXCSR(DEST[31:0]\*SRC3[31:0] - SRC2[31:0])
DEST[127:32] <- DEST[127:32]
DEST[VLMAX-1:128] <- 0
VFMSUB213SS DEST, SRC2, SRC3
DEST[31:0] <- RoundFPControl_MXCSR(SRC2[31:0]\*DEST[31:0] - SRC3[31:0])
DEST[127:32] <- DEST[127:32]
DEST[VLMAX-1:128] <- 0
VFMSUB231SS DEST, SRC2, SRC3
DEST[31:0] <- RoundFPControl_MXCSR(SRC2[31:0]\*SRC3[63:0] - DEST[31:0])
DEST[127:32] <- DEST[127:32]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                          | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 VEX.DDS.LIG.128.66.0F38.W0 9B /r VFMSUB132SS| A    | V/V            | FMA               | Multiply scalar single-precision floating-point
 xmm0, xmm1, xmm2/m32                        |      |                |                   | value from xmm0 and xmm2/mem, subtract         
                                             |      |                |                   | xmm1 and put result in xmm0.                   
 VEX.DDS.LIG.128.66.0F38.W0 AB /r VFMSUB213SS| A    | V/V            | FMA               | Multiply scalar single-precision floating-point
 xmm0, xmm1, xmm2/m32                        |      |                |                   | value from xmm0 and xmm1, subtract xmm2/mem    
                                             |      |                |                   | and put result in xmm0.                        
 VEX.DDS.LIG.128.66.0F38.W0 BB /r VFMSUB231SS| A    | V/V            | FMA               | Multiply scalar single-precision floating-point
 xmm0, xmm1, xmm2/m32                        |      |                |                   | value from xmm1 and xmm2/mem, subtract         
                                             |      |                |                   | xmm0 and put result in xmm0.                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 A    | ModRM:reg (r, w)| VEX.vvvv (r)| ModRM:r/m (r)| NA       

### Description
Performs a SIMD multiply-subtract computation on the low packed single-precision
floating-point values using three source operands and writes the multiply-add
result in the destination operand. The destination operand is also the first
source operand. The second operand must be a SIMD register. The third source
operand can be a SIMD register or a memory location. VFMSUB132SS: Multiplies
the low packed single-precision floating-point value from the first source operand
to the low packed single-precision floating-point value in the third source
operand. From the infinite precision intermediate result, subtracts the low
packed single-precision floating-point values in the second source operand,
performs rounding and stores the resulting packed single-precision floating-point
value to the destination operand (first source operand). VFMSUB213SS: Multiplies
the low packed single-precision floating-point value from the second source
operand to the low packed single-precision floating-point value in the first
source operand. From the infinite precision intermediate result, subtracts the
low packed single-precision floating-point value in the third source operand,
performs rounding and stores the resulting packed single-precision floating-point
value to the destination operand (first source operand). VFMSUB231SS: Multiplies
the low packed single-precision floating-point value from the second source
to the low packed single-precision floating-point value in the third source
operand. From the infinite precision intermediate result, subtracts the low
packed single-precision floating-point value in the first source operand, performs
rounding and stores the resulting packed single-precision floating-point value
### to the destination operand (first source operand). VEX.128 encoded version
The destination operand (also first source operand) is a XMM register and encoded
in reg_field. The second source operand is a XMM register and encoded in VEX.vvvv.
The third source operand is a XMM register or a 32-bit memory location and encoded
in rm_field. The upper bits ([VLMAX-1:128]) of the YMM destination register
are zeroed. Compiler tools may optionally support a complementary mnemonic for
each instruction mnemonic listed in the opcode/instruction column of the summary
table. The behavior of the complementary mnemonic in situations involving NANs
are governed by the definition of the instruction mnemonic defined in the opcode/instruction
column. See also Section 14.5.1, “FMA Instruction Operand Order and Arithmetic
Behavior” in the Intel® 64 and IA-32 Architectures Software Developer's Manual,
Volume 1.



### Intel C/C++ Compiler Intrinsic Equivalent
VFMSUB132SS: __m128 _mm_fmsub_ss (__m128 a, __m128 b, __m128 c);

VFMSUB213SS: __m128 _mm_fmsub_ss (__m128 a, __m128 b, __m128 c);

VFMSUB231SS: __m128 _mm_fmsub_ss (__m128 a, __m128 b, __m128 c);


### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal


### Other Exceptions
See Exceptions Type 3
