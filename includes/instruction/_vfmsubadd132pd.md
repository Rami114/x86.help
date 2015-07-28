## VFMSUBADD132PD/VFMSUBADD213PD/VFMSUBADD231PD  -  Fused Multiply-Alternating Subtract/Add of Packed Double-Precision Floating-Point Values

> Operation
``` slim

In the operations below, \"+\", \"-\", and \"\*\" symbols represent addition, subtraction, and multiplication operations
with infinite precision inputs and outputs (no rounding).
VFMSUBADD132PD DEST, SRC2, SRC3
IF (VEX.128) THEN
  DEST[63:0] <- RoundFPControl_MXCSR(DEST[63:0]\*SRC3[63:0] + SRC2[63:0])
  DEST[127:64] <- RoundFPControl_MXCSR(DEST[127:64]\*SRC3[127:64] - SRC2[127:64])
  DEST[VLMAX-1:128] <- 0
ELSEIF (VEX.256)
  DEST[63:0] <- RoundFPControl_MXCSR(DEST[63:0]\*SRC3[63:0] + SRC2[63:0])
  DEST[127:64] <- RoundFPControl_MXCSR(DEST[127:64]\*SRC3[127:64] - SRC2[127:64])
  DEST[191:128] <- RoundFPControl_MXCSR(DEST[191:128]\*SRC3[191:128] + SRC2[191:128])
  DEST[255:192] <- RoundFPControl_MXCSR(DEST[255:192]\*SRC3[255:192] - SRC2[255:192]
FI
VFMSUBADD213PD DEST, SRC2, SRC3
IF (VEX.128) THEN
  DEST[63:0] <- RoundFPControl_MXCSR(SRC2[63:0]\*DEST[63:0] + SRC3[63:0])
  DEST[127:64] <- RoundFPControl_MXCSR(SRC2[127:64]\*DEST[127:64] - SRC3[127:64])
  DEST[VLMAX-1:128] <- 0
ELSEIF (VEX.256)
  DEST[63:0] <- RoundFPControl_MXCSR(SRC2[63:0]\*DEST[63:0] + SRC3[63:0])
  DEST[127:64] <- RoundFPControl_MXCSR(SRC2[127:64]\*DEST[127:64] - SRC3[127:64])
  DEST[191:128] <- RoundFPControl_MXCSR(SRC2[191:128]\*DEST[191:128] + SRC3[191:128])
  DEST[255:192] <- RoundFPControl_MXCSR(SRC2[255:192]\*DEST[255:192] - SRC3[255:192]
FI
VFMSUBADD231PD DEST, SRC2, SRC3
IF (VEX.128) THEN
  DEST[63:0] <- RoundFPControl_MXCSR(SRC2[63:0]\*SRC3[63:0] + DEST[63:0])
  DEST[127:64] <- RoundFPControl_MXCSR(SRC2[127:64]\*SRC3[127:64] - DEST[127:64])
  DEST[VLMAX-1:128] <- 0
ELSEIF (VEX.256)
  DEST[63:0] <- RoundFPControl_MXCSR(SRC2[63:0]\*SRC3[63:0] + DEST[63:0])
  DEST[127:64] <- RoundFPControl_MXCSR(SRC2[127:64]\*SRC3[127:64] - DEST[127:64])
  DEST[191:128] <- RoundFPControl_MXCSR(SRC2[191:128]\*SRC3[191:128] + DEST[191:128])
  DEST[255:192] <- RoundFPControl_MXCSR(SRC2[255:192]\*SRC3[255:192] - DEST[255:192]
FI

```

 Opcode/Instruction                         | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 VEX.DDS.128.66.0F38.W1 97 /r VFMSUBADD132PD| A    | V/V            | FMA               | Multiply packed double-precision floating-point
 xmm0, xmm1, xmm2/m128                      |      |                |                   | values from xmm0 and xmm2/mem, subtract/add    
                                            |      |                |                   | elements in xmm1 and put result in xmm0.       
 VEX.DDS.128.66.0F38.W1 A7 /r VFMSUBADD213PD| A    | V/V            | FMA               | Multiply packed double-precision floating-point
 xmm0, xmm1, xmm2/m128                      |      |                |                   | values from xmm0 and xmm1, subtract/add        
                                            |      |                |                   | elements in xmm2/mem and put result            
                                            |      |                |                   | in xmm0.                                       
 VEX.DDS.128.66.0F38.W1 B7 /r VFMSUBADD231PD| A    | V/V            | FMA               | Multiply packed double-precision floating-point
 xmm0, xmm1, xmm2/m128                      |      |                |                   | values from xmm1 and xmm2/mem, subtract/add    
                                            |      |                |                   | elements in xmm0 and put result in xmm0.       
 VEX.DDS.256.66.0F38.W1 97 /r VFMSUBADD132PD| A    | V/V            | FMA               | Multiply packed double-precision floating-point
 ymm0, ymm1, ymm2/m256                      |      |                |                   | values from ymm0 and ymm2/mem, subtract/add    
                                            |      |                |                   | elements in ymm1 and put result in ymm0.       
 VEX.DDS.256.66.0F38.W1 A7 /r VFMSUBADD213PD| A    | V/V            | FMA               | Multiply packed double-precision floating-point
 ymm0, ymm1, ymm2/m256                      |      |                |                   | values from ymm0 and ymm1, subtract/add        
                                            |      |                |                   | elements in ymm2/mem and put result            
                                            |      |                |                   | in ymm0.                                       
 VEX.DDS.256.66.0F38.W1 B7 /r VFMSUBADD231PD| A    | V/V            | FMA               | Multiply packed double-precision floating-point
 ymm0, ymm1, ymm2/m256                      |      |                |                   | values from ymm1 and ymm2/mem, subtract/add    
                                            |      |                |                   | elements in ymm0 and put result in ymm0.       

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 A    | ModRM:reg (r, w)| VEX.vvvv (r)| ModRM:r/m (r)| NA       

