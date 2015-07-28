## PMULHRSW  -  Packed Multiply High with Round and Scale

> Operation

``` slim
PMULHRSW (with 64-bit operands)
  temp0[31:0] = INT32 ((DEST[15:0] * SRC[15:0]) >>14) + 1;
  temp1[31:0] = INT32 ((DEST[31:16] * SRC[31:16]) >>14) + 1;
  temp2[31:0] = INT32 ((DEST[47:32] * SRC[47:32]) >> 14) + 1;
  temp3[31:0] = INT32 ((DEST[63:48] * SRc[63:48]) >> 14) + 1;
  DEST[15:0] = temp0[16:1];
  DEST[31:16] = temp1[16:1];
  DEST[47:32] = temp2[16:1];
  DEST[63:48] = temp3[16:1];
PMULHRSW (with 128-bit operand)
  temp0[31:0] = INT32 ((DEST[15:0] * SRC[15:0]) >>14) + 1;
  temp1[31:0] = INT32 ((DEST[31:16] * SRC[31:16]) >>14) + 1;
  temp2[31:0] = INT32 ((DEST[47:32] * SRC[47:32]) >>14) + 1;
  temp3[31:0] = INT32 ((DEST[63:48] * SRC[63:48]) >>14) + 1;
  temp4[31:0] = INT32 ((DEST[79:64] * SRC[79:64]) >>14) + 1;
  temp5[31:0] = INT32 ((DEST[95:80] * SRC[95:80]) >>14) + 1;
  temp6[31:0] = INT32 ((DEST[111:96] * SRC[111:96]) >>14) + 1;
  temp7[31:0] = INT32 ((DEST[127:112] * SRC[127:112) >>14) + 1;
  DEST[15:0] = temp0[16:1];
  DEST[31:16] = temp1[16:1];
  DEST[47:32] = temp2[16:1];
  DEST[63:48] = temp3[16:1];
  DEST[79:64] = temp4[16:1];
  DEST[95:80] = temp5[16:1];
  DEST[111:96] = temp6[16:1];
  DEST[127:112] = temp7[16:1];
VPMULHRSW (VEX.128 encoded version)
temp0[31:0] <- INT32 ((SRC1[15:0] * SRC2[15:0]) >>14) + 1
temp1[31:0] <- INT32 ((SRC1[31:16] * SRC2[31:16]) >>14) + 1
temp2[31:0] <- INT32 ((SRC1[47:32] * SRC2[47:32]) >>14) + 1
temp3[31:0] <- INT32 ((SRC1[63:48] * SRC2[63:48]) >>14) + 1
temp4[31:0] <- INT32 ((SRC1[79:64] * SRC2[79:64]) >>14) + 1
temp5[31:0] <- INT32 ((SRC1[95:80] * SRC2[95:80]) >>14) + 1
temp6[31:0] <- INT32 ((SRC1[111:96] * SRC2[111:96]) >>14) + 1
temp7[31:0] <- INT32 ((SRC1[127:112] * SRC2[127:112) >>14) + 1
DEST[15:0] <- temp0[16:1]
DEST[31:16] <- temp1[16:1]
DEST[47:32] <- temp2[16:1]
DEST[63:48] <- temp3[16:1]
DEST[79:64] <- temp4[16:1]
DEST[95:80] <- temp5[16:1]
DEST[111:96] <- temp6[16:1]
DEST[127:112] <- temp7[16:1]
DEST[VLMAX-1:128] <- 0
VPMULHRSW (VEX.256 encoded version)
temp0[31:0] <- INT32 ((SRC1[15:0] * SRC2[15:0]) >>14) + 1
temp1[31:0] <- INT32 ((SRC1[31:16] * SRC2[31:16]) >>14) + 1
temp2[31:0] <- INT32 ((SRC1[47:32] * SRC2[47:32]) >>14) + 1
temp3[31:0] <- INT32 ((SRC1[63:48] * SRC2[63:48]) >>14) + 1
temp4[31:0] <- INT32 ((SRC1[79:64] * SRC2[79:64]) >>14) + 1
temp5[31:0] <- INT32 ((SRC1[95:80] * SRC2[95:80]) >>14) + 1
temp6[31:0] <- INT32 ((SRC1[111:96] * SRC2[111:96]) >>14) + 1
temp7[31:0] <- INT32 ((SRC1[127:112] * SRC2[127:112) >>14) + 1
temp8[31:0] <- INT32 ((SRC1[143:128] * SRC2[143:128]) >>14) + 1
temp9[31:0] <- INT32 ((SRC1[159:144] * SRC2[159:144]) >>14) + 1
temp10[31:0] <- INT32 ((SRC1[75:160] * SRC2[175:160]) >>14) + 1
temp11[31:0] <- INT32 ((SRC1[191:176] * SRC2[191:176]) >>14) + 1
temp12[31:0] <- INT32 ((SRC1[207:192] * SRC2[207:192]) >>14) + 1
temp13[31:0] <- INT32 ((SRC1[223:208] * SRC2[223:208]) >>14) + 1
temp14[31:0] <- INT32 ((SRC1[239:224] * SRC2[239:224]) >>14) + 1
temp15[31:0] <- INT32 ((SRC1[255:240] * SRC2[255:240) >>14) + 1

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PMULHRSW:   | __m64 _mm_mulhrs_pi16 (__m64 a, __m64
             | b)                                   
 (V)PMULHRSW:| __m128i _mm_mulhrs_epi16 (__m128i a, 
             | __m128i b)                           
 VPMULHRSW:  | __m256i _mm256_mulhrs_epi16 (__m256i 
             | a, __m256i b)                        

```

 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 0F 38 0B /r1 PMULHRSW mm1, mm2/m64     | RM   | V/V                   | SSSE3             | Multiply 16-bit signed words, scale    
                                        |      |                       |                   | and round signed doublewords, pack high
                                        |      |                       |                   | 16 bits to mm1.                        
 66 0F 38 0B /r PMULHRSW xmm1, xmm2/m128| RM   | V/V                   | SSSE3             | Multiply 16-bit signed words, scale    
                                        |      |                       |                   | and round signed doublewords, pack high
                                        |      |                       |                   | 16 bits to xmm1.                       
 VEX.NDS.128.66.0F38.WIG 0B /r VPMULHRSW| RVM  | V/V                   | AVX               | Multiply 16-bit signed words, scale    
 xmm1, xmm2, xmm3/m128                  |      |                       |                   | and round signed doublewords, pack high
                                        |      |                       |                   | 16 bits to xmm1.                       
 VEX.NDS.256.66.0F38.WIG 0B /r VPMULHRSW| RVM  | V/V                   | AVX2              | Multiply 16-bit signed words, scale    
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | and round signed doublewords, pack high
                                        |      |                       |                   | 16 bits to ymm1.                       
