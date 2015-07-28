## PMULHW - Multiply Packed Signed Integers and Store High Result

> Operation
``` slim

PMULHW (with 64-bit operands)
  TEMP0[31:0] <-
  TEMP1[31:0] <-
  TEMP2[31:0] <-
  TEMP3[31:0] <-
  DEST[15:0] <-
  DEST[31:16] <-
  DEST[47:32] <-
  DEST[63:48] <-
PMULHW (with 128-bit operands)
  TEMP0[31:0] <-
  TEMP1[31:0] <-
  TEMP2[31:0] <-
  TEMP3[31:0] <-
  TEMP4[31:0] <-
  TEMP5[31:0] <-
  TEMP6[31:0] <-
  TEMP7[31:0] <-
  DEST[15:0] <-
  DEST[31:16] <-
  DEST[47:32] <-
  DEST[63:48] <-
  DEST[79:64] <-
  DEST[95:80] <-
  DEST[111:96] <-
  DEST[127:112] <- TEMP7[31:16];
VPMULHW (VEX.128 encoded version)
TEMP0[31:0] <- SRC1[15:0] \* SRC2[15:0] (\*Signed Multiplication\*)
TEMP1[31:0] <- SRC1[31:16] \* SRC2[31:16]
TEMP2[31:0] <- SRC1[47:32] \* SRC2[47:32]
TEMP3[31:0] <- SRC1[63:48] \* SRC2[63:48]
TEMP4[31:0] <- SRC1[79:64] \* SRC2[79:64]
TEMP5[31:0] <- SRC1[95:80] \* SRC2[95:80]
TEMP6[31:0] <- SRC1[111:96] \* SRC2[111:96]
TEMP7[31:0] <- SRC1[127:112] \* SRC2[127:112]
DEST[15:0] <- TEMP0[31:16]
DEST[31:16] <- TEMP1[31:16]
DEST[47:32] <- TEMP2[31:16]
DEST[63:48] <- TEMP3[31:16]
DEST[79:64] <- TEMP4[31:16]
DEST[95:80] <- TEMP5[31:16]
DEST[111:96] <- TEMP6[31:16]
DEST[127:112] <- TEMP7[31:16]
DEST[VLMAX-1:128] <- 0
PMULHW (VEX.256 encoded version)
TEMP0[31:0] <- SRC1[15:0] \* SRC2[15:0] (\*Signed Multiplication\*)
TEMP1[31:0] <- SRC1[31:16] \* SRC2[31:16]
TEMP2[31:0] <- SRC1[47:32] \* SRC2[47:32]
TEMP3[31:0] <- SRC1[63:48] \* SRC2[63:48]
TEMP4[31:0] <- SRC1[79:64] \* SRC2[79:64]
TEMP5[31:0] <- SRC1[95:80] \* SRC2[95:80]
TEMP6[31:0] <- SRC1[111:96] \* SRC2[111:96]
TEMP7[31:0] <- SRC1[127:112] \* SRC2[127:112]
TEMP8[31:0] <- SRC1[143:128] \* SRC2[143:128]
TEMP9[31:0] <- SRC1[159:144] \* SRC2[159:144]
TEMP10[31:0] <- SRC1[175:160] \* SRC2[175:160]
TEMP11[31:0] <- SRC1[191:176] \* SRC2[191:176]
TEMP12[31:0] <- SRC1[207:192] \* SRC2[207:192]
TEMP13[31:0] <- SRC1[223:208] \* SRC2[223:208]
TEMP14[31:0] <- SRC1[239:224] \* SRC2[239:224]
TEMP15[31:0] <- SRC1[255:240] \* SRC2[255:240]
DEST[15:0] <- TEMP0[31:16]
DEST[31:16] <- TEMP1[31:16]
DEST[47:32] <- TEMP2[31:16]
DEST[63:48] <- TEMP3[31:16]
DEST[79:64] <- TEMP4[31:16]
DEST[95:80] <- TEMP5[31:16]
DEST[111:96] <- TEMP6[31:16]
DEST[127:112] <- TEMP7[31:16]
DEST[143:128] <- TEMP8[31:16]
DEST[159:144] <- TEMP9[31:16]
DEST[175:160] <- TEMP10[31:16]
DEST[191:176] <- TEMP11[31:16]
DEST[207:192] <- TEMP12[31:16]
DEST[223:208] <- TEMP13[31:16]
DEST[239:224] <- TEMP14[31:16]
DEST[255:240] <- TEMP15[31:16]

```

 Opcode/Instruction                 | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F E5 /r1 PMULHW mm, mm/m64        | RM   | V/V                   | MMX               | Multiply the packed signed word integers
                                    |      |                       |                   | in mm1 register and mm2/m64, and store  
                                    |      |                       |                   | the high 16 bits of the results in mm1. 
 66 0F E5 /r PMULHW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Multiply the packed signed word integers
                                    |      |                       |                   | in xmm1 and xmm2/m128, and store the    
                                    |      |                       |                   | high 16 bits of the results in xmm1.    
 VEX.NDS.128.66.0F.WIG E5 /r VPMULHW| RVM  | V/V                   | AVX               | Multiply the packed signed word integers
 xmm1, xmm2, xmm3/m128              |      |                       |                   | in xmm2 and xmm3/m128, and store the    
                                    |      |                       |                   | high 16 bits of the results in xmm1.    
 VEX.NDS.256.66.0F.WIG E5 /r VPMULHW| RVM  | V/V                   | AVX2              | Multiply the packed signed word integers
 ymm1, ymm2, ymm3/m256              |      |                       |                   | in ymm2 and ymm3/m256, and store the    
                                    |      |                       |                   | high 16 bits of the results in ymm1.    
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
Performs a SIMD signed multiply of the packed signed word integers in the destination
operand (first operand) and the source operand (second operand), and stores
the high 16 bits of each intermediate 32-bit result in the destination operand.
(Figure 4-8 shows this operation when using 64-bit operands.)

n 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). Legacy SSE version: The source
operand can be an MMX technology register or a 64-bit memory location. The destination
operand is an MMX technology register.

128-bit Legacy SSE version: The first source and destination operands are XMM
registers. The second source operand is an XMM register or a 128-bit memory
location. Bits (VLMAX-1:128) of the corresponding YMM destination register remain
unchanged. VEX.128 encoded version: The first source and destination operands
are XMM registers. The second source operand is an XMM register or a 128-bit
memory location. Bits (VLMAX-1:128) of the destination YMM register are zeroed.
### VEX.L must be 0, otherwise the instruction will #UD. VEX.256 encoded version
The second source operand can be an YMM register or a 256-bit memory location.
The first source and destination operands are YMM registers.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PMULHW:   | __m64 _mm_mulhi_pi16 (__m64 m1, __m64
           | m2)                                  
 (V)PMULHW:| __m128i _mm_mulhi_epi16 ( __m128i a, 
           | __m128i b)                           
 VPMULHW:  | __m256i _mm256_mulhi_epi16 ( __m256i 
           | a, __m256i b)                        

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
