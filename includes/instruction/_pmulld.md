## PMULLD  -  Multiply Packed Signed Dword Integers and Store Low Result

> Operation

``` slim
Temp0[63:0] <- DEST[31:0] \* SRC[31:0];
Temp1[63:0] <- DEST[63:32] \* SRC[63:32];
Temp2[63:0] <- DEST[95:64] \* SRC[95:64];
Temp3[63:0] <- DEST[127:96] \* SRC[127:96];
DEST[31:0] <- Temp0[31:0];
DEST[63:32] <- Temp1[31:0];
DEST[95:64] <- Temp2[31:0];
DEST[127:96] <- Temp3[31:0];
VPMULLD (VEX.128 encoded version)
Temp0[63:0] <- SRC1[31:0] \* SRC2[31:0]
Temp1[63:0] <- SRC1[63:32] \* SRC2[63:32]
Temp2[63:0] <- SRC1[95:64] \* SRC2[95:64]
Temp3[63:0] <- SRC1[127:96] \* SRC2[127:96]
DEST[31:0] <- Temp0[31:0]
DEST[63:32] <- Temp1[31:0]
DEST[95:64] <- Temp2[31:0]
DEST[127:96] <- Temp3[31:0]
DEST[VLMAX-1:128] <- 0
VPMULLD (VEX.256 encoded version)
Temp0[63:0] <- SRC1[31:0] \* SRC2[31:0]
Temp1[63:0] <- SRC1[63:32] \* SRC2[63:32]
Temp2[63:0] <- SRC1[95:64] \* SRC2[95:64]
Temp3[63:0] <- SRC1[127:96] \* SRC2[127:96]
Temp4[63:0] <- SRC1[159:128] \* SRC2[159:128]
Temp5[63:0] <- SRC1[191:160] \* SRC2[191:160]
Temp6[63:0] <- SRC1[223:192] \* SRC2[223:192]
Temp7[63:0] <- SRC1[255:224] \* SRC2[255:224]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 (V)PMULLUD:| __m128i _mm_mullo_epi32(__m128i a, __m128i
            | b);                                       
 VPMULLD:   | __m256i _mm256_mullo_epi32(__m256i a,     
            | __m256i b);                               

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 66 0F 38 40 /r PMULLD xmm1, xmm2/m128| RM   | V/V                   | SSE4_1            | Multiply the packed dword signed integers
                                      |      |                       |                   | in xmm1 and xmm2/m128 and store the      
                                      |      |                       |                   | low 32 bits of each product in xmm1.     
 VEX.NDS.128.66.0F38.WIG 40 /r VPMULLD| RVM  | V/V                   | AVX               | Multiply the packed dword signed integers
 xmm1, xmm2, xmm3/m128                |      |                       |                   | in xmm2 and xmm3/m128 and store the      
                                      |      |                       |                   | low 32 bits of each product in xmm1.     
 VEX.NDS.256.66.0F38.WIG 40 /r VPMULLD| RVM  | V/V                   | AVX2              | Multiply the packed dword signed integers
 ymm1, ymm2, ymm3/m256                |      |                       |                   | in ymm2 and ymm3/m256 and store the      
                                      |      |                       |                   | low 32 bits of each product in ymm1.     

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs four signed multiplications from four pairs of signed dword integers
and stores the lower 32 bits of the four 64-bit products in the destination
operand (first operand). Each dword element in the destination operand is multiplied
with the corresponding dword element of the source operand (second operand)
to obtain a 64-bit intermediate product. 128-bit Legacy SSE version: The first
source and destination operands are XMM registers. The second source operand
is an XMM register or a 128-bit memory location. Bits (VLMAX-1:128) of the corresponding
YMM destination register remain unchanged. VEX.128 encoded version: The first
source and destination operands are XMM registers. The second source operand
is an XMM register or a 128-bit memory location. Bits (VLMAX-1:128) of the destination
YMM register are zeroed. VEX.256 encoded version: The second source operand
can be an YMM register or a 256-bit memory location. The first source and destination
operands are YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