<aside class="notification">
1. See note in Section 2.4, “Instruction Exception Specification” in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 2A
and Section 22.25.3, “Exception Conditions of Legacy SIMD Instructions Operating
on MMX Registers” in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3A.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
PMULHRSW multiplies vertically each signed 16-bit integer from the destination
operand (first operand) with the corresponding signed 16-bit integer of the
source operand (second operand), producing intermediate, signed 32bit integers.
Each intermediate 32-bit integer is truncated to the 18 most significant bits.
Rounding is always performed by adding 1 to the least significant bit of the
18-bit intermediate result. The final result is obtained by selecting the 16
bits immediately to the right of the most significant bit of each 18-bit intermediate
result and packed to the destination operand.

When the source operand is a 128-bit memory operand, the operand must be aligned
on a 16-byte boundary or a general-protection exception (**``#GP)``** will be generated.

In 64-bit mode, use the REX prefix to access additional registers. Legacy SSE
version: Both operands can be MMX registers. The second source operand is an
MMX register or a 64bit memory location.

128-bit Legacy SSE version: The first source and destination operands are XMM
registers. The second source operand is an XMM register or a 128-bit memory
location. Bits (VLMAX-1:128) of the corresponding YMM destination register remain
unchanged. VEX.128 encoded version: The first source and destination operands
are XMM registers. The second source operand is an XMM register or a 128-bit
memory location. Bits (VLMAX-1:128) of the destination YMM register are zeroed.
### VEX.L must be 0, otherwise the instruction will #UD. VEX.256 encoded version
The second source operand can be an YMM register or a 256-bit memory location.
The first source and destination operands are YMM registers.



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
