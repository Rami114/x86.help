## VFMADD132PD/VFMADD213PD/VFMADD231PD  -  Fused Multiply-Add of Packed DoublePrecision Floating-Point Values

> Operation

``` slim
In the operations below, \"+\", \"-\", and \"*\" symbols represent addition, subtraction, and multiplication operations
with infinite precision inputs and outputs (no rounding).
VFMADD132PD DEST, SRC2, SRC3
IF (VEX.128) THEN
  MAXVL =2
ELSEIF (VEX.256)
  MAXVL = 4
FI
For i = 0 to MAXVL-1 {
  n = 64*i;
  DEST[n+63:n] <- RoundFPControl_MXCSR(DEST[n+63:n]*SRC3[n+63:n] + SRC2[n+63:n])
}
IF (VEX.128) THEN
DEST[VLMAX-1:128] <- 0
FI
VFMADD213PD DEST, SRC2, SRC3
IF (VEX.128) THEN
  MAXVL =2
ELSEIF (VEX.256)
  MAXVL = 4
FI
For i = 0 to MAXVL-1 {
  n = 64*i;
  DEST[n+63:n] <- RoundFPControl_MXCSR(SRC2[n+63:n]*DEST[n+63:n] + SRC3[n+63:n])
}
IF (VEX.128) THEN
DEST[VLMAX-1:128] <- 0
FI
VFMADD231PD DEST, SRC2, SRC3
IF (VEX.128) THEN
  MAXVL =2
ELSEIF (VEX.256)
  MAXVL = 4
FI
For i = 0 to MAXVL-1 {
  n = 64*i;
  DEST[n+63:n] <- RoundFPControl_MXCSR(SRC2[n+63:n]*SRC3[n+63:n] + DEST[n+63:n])
}
IF (VEX.128) THEN
DEST[VLMAX-1:128] <- 0
FI

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
VFMADD132PD: __m128d _mm_fmadd_pd (__m128d a, __m128d b, __m128d c);

VFMADD213PD: __m128d _mm_fmadd_pd (__m128d a, __m128d b, __m128d c);

VFMADD231PD: __m128d _mm_fmadd_pd (__m128d a, __m128d b, __m128d c);

VFMADD132PD: __m256d _mm256_fmadd_pd (__m256d a, __m256d b, __m256d c);

VFMADD213PD: __m256d _mm256_fmadd_pd (__m256d a, __m256d b, __m256d c);

VFMADD231PD: __m256d _mm256_fmadd_pd (__m256d a, __m256d b, __m256d c);


```

 Opcode/Instruction                      | Op/En| 64/32bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 VEX.DDS.128.66.0F38.W1 98 /r VFMADD132PD| A    | V/V          | FMA               | Multiply packed double-precision floating-point
 xmm0, xmm1, xmm2/m128                   |      |              |                   | values from xmm0 and xmm2/mem, add to          
                                         |      |              |                   | xmm1 and put result in xmm0.                   
 VEX.DDS.128.66.0F38.W1 A8 /r VFMADD213PD| A    | V/V          | FMA               | Multiply packed double-precision floating-point
 xmm0, xmm1, xmm2/m128                   |      |              |                   | values from xmm0 and xmm1, add to xmm2/mem     
                                         |      |              |                   | and put result in xmm0.                        
 VEX.DDS.128.66.0F38.W1 B8 /r VFMADD231PD| A    | V/V          | FMA               | Multiply packed double-precision floating-point
 xmm0, xmm1, xmm2/m128                   |      |              |                   | values from xmm1 and xmm2/mem, add to          
                                         |      |              |                   | xmm0 and put result in xmm0.                   
 VEX.DDS.256.66.0F38.W1 98 /r VFMADD132PD| A    | V/V          | FMA               | Multiply packed double-precision floating-point
 ymm0, ymm1, ymm2/m256                   |      |              |                   | values from ymm0 and ymm2/mem, add to          
                                         |      |              |                   | ymm1 and put result in ymm0.                   
 VEX.DDS.256.66.0F38.W1 A8 /r VFMADD213PD| A    | V/V          | FMA               | Multiply packed double-precision floating-point
 ymm0, ymm1, ymm2/m256                   |      |              |                   | values from ymm0 and ymm1, add to ymm2/mem     
                                         |      |              |                   | and put result in ymm0.                        
 VEX.DDS.256.66.0F38.W1 B8 /r VFMADD231PD| A    | V/V          | FMA               | Multiply packed double-precision floating-point
 ymm0, ymm1, ymm2/m256                   |      |              |                   | values from ymm1 and ymm2/mem, add to          
                                         |      |              |                   | ymm0 and put result in ymm0.                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 A    | ModRM:reg (r, w)| VEX.vvvv (r)| ModRM:r/m (r)| NA       

### Description
Performs a set of SIMD multiply-add computation on packed double-precision floating-point
values using three source operands and writes the multiply-add results in the
destination operand. The destination operand is also the first source operand.
The second operand must be a SIMD register. The third source operand can be
a SIMD register or a memory location. VFMADD132PD: Multiplies the two or four
packed double-precision floating-point values from the first source operand
to the two or four packed double-precision floating-point values in the third
source operand, adds the infinite precision intermediate result to the two or
four packed double-precision floating-point values in the second source operand,
performs rounding and stores the resulting two or four packed double-precision
### floating-point values to the destination operand (first source operand). VFMADD213PD
Multiplies the two or four packed double-precision floating-point values from
the second source operand to the two or four packed double-precision floating-point
values in the first source operand, adds the infinite precision intermediate
result to the two or four packed double-precision floating-point values in the
third source operand, performs rounding and stores the resulting two or four
packed double-precision floating-point values to the destination operand (first
source operand). VFMADD231PD: Multiplies the two or four packed double-precision
floating-point values from the second source to the two or four packed double-precision
floating-point values in the third source operand, adds the infinite precision
intermediate result to the two or four packed double-precision floating-point
values in the first source operand, performs rounding and stores the resulting
two or four packed double-precision floating-point values to the destination
operand (first source operand). VEX.128 encoded version: The destination operand
(also first source operand) is a XMM register and encoded in reg_field. The
second source operand is a XMM register and encoded in VEX.vvvv. The third source
operand is a XMM register or a 128-bit memory location and encoded in rm_field.
The upper 128 bits of the YMM destination register are zeroed.

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



### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal


### Other Exceptions
See Exceptions Type 2
