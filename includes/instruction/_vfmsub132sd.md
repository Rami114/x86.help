## VFMSUB132SD/VFMSUB213SD/VFMSUB231SD  -  Fused Multiply-Subtract of Scalar DoublePrecision Floating-Point Values

> Operation

``` slim
In the operations below, \"+\", \"-\", and \"\*\" symbols represent addition, subtraction, and multiplication operations
with infinite precision inputs and outputs (no rounding).
VFMSUB132SD DEST, SRC2, SRC3
DEST[63:0] <- RoundFPControl_MXCSR(DEST[63:0]\*SRC3[63:0] - SRC2[63:0])
DEST[127:64] <- DEST[127:64]
DEST[VLMAX-1:128] <- 0
VFMSUB213SD DEST, SRC2, SRC3
DEST[63:0] <- RoundFPControl_MXCSR(SRC2[63:0]\*DEST[63:0] - SRC3[63:0])
DEST[127:64] <- DEST[127:64]
DEST[VLMAX-1:128] <- 0
VFMSUB231SD DEST, SRC2, SRC3
DEST[63:0] <- RoundFPControl_MXCSR(SRC2[63:0]\*SRC3[63:0] - DEST[63:0])
DEST[127:64] <- DEST[127:64]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                          | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 VEX.DDS.LIG.128.66.0F38.W1 9B /r VFMSUB132SD| A    | V/V            | FMA               | Multiply scalar double-precision floating-point
 xmm0, xmm1, xmm2/m64                        |      |                |                   | value from xmm0 and xmm2/mem, subtract         
                                             |      |                |                   | xmm1 and put result in xmm0.                   
 VEX.DDS.LIG.128.66.0F38.W1 AB /r VFMSUB213SD| A    | V/V            | FMA               | Multiply scalar double-precision floating-point
 xmm0, xmm1, xmm2/m64                        |      |                |                   | value from xmm0 and xmm1, subtract xmm2/mem    
                                             |      |                |                   | and put result in xmm0.                        
 VEX.DDS.LIG.128.66.0F38.W1 BB /r VFMSUB231SD| A    | V/V            | FMA               | Multiply scalar double-precision floating-point
 xmm0, xmm1, xmm2/m64                        |      |                |                   | value from xmm1 and xmm2/mem, subtract         
                                             |      |                |                   | xmm0 and put result in xmm0.                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 A    | ModRM:reg (r, w)| VEX.vvvv (r)| ModRM:r/m (r)| NA       

### Description
Performs a SIMD multiply-subtract computation on the low packed double-precision
floating-point values using three source operands and writes the multiply-add
result in the destination operand. The destination operand is also the first
source operand. The second operand must be a SIMD register. The third source
operand can be a SIMD register or a memory location. VFMSUB132SD: Multiplies
the low packed double-precision floating-point value from the first source operand
to the low packed double-precision floating-point value in the third source
operand. From the infinite precision intermediate result, subtracts the low
packed double-precision floating-point values in the second source operand,
performs rounding and stores the resulting packed double-precision floating-point
value to the destination operand (first source operand). VFMSUB213SD: Multiplies
the low packed double-precision floating-point value from the second source
operand to the low packed double-precision floating-point value in the first
source operand. From the infinite precision intermediate result, subtracts the
low packed double-precision floating-point value in the third source operand,
performs rounding and stores the resulting packed double-precision floating-point
value to the destination operand (first source operand). VFMSUB231SD: Multiplies
the low packed double-precision floating-point value from the second source
to the low packed double-precision floating-point value in the third source
operand. From the infinite precision intermediate result, subtracts the low
packed double-precision floating-point value in the first source operand, performs
rounding and stores the resulting packed double-precision floating-point value
### to the destination operand (first source operand). VEX.128 encoded version
The destination operand (also first source operand) is a XMM register and encoded
in reg_field. The second source operand is a XMM register and encoded in VEX.vvvv.
The third source operand is a XMM register or a 64-bit memory location and encoded
in rm_field. The upper bits ([VLMAX-1:128]) of the YMM destination register
are zeroed. Compiler tools may optionally support a complementary mnemonic for
each instruction mnemonic listed in the opcode/instruction column of the summary
table. The behavior of the complementary mnemonic in situations involving NANs
are governed by the definition of the instruction mnemonic defined in the opcode/instruction
column. See also Section 14.5.1, “FMA Instruction Operand Order and Arithmetic
Behavior” in the Intel® 64 and IA-32 Architectures Software Developer's Manual,
Volume 1.



### Intel C/C++ Compiler Intrinsic Equivalent
VFMSUB132SD: __m128d _mm_fmsub_sd (__m128d a, __m128d b, __m128d c);

VFMSUB213SD: __m128d _mm_fmsub_sd (__m128d a, __m128d b, __m128d c);

VFMSUB231SD: __m128d _mm_fmsub_sd (__m128d a, __m128d b, __m128d c);


### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal


### Other Exceptions
See Exceptions Type 3