### Description
VFMSUBADD132PD: Multiplies the two or four packed double-precision floating-point
values from the first source operand to the two or four packed double-precision
floating-point values in the third source operand. From the infinite precision
intermediate result, subtracts the odd double-precision floating-point elements
and adds the even double-precision floating-point values in the second source
operand, performs rounding and stores the resulting two or four packed double-precision
### floating-point values to the destination operand (first source operand). VFMSUBADD213PD
Multiplies the two or four packed double-precision floating-point values from
the second source operand to the two or four packed double-precision floating-point
values in the first source operand. From the infinite precision intermediate
result, subtracts the odd double-precision floating-point elements and adds
the even double-precision floating-point values in the third source operand,
performs rounding and stores the resulting two or four packed double-precision
### floating-point values to the destination operand (first source operand). VFMSUBADD231PD
Multiplies the two or four packed double-precision floating-point values from
the second source operand to the two or four packed double-precision floating-point
values in the third source operand. From the infinite precision intermediate
result, subtracts the odd double-precision floating-point elements and adds
the even double-precision floating-point values in the first source operand,
performs rounding and stores the resulting two or four packed double-precision
floating-point values to the destination operand (first source operand). VEX.128
encoded version: The destination operand (also first source operand) is a XMM
register and encoded in reg_field. The second source operand is a XMM register
and encoded in VEX.vvvv. The third source operand is a XMM register or a 128-bit
memory location and encoded in rm_field. The upper 128 bits of the YMM destination
register are zeroed.

VEX.256 encoded version: The destination operand (also first source operand)
is a YMM register and encoded in reg_field. The second source operand is a YMM
register and encoded in VEX.vvvv. The third source operand is a YMM register
or a 256-bit memory location and encoded in rm_field.

Compiler tools may optionally support a complementary mnemonic for each instruction
mnemonic listed in the opcode/instruction column of the summary table. The behavior
of the complementary mnemonic in situations involving NANs are governed by the
definition of the instruction mnemonic defined in the opcode/instruction column.
See also Section 14.5.1, “FMA Instruction Operand Order and Arithmetic Behavior”
in the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume
1.



### Intel C/C++ Compiler Intrinsic Equivalent
VFMSUBADD132PD: __m128d _mm_fmsubadd_pd (__m128d a, __m128d b, __m128d c);

VFMSUBADD213PD: __m128d _mm_fmsubadd_pd (__m128d a, __m128d b, __m128d c);

VFMSUBADD231PD: __m128d _mm_fmsubadd_pd (__m128d a, __m128d b, __m128d c);

VFMSUBADD132PD: __m256d _mm256_fmsubadd_pd (__m256d a, __m256d b, __m256d c);

VFMSUBADD213PD: __m256d _mm256_fmsubadd_pd (__m256d a, __m256d b, __m256d c);

VFMSUBADD231PD: __m256d _mm256_fmsubadd_pd (__m256d a, __m256d b, __m256d c);


### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal


### Other Exceptions
See Exceptions Type 2
