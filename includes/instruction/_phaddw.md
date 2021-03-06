## PHADDW/PHADDD  -  Packed Horizontal Add

> Operation

``` slim
PHADDW (with 64-bit operands)
  mm1[15-0]
  mm1[31-16] = mm1[63-48] + mm1[47-32];
  mm1[47-32] = mm2/m64[31-16] + mm2/m64[15-0];
  mm1[63-48] = mm2/m64[63-48] + mm2/m64[47-32];
PHADDW (with 128-bit operands)
  xmm1[15-0] = xmm1[31-16] + xmm1[15-0];
  xmm1[31-16] = xmm1[63-48] + xmm1[47-32];
  xmm1[47-32] = xmm1[95-80] + xmm1[79-64];
  xmm1[63-48] = xmm1[127-112] + xmm1[111-96];
  xmm1[79-64] = xmm2/m128[31-16] + xmm2/m128[15-0];
  xmm1[95-80] = xmm2/m128[63-48] + xmm2/m128[47-32];
  xmm1[111-96] = xmm2/m128[95-80] + xmm2/m128[79-64];
  xmm1[127-112] = xmm2/m128[127-112] + xmm2/m128[111-96];
VPHADDW (VEX.128 encoded version)
DEST[15:0] <- SRC1[31:16] + SRC1[15:0]
DEST[31:16] <- SRC1[63:48] + SRC1[47:32]
DEST[47:32] <- SRC1[95:80] + SRC1[79:64]
DEST[63:48] <- SRC1[127:112] + SRC1[111:96]
DEST[79:64] <- SRC2[31:16] + SRC2[15:0]
DEST[95:80] <- SRC2[63:48] + SRC2[47:32]
DEST[111:96] <- SRC2[95:80] + SRC2[79:64]
DEST[127:112] <- SRC2[127:112] + SRC2[111:96]
DEST[VLMAX-1:128] <- 0
VPHADDW (VEX.256 encoded version)
DEST[15:0] <- SRC1[31:16] + SRC1[15:0]
DEST[31:16] <- SRC1[63:48] + SRC1[47:32]
DEST[47:32] <- SRC1[95:80] + SRC1[79:64]
DEST[63:48] <- SRC1[127:112] + SRC1[111:96]
DEST[79:64] <- SRC2[31:16] + SRC2[15:0]
DEST[95:80] <- SRC2[63:48] + SRC2[47:32]
DEST[111:96] <- SRC2[95:80] + SRC2[79:64]
DEST[127:112] <- SRC2[127:112] + SRC2[111:96]
DEST[143:128] <- SRC1[159:144] + SRC1[143:128]
DEST[159:144] <- SRC1[191:176] + SRC1[175:160]
DEST[175:160] <- SRC1[223:208] + SRC1[207:192]
DEST[191:176] <- SRC1[255:240] + SRC1[239:224]
DEST[207:192] <- SRC2[127:112] + SRC2[143:128]
DEST[223:208] <- SRC2[159:144] + SRC2[175:160]
DEST[239:224] <- SRC2[191:176] + SRC2[207:192]
DEST[255:240] <- SRC2[223:208] + SRC2[239:224]
PHADDD (with 64-bit operands)
  mm1[31-0]
  mm1[63-32] = mm2/m64[63-32] + mm2/m64[31-0];
PHADDD (with 128-bit operands)
  xmm1[31-0] = xmm1[63-32] + xmm1[31-0];
  xmm1[63-32] = xmm1[127-96] + xmm1[95-64];
  xmm1[95-64] = xmm2/m128[63-32] + xmm2/m128[31-0];
  xmm1[127-96] = xmm2/m128[127-96] + xmm2/m128[95-64];
VPHADDD (VEX.128 encoded version)
DEST[31-0] <- SRC1[63-32] + SRC1[31-0]
DEST[63-32] <- SRC1[127-96] + SRC1[95-64]
DEST[95-64] <- SRC2[63-32] + SRC2[31-0]
DEST[127-96] <- SRC2[127-96] + SRC2[95-64]
DEST[VLMAX-1:128] <- 0
VPHADDD (VEX.256 encoded version)
DEST[31-0] <- SRC1[63-32] + SRC1[31-0]
DEST[63-32] <- SRC1[127-96] + SRC1[95-64]
DEST[95-64] <- SRC2[63-32] + SRC2[31-0]
DEST[127-96] <- SRC2[127-96] + SRC2[95-64]
DEST[159-128] <- SRC1[191-160] + SRC1[159-128]
DEST[191-160] <- SRC1[255-224] + SRC1[223-192]
DEST[223-192] <- SRC2[191-160] + SRC2[159-128]
DEST[255-224] <- SRC2[255-224] + SRC2[223-192]

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PHADDW:   | __m64 _mm_hadd_pi16 (__m64 a, __m64       
           | b)                                        
 PHADDD:   | __m64 _mm_hadd_pi32 (__m64 a, __m64       
           | b)                                        
 (V)PHADDW:| __m128i _mm_hadd_epi16 (__m128i a, __m128i
           | b)                                        
 (V)PHADDD:| __m128i _mm_hadd_epi32 (__m128i a, __m128i
           | b)                                        
 VPHADDW:  | __m256i _mm256_hadd_epi16 (__m256i a,     
           | __m256i b)                                
 VPHADDD:  | __m256i _mm256_hadd_epi32 (__m256i a,     
           | __m256i b)                                

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F 38 01 /r1 PHADDW mm1, mm2/m64     | RM   | V/V                   | SSSE3             | Add 16-bit integers horizontally, pack  
                                      |      |                       |                   | to mm1.                                 
 66 0F 38 01 /r PHADDW xmm1, xmm2/m128| RM   | V/V                   | SSSE3             | Add 16-bit integers horizontally, pack  
                                      |      |                       |                   | to xmm1.                                
 0F 38 02 /r PHADDD mm1, mm2/m64      | RM   | V/V                   | SSSE3             | Add 32-bit integers horizontally, pack  
                                      |      |                       |                   | to mm1.                                 
 66 0F 38 02 /r PHADDD xmm1, xmm2/m128| RM   | V/V                   | SSSE3             | Add 32-bit integers horizontally, pack  
                                      |      |                       |                   | to xmm1.                                
 VEX.NDS.128.66.0F38.WIG 01 /r VPHADDW| RVM  | V/V                   | AVX               | Add 16-bit integers horizontally, pack  
 xmm1, xmm2, xmm3/m128                |      |                       |                   | to xmm1.                                
 VEX.NDS.128.66.0F38.WIG 02 /r VPHADDD| RVM  | V/V                   | AVX               | Add 32-bit integers horizontally, pack  
 xmm1, xmm2, xmm3/m128                |      |                       |                   | to xmm1.                                
 VEX.NDS.256.66.0F38.WIG 01 /r VPHADDW| RVM  | V/V                   | AVX2              | Add 16-bit signed integers horizontally,
 ymm1, ymm2, ymm3/m256                |      |                       |                   | pack to ymm1.                           
 VEX.NDS.256.66.0F38.WIG 02 /r VPHADDD| RVM  | V/V                   | AVX2              | Add 32-bit signed integers horizontally,
 ymm1, ymm2, ymm3/m256                |      |                       |                   | pack to ymm1.                           
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
(V)PHADDW adds two adjacent 16-bit signed integers horizontally from the source
and destination operands and packs the 16-bit signed results to the destination
operand (first operand). (V)PHADDD adds two adjacent 32-bit signed integers
horizontally from the source and destination operands and packs the 32-bit signed
results to the destination operand (first operand). When the source operand
is a 128-bit memory operand, the operand must be aligned on a 16-byte boundary
or a general-protection exception (**``#GP)``** will be generated.

<aside class="notification">
Note that these instructions can operate on either unsigned or signed (two's
complement notation) integers; however, it does not set bits in the EFLAGS register
to indicate overflow and/or a carry. To prevent undetected overflow conditions,
software must control the ranges of the values operated on.
</aside>

Legacy SSE instructions: Both operands can be MMX registers. The second source
operand can be an MMX register or a 64-bit memory location. 128-bit Legacy SSE
version: The first source and destination operands are XMM registers. The second
source operand can be an XMM register or a 128-bit memory location. Bits (VLMAX-1:128)
of the corresponding YMM destination register remain unchanged.

In 64-bit mode, use the REX prefix to access additional registers.

VEX.128 encoded version: The first source and destination operands are XMM registers.
The second source operand can be an XMM register or a 128-bit memory location.
Bits (VLMAX-1:128) of the corresponding YMM register are zeroed. VEX.256 encoded
version: Horizontal addition of two adjacent data elements of the low 16-bytes
of the first and second source operands are packed into the low 16-bytes of
the destination operand. Horizontal addition of two adjacent data elements of
the high 16-bytes of the first and second source operands are packed into the
high 16bytes of the destination operand. The first source and destination operands
are YMM registers. The second source operand can be an YMM register or a 256-bit
memory location.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>

   | |  
---- | -----
 SRC2| Y7| Y6 S7| Y5 Figure 4-6.| Y4 S3| Y3 Dest| Y2 S3 255 256-bit VPHADDD Instruction| Y1| Y0 S4| X7 S3| X6| X5| X4 S2| X3 S1| X2 0| X1| X0 S0| SRC1
     |   |      |               |      |        | Operation                            |   |      |      |   |   |      |      |     |   |      |     


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
