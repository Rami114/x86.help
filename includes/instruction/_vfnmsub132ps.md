## VFNMSUB132PS/VFNMSUB213PS/VFNMSUB231PS  -  Fused Negative Multiply-Subtract of Packed Single-Precision Floating-Point Values

> Operation

``` slim
In the operations below, \"+\", \"-\", and \"\*\" symbols represent addition, subtraction, and multiplication operations
with infinite precision inputs and outputs (no rounding).
VFNMSUB132PS DEST, SRC2, SRC3
IF (VEX.128) THEN
  MAXVL =4
ELSEIF (VEX.256)
  MAXVL = 8
FI
For i = 0 to MAXVL-1 {
  n = 32\*i;
  DEST[n+31:n] <- RoundFPControl_MXCSR( - (DEST[n+31:n]\*SRC3[n+31:n]) - SRC2[n+31:n])
}
IF (VEX.128) THEN
  DEST[VLMAX-1:128] <- 0
FI
VFNMSUB213PS DEST, SRC2, SRC3
IF (VEX.128) THEN
  MAXVL =4
ELSEIF (VEX.256)
  MAXVL = 8
FI
For i = 0 to MAXVL-1 {
  n = 32\*i;
  DEST[n+31:n] <- RoundFPControl_MXCSR( - (SRC2[n+31:n]\*DEST[n+31:n]) - SRC3[n+31:n])
}
IF (VEX.128) THEN
  DEST[VLMAX-1:128] <- 0
FI
VFNMSUB231PS DEST, SRC2, SRC3
IF (VEX.128) THEN
  MAXVL =4
ELSEIF (VEX.256)
  MAXVL = 8
FI
For i = 0 to MAXVL-1 {
  n = 32\*i;
  DEST[n+31:n] <- RoundFPControl_MXCSR( - (SRC2[n+31:n]\*SRC3[n+31:n]) - DEST[n+31:n])
}
IF (VEX.128) THEN
  DEST[VLMAX-1:128] <- 0
FI

```

 Opcode/Instruction                       | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 VEX.DDS.128.66.0F38.W0 9E /r VFNMSUB132PS| A    | V/V            | FMA               | Multiply packed single-precision floating-point
 xmm0, xmm1, xmm2/m128                    |      |                |                   | values from xmm0 and xmm2/mem, negate          
                                          |      |                |                   | the multiplication result and subtract         
                                          |      |                |                   | xmm1 and put result in xmm0.                   
 VEX.DDS.128.66.0F38.W0 AE /r VFNMSUB213PS| A    | V/V            | FMA               | Multiply packed single-precision floating-point
 xmm0, xmm1, xmm2/m128                    |      |                |                   | values from xmm0 and xmm1, negate the          
                                          |      |                |                   | multiplication result and subtract xmm2/mem    
                                          |      |                |                   | and put result in xmm0.                        
 VEX.DDS.128.66.0F38.W0 BE /r VFNMSUB231PS| A    | V/V            | FMA               | Multiply packed single-precision floating-point
 xmm0, xmm1, xmm2/m128                    |      |                |                   | values from xmm1 and xmm2/mem, negate          
                                          |      |                |                   | the multiplication result and subtract         
                                          |      |                |                   | xmm0 and put result in xmm0.                   
 VEX.DDS.256.66.0F38.W0 9E /r VFNMSUB132PS| A    | V/V            | FMA               | Multiply packed single-precision floating-point
 ymm0, ymm1, ymm2/m256                    |      |                |                   | values from ymm0 and ymm2/mem, negate          
                                          |      |                |                   | the multiplication result and subtract         
                                          |      |                |                   | ymm1 and put result in ymm0.                   
 VEX.DDS.256.66.0F38.W0 AE /r VFNMSUB213PS| A    | V/V            | FMA               | Multiply packed single-precision floating-point
 ymm0, ymm1, ymm2/m256                    |      |                |                   | values from ymm0 and ymm1, negate the          
                                          |      |                |                   | multiplication result and subtract ymm2/mem    
                                          |      |                |                   | and put result in ymm0.                        
 VEX.DDS.256.66.0F38.0 BE /r VFNMSUB231PS | A    | V/V            | FMA               | Multiply packed single-precision floating-point
 ymm0, ymm1, ymm2/m256                    |      |                |                   | values from ymm1 and ymm2/mem, negate          
                                          |      |                |                   | the multiplication result and subtract         
                                          |      |                |                   | ymm0 and put result in ymm0.                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 A    | ModRM:reg (r, w)| VEX.vvvv (r)| ModRM:r/m (r)| NA       

### Description
VFNMSUB132PS: Multiplies the four or eight packed single-precision floating-point
values from the first source operand to the four or eight packed single-precision
floating-point values in the third source operand. From negated infinite precision
intermediate results, subtracts the four or eight packed single-precision floating-point
values in the second source operand, performs rounding and stores the resulting
four or eight packed single-precision floating-point values to the destination
operand (first source operand). VFNMSUB213PS: Multiplies the four or eight packed
single-precision floating-point values from the second source operand to the
four or eight packed single-precision floating-point values in the first source
operand. From negated infinite precision intermediate results, subtracts the
four or eight packed single-precision floating-point values in the third source
operand, performs rounding and stores the resulting four or eight packed single-precision
### floatingpoint values to the destination operand (first source operand). VFNMSUB231PS
Multiplies the four or eight packed single-precision floating-point values from
the second source to the four or eight packed single-precision floating-point
values in the third source operand. From negated infinite precision intermediate
results, subtracts the four or eight packed single-precision floating-point
values in the first source operand, performs rounding and stores the resulting
four or eight packed single-precision floating-point values to the destination
operand (first source operand). VEX.128 encoded version: The destination operand
(also first source operand) is a XMM register and encoded in reg_field. The
second source operand is a XMM register and encoded in VEX.vvvv. The third source
operand is a

XMM register or a 128-bit memory location and encoded in rm_field. The upper
128 bits of the YMM destination register are zeroed.

VEX.256 encoded version: The destination operand (also first source operand)
is a YMM register and encoded in reg_field. The second source operand is a YMM
register and encoded in VEX.vvvv. The third source operand is a YMM register
or a 256-bit memory location and encoded in rm_field. Compiler tools may optionally
support a complementary mnemonic for each instruction mnemonic listed in the
opcode/instruction column of the summary table. The behavior of the complementary
mnemonic in situations involving NANs are governed by the definition of the
instruction mnemonic defined in the opcode/instruction column. See also Section
14.5.1, “FMA Instruction Operand Order and Arithmetic Behavior” in the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1.



### Intel C/C++ Compiler Intrinsic Equivalent
VFNMSUB132PS: __m128 _mm_fnmsub_ps (__m128 a, __m128 b, __m128 c);

VFNMSUB213PS: __m128 _mm_fnmsub_ps (__m128 a, __m128 b, __m128 c);

VFNMSUB231PS: __m128 _mm_fnmsub_ps (__m128 a, __m128 b, __m128 c);

VFNMSUB132PS: __m256 _mm256_fnmsub_ps (__m256 a, __m256 b, __m256 c);

VFNMSUB213PS: __m256 _mm256_fnmsub_ps (__m256 a, __m256 b, __m256 c);

VFNMSUB231PS: __m256 _mm256_fnmsub_ps (__m256 a, __m256 b, __m256 c);


### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal


### Other Exceptions
See Exceptions Type 2
