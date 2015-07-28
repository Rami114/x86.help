## PUNPCKLBW/PUNPCKLWD/PUNPCKLDQ/PUNPCKLQDQ - Unpack Low Data

> Operation

``` slim```

### PUNPCKLBW instruction with 64-bit operands
  DEST[63:56] <- SRC[31:24];
  DEST[55:48] <- DEST[31:24];
  DEST[47:40] <- SRC[23:16];
  DEST[39:32] <- DEST[23:16];
  DEST[31:24] <- SRC[15:8];
  DEST[23:16] <- DEST[15:8];
  DEST[15:8] <- SRC[7:0];
  DEST[7:0] <- DEST[7:0];
### PUNPCKLWD instruction with 64-bit operands
  DEST[63:48] <- SRC[31:16];
  DEST[47:32] <- DEST[31:16];
  DEST[31:16] <- SRC[15:0];
  DEST[15:0] <- DEST[15:0];
### PUNPCKLDQ instruction with 64-bit operands
  DEST[63:32] <- SRC[31:0];
  DEST[31:0] <- DEST[31:0];
### PUNPCKLBW instruction with 128-bit operands
  DEST[7:0]<- DEST[7:0];
  DEST[15:8]
  DEST[23:16] <- DEST[15:8];
  DEST[31:24] <- SRC[15:8];
  DEST[39:32] <- DEST[23:16];
  DEST[47:40] <- SRC[23:16];
  DEST[55:48] <- DEST[31:24];
  DEST[63:56] <- SRC[31:24];
  DEST[71:64] <- DEST[39:32];
  DEST[79:72] <- SRC[39:32];
  DEST[87:80] <- DEST[47:40];
  DEST[95:88] <- SRC[47:40];
  DEST[103:96]
  DEST[111:104] <- SRC[55:48];
  DEST[119:112] <- DEST[63:56];
  DEST[127:120] <- SRC[63:56];
### PUNPCKLWD instruction with 128-bit operands
  DEST[15:0]
  DEST[31:16] <- SRC[15:0];
  DEST[47:32] <- DEST[31:16];
  DEST[63:48] <- SRC[31:16];
  DEST[79:64] <- DEST[47:32];
  DEST[95:80] <- SRC[47:32];
  DEST[111:96]
  DEST[127:112] <- SRC[63:48];
### PUNPCKLDQ instruction with 128-bit operands
  DEST[31:0] <- DEST[31:0];
  DEST[63:32]
  DEST[95:64]
  DEST[127:96] <- SRC[63:32];
PUNPCKLQDQ
  DEST[63:0] <- DEST[63:0];
  DEST[127:64] <- SRC[63:0];
