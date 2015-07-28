## VPSRAVD  -  Variable Bit Shift Right Arithmetic

> Operation

``` slim
VPSRAVD (VEX.128 version)
COUNT_0 <- SRC2[31: 0]
  (\* Repeat Each COUNT_i for the 2nd through 4th dwords of SRC2\*)
COUNT_3 <- SRC2[127 : 112];
IF COUNT_0 < 32 THEN
  DEST[31:0] <- SignExtend(SRC1[31:0] >> COUNT_0);
ELSE
  For (i = 0 to 31) DEST[i + 0] <- (SRC1[31] );
FI;
  (\* Repeat shift operation for 2nd through 4th dwords \*)
IF COUNT_3 < 32 THEN
  DEST[127:96] <- SignExtend(SRC1[127:96] >> COUNT_3);
ELSE
  For (i = 0 to 31) DEST[i + 96] <- (SRC1[127] );
FI;
DEST[VLMAX-1:128] <- 0;
VPSRAVD (VEX.256 version)
COUNT_0 <- SRC2[31 : 0];
  (\* Repeat Each COUNT_i for the 2nd through 7th dwords of SRC2\*)
COUNT_7 <- SRC2[255 : 224];
IF COUNT_0 < 32 THEN
  DEST[31:0] <- SignExtend(SRC1[31:0] >> COUNT_0);
ELSE
  For (i = 0 to 31) DEST[i + 0] <- (SRC1[31] );
FI;
  (\* Repeat shift operation for 2nd through 7th dwords \*)
IF COUNT_7 < 32 THEN
  DEST[255:224] <- SignExtend(SRC1[255:224] >> COUNT_7);
ELSE
  For (i = 0 to 31) DEST[i + 224] <- (SRC1[255] );
FI;

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
VPSRAVD: __m256i _mm256_srav_epi32 (__m256i m, __m256i count)

VPSRAVD: __m128i _mm_srav_epi32 (__m128i m, __m128i count)


```

 Opcode/Instruction                  | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 VEX.NDS.128.66.0F38.W0 46 /r VPSRAVD| RVM  | V/V            | AVX2              | Shift bits in doublewords in xmm2 right 
 xmm1, xmm2, xmm3/m128               |      |                |                   | by amount specified in the corresponding
                                     |      |                |                   | element of xmm3/m128 while shifting     
                                     |      |                |                   | in the sign bits.                       
 VEX.NDS.256.66.0F38.W0 46 /r VPSRAVD| RVM  | V/V            | AVX2              | Shift bits in doublewords in ymm2 right 
 ymm1, ymm2, ymm3/m256               |      |                |                   | by amount specified in the corresponding
                                     |      |                |                   | element of ymm3/m256 while shifting     
                                     |      |                |                   | in the sign bits.                       

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVM  | ModRM:reg (w)| VEX.vvvv | ModRM:r/m (r)| NA       

### Description
Shifts the bits in the individual doubleword data elements in the first source
operand to the right by the count value of respective data elements in the second
source operand. As the bits in each data element are shifted right, the empty
high-order bits are filled with the sign bit of the source element. The count
values are specified individually in each data element of the second source
operand. If the unsigned integer value specified in the respective data element
of the second source operand is greater than 31, then the destination data element
are filled with the corresponding sign bit of the source element. VEX.128 encoded
version: The destination and first source operands are XMM registers. The count
operand can be either an XMM register or a 128-bit memory location. Bits (VLMAX-1:128)
of the corresponding YMM register are zeroed. VEX.256 encoded version: The destination
and first source operands are YMM registers. The count operand can be either
an YMM register or a 256-bit memory location.



### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.W = 1.
