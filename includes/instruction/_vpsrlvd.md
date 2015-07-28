## VPSRLVD/VPSRLVQ  -  Variable Bit Shift Right Logical

> Operation

``` slim
VPSRLVD (VEX.128 version)
COUNT_0 <- SRC2[31 : 0]
  (\* Repeat Each COUNT_i for the 2nd through 4th dwords of SRC2\*)
COUNT_3 <- SRC2[127 : 96];
IF COUNT_0 < 32 THEN
DEST[31:0] <- ZeroExtend(SRC1[31:0] >> COUNT_0);
ELSE
DEST[31:0] <- 0;
  (\* Repeat shift operation for 2nd through 4th dwords \*)
IF COUNT_3 < 32 THEN
DEST[127:96] <- ZeroExtend(SRC1[127:96] >> COUNT_3);
ELSE
DEST[127:96] <- 0;
DEST[VLMAX-1:128] <- 0;
VPSRLVD (VEX.256 version)
COUNT_0 <- SRC2[31 : 0];
  (\* Repeat Each COUNT_i for the 2nd through 7th dwords of SRC2\*)
COUNT_7 <- SRC2[255 : 224];
IF COUNT_0 < 32 THEN
DEST[31:0] <- ZeroExtend(SRC1[31:0] >> COUNT_0);
ELSE
DEST[31:0] <- 0;
  (\* Repeat shift operation for 2nd through 7th dwords \*)
IF COUNT_7 < 32 THEN
DEST[255:224] <- ZeroExtend(SRC1[255:224] >> COUNT_7);
ELSE
DEST[255:224] <- 0;
VPSRLVQ (VEX.128 version)
COUNT_0 <- SRC2[63 : 0];
COUNT_1 <- SRC2[127 : 64];
IF COUNT_0 < 64 THEN
DEST[63:0] <- ZeroExtend(SRC1[63:0] >> COUNT_0);
ELSE
DEST[63:0] <- 0;
IF COUNT_1 < 64THEN
DEST[127:64] <- ZeroExtend(SRC1[127:64] >> COUNT_1);
ELSE
DEST[127:64] <- 0;
DEST[VLMAX-1:128] <- 0;
VPSRLVQ (VEX.256 version)
COUNT_0 <- SRC2[63 : 0];
  (\* Repeat Each COUNT_i for the 2nd through 4th dwords of SRC2\*)
COUNT_3 <- SRC2[255 : 192];
IF COUNT_0 < 64 THEN
DEST[63:0] <- ZeroExtend(SRC1[63:0] >> COUNT_0);
ELSE
DEST[63:0] <- 0;
  (\* Repeat shift operation for 2nd through 4th dwords \*)
IF COUNT_3 < 64THEN
DEST[255:192] <- ZeroExtend(SRC1[255:192] >> COUNT_3);
ELSE
DEST[255:192] <- 0;

```

 Opcode/Instruction                  | Op/EN| 64/32 -bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 VEX.NDS.128.66.0F38.W0 45 /r VPSRLVD| RVM  | V/V            | AVX2              | Shift bits in doublewords in xmm2 right 
 xmm1, xmm2, xmm3/m128               |      |                |                   | by amount specified in the corresponding
                                     |      |                |                   | element of xmm3/m128 while shifting     
                                     |      |                |                   | in 0s.                                  
 VEX.NDS.128.66.0F38.W1 45 /r VPSRLVQ| RVM  | V/V            | AVX2              | Shift bits in quadwords in xmm2 right   
 xmm1, xmm2, xmm3/m128               |      |                |                   | by amount specified in the corresponding
                                     |      |                |                   | element of xmm3/m128 while shifting     
                                     |      |                |                   | in 0s.                                  
 VEX.NDS.256.66.0F38.W0 45 /r VPSRLVD| RVM  | V/V            | AVX2              | Shift bits in doublewords in ymm2 right 
 ymm1, ymm2, ymm3/m256               |      |                |                   | by amount specified in the corresponding
                                     |      |                |                   | element of ymm3/m256 while shifting     
                                     |      |                |                   | in 0s.                                  
 VEX.NDS.256.66.0F38.W1 45 /r VPSRLVQ| RVM  | V/V            | AVX2              | Shift bits in quadwords in ymm2 right   
 ymm1, ymm2, ymm3/m256               |      |                |                   | by amount specified in the corresponding
                                     |      |                |                   | element of ymm3/m256 while shifting     
                                     |      |                |                   | in 0s.                                  

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVM  | ModRM:reg (w)| VEX.vvvv | ModRM:r/m (r)| NA       

### Description
Shifts the bits in the individual data elements (doublewords, or quadword) in
the first source operand to the right by the count value of respective data
elements in the second source operand. As the bits in the data elements are
shifted right, the empty high-order bits are cleared (set to 0). The count values
are specified individually in each data element of the second source operand.
If the unsigned integer value specified in the respective data element of the
second source operand is greater than 31 (for doublewords), or 63 (for a quadword),
### then the destination data element are written with 0. VEX.128 encoded version
The destination and first source operands are XMM registers. The count operand
can be either an XMM register or a 128-bit memory location. Bits (VLMAX-1:128)
of the corresponding YMM register are zeroed. VEX.256 encoded version: The destination
and first source operands are YMM registers. The count operand can be either
an YMM register or a 256-bit memory location.



### Intel C/C++ Compiler Intrinsic Equivalent
VPSRLVD: __m256i _mm256_srlv_epi32 (__m256i m, __m256i count);

VPSRLVD: __m128i _mm_srlv_epi32 (__m128i m, __m128i count);

VPSRLVQ: __m256i _mm256_srlv_epi64 (__m256i m, __m256i count);

VPSRLVQ: __m128i _mm_srlv_epi64 (__m128i m, __m128i count);


### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 4
