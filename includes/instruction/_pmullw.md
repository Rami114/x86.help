## PMULLW - Multiply Packed Signed Integers and Store Low Result

> Operation

``` slim
PMULLW (with 64-bit operands)
  TEMP0[31:0] <-
  TEMP1[31:0] <-
  TEMP2[31:0] <-
  TEMP3[31:0] <-
  DEST[15:0] <-
  DEST[31:16] <-
  DEST[47:32] <-
  DEST[63:48] <-
PMULLW (with 128-bit operands)
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
  DEST[127:112] <- TEMP7[15:0];
VPMULLW (VEX.128 encoded version)
Temp0[31:0] <- SRC1[15:0] * SRC2[15:0]
Temp1[31:0] <- SRC1[31:16] * SRC2[31:16]
Temp2[31:0] <- SRC1[47:32] * SRC2[47:32]
Temp3[31:0] <- SRC1[63:48] * SRC2[63:48]
Temp4[31:0] <- SRC1[79:64] * SRC2[79:64]
Temp5[31:0] <- SRC1[95:80] * SRC2[95:80]
Temp6[31:0] <- SRC1[111:96] * SRC2[111:96]
Temp7[31:0] <- SRC1[127:112] * SRC2[127:112]
DEST[15:0] <- Temp0[15:0]
DEST[31:16] <- Temp1[15:0]
DEST[47:32] <- Temp2[15:0]
DEST[63:48] <- Temp3[15:0]
DEST[79:64] <- Temp4[15:0]
DEST[95:80] <- Temp5[15:0]
DEST[111:96] <- Temp6[15:0]
DEST[127:112] <- Temp7[15:0]
DEST[VLMAX-1:128] <- 0
VPMULLD (VEX.256 encoded version)
Temp0[63:0] <- SRC1[31:0] * SRC2[31:0]
Temp1[63:0] <- SRC1[63:32] * SRC2[63:32]
Temp2[63:0] <- SRC1[95:64] * SRC2[95:64]
Temp3[63:0] <- SRC1[127:96] * SRC2[127:96]
Temp4[63:0] <- SRC1[159:128] * SRC2[159:128]
Temp5[63:0] <- SRC1[191:160] * SRC2[191:160]
Temp6[63:0] <- SRC1[223:192] * SRC2[223:192]
Temp7[63:0] <- SRC1[255:224] * SRC2[255:224]
DEST[31:0] <- Temp0[31:0]
DEST[63:32] <- Temp1[31:0]
DEST[95:64] <- Temp2[31:0]
DEST[127:96] <- Temp3[31:0]
DEST[159:128] <- Temp4[31:0]
DEST[191:160] <- Temp5[31:0]
DEST[223:192] <- Temp6[31:0]
DEST[255:224] <- Temp7[31:0]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PMULLW:   | __m64 _mm_mullo_pi16(__m64 m1, __m64
           | m2)                                 
 (V)PMULLW:| __m128i _mm_mullo_epi16 ( __m128i a,
           | __m128i b)                          
 VPMULLW:  | __m256i _mm256_mullo_epi16 ( __m256i
           | a, __m256i b);                      

```

 Opcode/Instruction                 | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 0F D5 /r1 PMULLW mm, mm/m64        | RM   | V/V                   | MMX               | Multiply the packed signed word integers 
                                    |      |                       |                   | in mm1 register and mm2/m64, and store   
                                    |      |                       |                   | the low 16 bits of the results in mm1.   
 66 0F D5 /r PMULLW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Multiply the packed signed word integers 
                                    |      |                       |                   | in xmm1 and xmm2/m128, and store the     
                                    |      |                       |                   | low 16 bits of the results in xmm1.      
 VEX.NDS.128.66.0F.WIG D5 /r VPMULLW| RVM  | V/V                   | AVX               | Multiply the packed dword signed integers
 xmm1, xmm2, xmm3/m128              |      |                       |                   | in xmm2 and xmm3/m128 and store the      
                                    |      |                       |                   | low 32 bits of each product in xmm1.     
 VEX.NDS.256.66.0F.WIG D5 /r VPMULLW| RVM  | V/V                   | AVX2              | Multiply the packed signed word integers 
 ymm1, ymm2, ymm3/m256              |      |                       |                   | in ymm2 and ymm3/m256, and store the     
                                    |      |                       |                   | low 16 bits of the results in ymm1.      
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
the low 16 bits of each intermediate 32-bit result in the destination operand.
(Figure 4-8 shows this operation when using 64-bit operands.)

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
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

   | |  
---- | -----
 SRC                                    | X3             | X2      | X1             | X0                 
 DEST                                   | Y3 Z2 = X2 * Y2| Y2      | Y1 Z1 = X1 * Y1| Y0 Z0 = X0 * Y0    
 DEST PMULLU Instruction Operation Using| Z3[15:0]       | Z2[15:0]| Z1[15:0]       | Z0[15:0]Figure 4-9.
 64-bit Operands                        |                |         |                |                    


### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