INTERLEAVE_BYTES_256b (SRC1, SRC2)
DEST[7:0] <- SRC1[7:0]
DEST[15:8] <- SRC2[7:0]
DEST[23:16] <- SRC1[15:8]
DEST[31:24] <- SRC2[15:8]
DEST[39:32] <- SRC1[23:16]
DEST[47:40] <- SRC2[23:16]
DEST[55:48] <- SRC1[31:24]
DEST[63:56] <-SRC2[31:24]
DEST[71:64] <- SRC1[39:32]
DEST[79:72] <- SRC2[39:32]
DEST[87:80] <- SRC1[47:40]
DEST[95:88] <- SRC2[47:40]
DEST[103:96] <- SRC1[55:48]
DEST[111:104] <- SRC2[55:48]
DEST[119:112] <- SRC1[63:56]
DEST[127:120] <- SRC2[63:56]
DEST[135:128] <- SRC1[135:128]
DEST[143:136] <- SRC2[135:128]
DEST[151:144] <- SRC1[143:136]
DEST[159:152] <- SRC2[143:136]
DEST[167:160] <- SRC1[151:144]
DEST[175:168] <- SRC2[151:144]
DEST[183:176] <- SRC1[159:152]
DEST[191:184] <-SRC2[159:152]
DEST[199:192] <- SRC1[167:160]
DEST[207:200] <- SRC2[167:160]
DEST[215:208] <- SRC1[175:168]
DEST[223:216] <- SRC2[175:168]
DEST[231:224] <- SRC1[183:176]
DEST[239:232] <- SRC2[183:176]
DEST[247:240] <- SRC1[191:184]
DEST[255:248] <- SRC2[191:184]
INTERLEAVE_BYTES (SRC1, SRC2)
DEST[7:0] <- SRC1[7:0]
DEST[15:8] <- SRC2[7:0]
DEST[23:16] <- SRC2[15:8]
DEST[31:24] <- SRC2[15:8]
DEST[39:32] <- SRC1[23:16]
DEST[47:40] <- SRC2[23:16]
DEST[55:48] <- SRC1[31:24]
DEST[63:56] <-SRC2[31:24]
DEST[71:64] <- SRC1[39:32]
DEST[79:72] <- SRC2[39:32]
DEST[87:80] <- SRC1[47:40]
DEST[95:88] <- SRC2[47:40]
DEST[103:96] <- SRC1[55:48]
DEST[111:104] <- SRC2[55:48]
DEST[119:112] <- SRC1[63:56]
DEST[127:120] <- SRC2[63:56]
INTERLEAVE_WORDS_256b(SRC1, SRC2)
DEST[15:0] <- SRC1[15:0]
DEST[31:16] <- SRC2[15:0]
DEST[47:32] <- SRC1[31:16]
DEST[63:48] <- SRC2[31:16]
DEST[79:64] <- SRC1[47:32]
DEST[95:80] <- SRC2[47:32]
DEST[111:96] <- SRC1[63:48]
DEST[127:112] <- SRC2[63:48]
DEST[143:128] <- SRC1[143:128]
DEST[159:144] <- SRC2[143:128]
DEST[175:160] <- SRC1[159:144]
DEST[191:176] <- SRC2[159:144]
DEST[207:192] <- SRC1[175:160]
DEST[223:208] <- SRC2[175:160]
DEST[239:224] <- SRC1[191:176]
DEST[255:240] <- SRC2[191:176]
INTERLEAVE_WORDS (SRC1, SRC2)
DEST[15:0] <- SRC1[15:0]
DEST[31:16] <- SRC2[15:0]
DEST[47:32] <- SRC1[31:16]
DEST[63:48] <- SRC2[31:16]
DEST[79:64] <- SRC1[47:32]
DEST[95:80] <- SRC2[47:32]
DEST[111:96] <- SRC1[63:48]
DEST[127:112] <- SRC2[63:48]
INTERLEAVE_DWORDS_256b(SRC1, SRC2)
DEST[31:0] <- SRC1[31:0]
DEST[63:32] <- SRC2[31:0]
DEST[95:64] <- SRC1[63:32]
DEST[127:96] <- SRC2[63:32]
DEST[159:128] <- SRC1[159:128]
DEST[191:160] <- SRC2[159:128]
DEST[223:192] <- SRC1[191:160]
DEST[255:224] <- SRC2[191:160]
INTERLEAVE_DWORDS(SRC1, SRC2)
DEST[31:0] <- SRC1[31:0]
DEST[63:32] <- SRC2[31:0]
DEST[95:64] <- SRC1[63:32]
DEST[127:96] <- SRC2[63:32]
INTERLEAVE_QWORDS_256b(SRC1, SRC2)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
DEST[191:128] <- SRC1[191:128]
DEST[255:192] <- SRC2[191:128]
INTERLEAVE_QWORDS(SRC1, SRC2)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
PUNPCKLBW (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_BYTES(DEST, SRC)
DEST[255:127] (Unmodified)
VPUNPCKLBW (VEX.128 encoded instruction)
DEST[127:0] <- INTERLEAVE_BYTES(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPUNPCKLBW (VEX.256 encoded instruction)
DEST[255:0] <- INTERLEAVE_BYTES_128b(SRC1, SRC2)
PUNPCKLWD (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_WORDS(DEST, SRC)
DEST[255:127] (Unmodified)
VPUNPCKLWD (VEX.128 encoded instruction)
DEST[127:0] <- INTERLEAVE_WORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPUNPCKLWD (VEX.256 encoded instruction)
DEST[255:0] <- INTERLEAVE_WORDS(SRC1, SRC2)
PUNPCKLDQ (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_DWORDS(DEST, SRC)
DEST[255:127] (Unmodified)
VPUNPCKLDQ (VEX.128 encoded instruction)
DEST[127:0] <- INTERLEAVE_DWORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPUNPCKLDQ (VEX.256 encoded instruction)
DEST[255:0] <- INTERLEAVE_DWORDS(SRC1, SRC2)
PUNPCKLQDQ (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_QWORDS(DEST, SRC)
DEST[255:127] (Unmodified)
VPUNPCKLQDQ (VEX.128 encoded instruction)
DEST[127:0] <- INTERLEAVE_QWORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPUNPCKLQDQ (VEX.256 encoded instruction)
DEST[255:0] <- INTERLEAVE_QWORDS(SRC1, SRC2)

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PUNPCKLBW:    | __m64 _mm_unpacklo_pi8 (__m64 m1, __m64 
               | m2)                                     
 (V)PUNPCKLBW: | __m128i _mm_unpacklo_epi8 (__m128i m1,  
               | __m128i m2)                             
 VPUNPCKLBW:   | __m256i _mm256_unpacklo_epi8 (__m256i   
               | m1, __m256i m2)                         
 PUNPCKLWD:    | __m64 _mm_unpacklo_pi16 (__m64 m1, __m64
               | m2)                                     
 (V)PUNPCKLWD: | __m128i _mm_unpacklo_epi16 (__m128i     
               | m1, __m128i m2)                         
 VPUNPCKLWD:   | __m256i _mm256_unpacklo_epi16 (__m256i  
               | m1, __m256i m2)                         
 PUNPCKLDQ:    | __m64 _mm_unpacklo_pi32 (__m64 m1, __m64
               | m2)                                     
 (V)PUNPCKLDQ: | __m128i _mm_unpacklo_epi32 (__m128i     
               | m1, __m128i m2)                         
 VPUNPCKLDQ:   | __m256i _mm256_unpacklo_epi32 (__m256i  
               | m1, __m256i m2)                         
 (V)PUNPCKLQDQ:| __m128i _mm_unpacklo_epi64 (__m128i     
               | m1, __m128i m2)                         
 VPUNPCKLQDQ:  | __m256i _mm256_unpacklo_epi64 (__m256i  
               | m1, __m256i m2)                         

```

 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 0F 60 /r1 PUNPCKLBW mm, mm/m32         | RM   | V/V                   | MMX               | Interleave low-order bytes from mm and 
                                        |      |                       |                   | mm/m32 into mm.                        
 66 0F 60 /r PUNPCKLBW xmm1, xmm2/m128  | RM   | V/V                   | SSE2              | Interleave low-order bytes from xmm1   
                                        |      |                       |                   | and xmm2/m128 into xmm1.               
 0F 61 /r1 PUNPCKLWD mm, mm/m32         | RM   | V/V                   | MMX               | Interleave low-order words from mm and 
                                        |      |                       |                   | mm/m32 into mm.                        
 66 0F 61 /r PUNPCKLWD xmm1, xmm2/m128  | RM   | V/V                   | SSE2              | Interleave low-order words from xmm1   
                                        |      |                       |                   | and xmm2/m128 into xmm1.               
 0F 62 /r1 PUNPCKLDQ mm, mm/m32         | RM   | V/V                   | MMX               | Interleave low-order doublewords from  
                                        |      |                       |                   | mm and mm/m32 into mm.                 
 66 0F 62 /r PUNPCKLDQ xmm1, xmm2/m128  | RM   | V/V                   | SSE2              | Interleave low-order doublewords from  
                                        |      |                       |                   | xmm1 and xmm2/m128 into xmm1.          
 66 0F 6C /r PUNPCKLQDQ xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Interleave low-order quadword from xmm1
                                        |      |                       |                   | and xmm2/m128 into xmm1 register.      
 VEX.NDS.128.66.0F.WIG 60/r VPUNPCKLBW  | RVM  | V/V                   | AVX               | Interleave low-order bytes from xmm2   
 xmm1,xmm2, xmm3/m128                   |      |                       |                   | and xmm3/m128 into xmm1.               
 VEX.NDS.128.66.0F.WIG 61/r VPUNPCKLWD  | RVM  | V/V                   | AVX               | Interleave low-order words from xmm2   
 xmm1,xmm2, xmm3/m128                   |      |                       |                   | and xmm3/m128 into xmm1.               
 VEX.NDS.128.66.0F.WIG 62/r VPUNPCKLDQ  | RVM  | V/V                   | AVX               | Interleave low-order doublewords from  
 xmm1, xmm2, xmm3/m128                  |      |                       |                   | xmm2 and xmm3/m128 into xmm1.          
 VEX.NDS.128.66.0F.WIG 6C/r VPUNPCKLQDQ | RVM  | V/V                   | AVX               | Interleave low-order quadword from xmm2
 xmm1, xmm2, xmm3/m128                  |      |                       |                   | and xmm3/m128 into xmm1 register.      
 VEX.NDS.256.66.0F.WIG 60 /r VPUNPCKLBW | RVM  | V/V                   | AVX2              | Interleave low-order bytes from ymm2   
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | and ymm3/m256 into ymm1 register.      
 VEX.NDS.256.66.0F.WIG 61 /r VPUNPCKLWD | RVM  | V/V                   | AVX2              | Interleave low-order words from ymm2   
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | and ymm3/m256 into ymm1 register.      
 VEX.NDS.256.66.0F.WIG 62 /r VPUNPCKLDQ | RVM  | V/V                   | AVX2              | Interleave low-order doublewords from  
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | ymm2 and ymm3/m256 into ymm1 register. 
 VEX.NDS.256.66.0F.WIG 6C /r VPUNPCKLQDQ| RVM  | V/V                   | AVX2              | Interleave low-order quadword from ymm2
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | and ymm3/m256 into ymm1 register.      
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
Unpacks and interleaves the low-order data elements (bytes, words, doublewords,
and quadwords) of the destination operand (first operand) and source operand
(second operand) into the destination operand. (Figure 4-18 shows the unpack
operation for bytes in 64-bit operands.). The high-order data elements are ignored.

   | |  
---- | -----
 SRC SRC| Y7 Figure 4-18. 255 Y7| Y6 Y6 Figure 4-19.| Y5 Y5| Y4 PUNPCKLBW Instruction Operation Using| Y3 Y3| Y2 Y2 256-bit VPUNPCKLDQ Instruction| Y1 DEST Y1 DEST| Y0 Y3 0 Y0 255 Y5| X7 Y2 255 X7 Y4| X6 X2 X6 X4| X5 Y1 X5 Y1| X4 X1 X4 X1| X3 Y0 X3 Y0| X2 X0 X2 X0| X1 X1 0| X0 31 X0| DEST 0
        |                       |                   |      | 64-bit Operands Y4                      |      | Operation                           |                |                  |                |            |            |            |            |            |        |         |       
When the source data comes from a 128-bit memory operand, an implementation
may fetch only the appropriate 64 bits; however, alignment to a 16-byte boundary
and normal segment checking will still be enforced.

The (V)PUNPCKLBW instruction interleaves the low-order bytes of the source and
destination operands, the (V)PUNPCKLWD instruction interleaves the low-order
words of the source and destination operands, the (V)PUNPCKLDQ instruction interleaves
the low-order doubleword (or doublewords) of the source and destination operands,
and the (V)PUNPCKLQDQ instruction interleaves the low-order quadwords of the
source and destination operands.

These instructions can be used to convert bytes to words, words to doublewords,
doublewords to quadwords, and quadwords to double quadwords, respectively, by
placing all 0s in the source operand. Here, if the source operand contains all
0s, the result (stored in the destination operand) contains zero extensions
of the high-order data elements from the original value in the destination operand.
For example, with the (V)PUNPCKLBW instruction the high-order bytes are zero
extended (that is, unpacked into unsigned word integers), and with the (V)PUNPCKLWD
instruction, the high-order words are zero extended (unpacked into unsigned
doubleword integers).

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE versions: The source operand can be an MMX technology register or
a 32-bit memory location. The destination operand is an MMX technology register.
128-bit Legacy SSE versions: The second source operand is an XMM register or
a 128-bit memory location. The first source operand and destination operands
are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded versions: The second source operand is an
XMM register or a 128-bit memory location. The first source operand and destination
operands are XMM registers. Bits (VLMAX-1:128) of the destination YMM register
are zeroed.

VEX.256 encoded version: The second source operand is an YMM register or a 256-bit
memory location. The first source operand and destination operands are YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise instructions will #UD.
</aside>



### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
