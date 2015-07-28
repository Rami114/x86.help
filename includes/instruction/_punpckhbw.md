## PUNPCKHBW/PUNPCKHWD/PUNPCKHDQ/PUNPCKHQDQ -  Unpack High Data

> Operation

``` slim```

### PUNPCKHBW instruction with 64-bit operands
  DEST[7:0] <- DEST[39:32];
  DEST[15:8] <- SRC[39:32];
  DEST[23:16] <- DEST[47:40];
  DEST[31:24] <- SRC[47:40];
  DEST[39:32] <- DEST[55:48];
  DEST[47:40] <- SRC[55:48];
  DEST[55:48] <- DEST[63:56];
  DEST[63:56] <- SRC[63:56];
### PUNPCKHW instruction with 64-bit operands
  DEST[15:0] <- DEST[47:32];
  DEST[31:16] <- SRC[47:32];
  DEST[47:32] <- DEST[63:48];
  DEST[63:48] <- SRC[63:48];
### PUNPCKHDQ instruction with 64-bit operands
  DEST[31:0] <- DEST[63:32];
  DEST[63:32] <- SRC[63:32];
### PUNPCKHBW instruction with 128-bit operands
  DEST[7:0]<- DEST[71:64];
  DEST[15:8]
  DEST[23:16] <- DEST[79:72];
  DEST[31:24] <- SRC[79:72];
  DEST[39:32] <- DEST[87:80];
  DEST[47:40] <- SRC[87:80];
  DEST[55:48] <- DEST[95:88];
  DEST[63:56] <- SRC[95:88];
  DEST[71:64] <- DEST[103:96];
  DEST[79:72] <- SRC[103:96];
  DEST[87:80] <- DEST[111:104];
  DEST[95:88] <- SRC[111:104];
  DEST[103:96]
  DEST[111:104] <- SRC[119:112];
  DEST[119:112] <- DEST[127:120];
  DEST[127:120] <- SRC[127:120];
### PUNPCKHWD instruction with 128-bit operands
  DEST[15:0]
  DEST[31:16] <- SRC[79:64];
  DEST[47:32] <- DEST[95:80];
  DEST[63:48] <- SRC[95:80];
  DEST[79:64] <- DEST[111:96];
  DEST[95:80] <- SRC[111:96];
  DEST[111:96]
  DEST[127:112] <- SRC[127:112];
### PUNPCKHDQ instruction with 128-bit operands
  DEST[31:0] <- DEST[95:64];
  DEST[63:32]
  DEST[95:64]
  DEST[127:96] <- SRC[127:96];
### PUNPCKHQDQ instruction
  DEST[63:0] <- DEST[127:64];
  DEST[127:64] <- SRC[127:64];
INTERLEAVE_HIGH_BYTES_256b (SRC1, SRC2)
DEST[7:0] <- SRC1[71:64]
DEST[15:8] <- SRC2[71:64]
DEST[23:16] <- SRC1[79:72]
DEST[31:24] <- SRC2[79:72]
DEST[39:32] <- SRC1[87:80]
DEST[47:40] <- SRC2[87:80]
DEST[55:48] <- SRC1[95:88]
DEST[63:56] <-SRC2[95:88]
DEST[71:64] <- SRC1[103:96]
DEST[79:72] <- SRC2[103:96]
DEST[87:80] <- SRC1[111:104]
DEST[95:88] <- SRC2[111:104]
DEST[103:96] <- SRC1[119:112]
DEST[111:104] <- SRC2[119:112]
DEST[119:112] <- SRC1[127:120]
DEST[127:120] <- SRC2[127:120]
DEST[135:128] <- SRC1[199:192]
DEST[143:136] <- SRC2[199:192]
DEST[151:144] <- SRC1[207:200]
DEST[159:152] <- SRC2[207:200]
DEST[167:160] <- SRC1[215:208]
DEST[175:168] <- SRC2[215:208]
DEST[183:176] <- SRC1[223:216]
DEST[191:184] <-SRC2[223:216]
DEST[199:192] <- SRC1[231:224]
DEST[207:200] <- SRC2[231:224]
DEST[215:208] <- SRC1[239:232]
DEST[223:216] <- SRC2[239:232]
DEST[231:224] <- SRC1[247:240]
DEST[239:232] <- SRC2[247:240]
DEST[247:240] <- SRC1[255:248]
DEST[255:248] <- SRC2[255:248]
INTERLEAVE_HIGH_BYTES (SRC1, SRC2)
DEST[7:0] <- SRC1[71:64]
DEST[15:8] <- SRC2[71:64]
DEST[23:16] <- SRC1[79:72]
DEST[31:24] <- SRC2[79:72]
DEST[39:32] <- SRC1[87:80]
DEST[47:40] <- SRC2[87:80]
DEST[55:48] <- SRC1[95:88]
DEST[63:56] <-SRC2[95:88]
DEST[71:64] <- SRC1[103:96]
DEST[79:72] <- SRC2[103:96]
DEST[87:80] <- SRC1[111:104]
DEST[95:88] <- SRC2[111:104]
DEST[103:96] <- SRC1[119:112]
DEST[111:104] <- SRC2[119:112]
DEST[119:112] <- SRC1[127:120]
DEST[127:120] <- SRC2[127:120]
INTERLEAVE_HIGH_WORDS_256b(SRC1, SRC2)
DEST[15:0] <- SRC1[79:64]
DEST[31:16] <- SRC2[79:64]
DEST[47:32] <- SRC1[95:80]
DEST[63:48] <- SRC2[95:80]
DEST[79:64] <- SRC1[111:96]
DEST[95:80] <- SRC2[111:96]
DEST[111:96] <- SRC1[127:112]
DEST[127:112] <- SRC2[127:112]
DEST[143:128] <- SRC1[207:192]
DEST[159:144] <- SRC2[207:192]
DEST[175:160] <- SRC1[223:208]
DEST[191:176] <- SRC2[223:208]
DEST[207:192] <- SRC1[239:224]
DEST[223:208] <- SRC2[239:224]
DEST[239:224] <- SRC1[255:240]
DEST[255:240] <- SRC2[255:240]
INTERLEAVE_HIGH_WORDS (SRC1, SRC2)
DEST[15:0] <- SRC1[79:64]
DEST[31:16] <- SRC2[79:64]
DEST[47:32] <- SRC1[95:80]
DEST[63:48] <- SRC2[95:80]
DEST[79:64] <- SRC1[111:96]
DEST[95:80] <- SRC2[111:96]
DEST[111:96] <-SRC1[127:112]
DEST[127:112] <- SRC2[127:112]
INTERLEAVE_HIGH_DWORDS_256b(SRC1, SRC2)
DEST[31:0] <- SRC1[95:64]
DEST[63:32] <- SRC2[95:64]
DEST[95:64] <- SRC1[127:96]
DEST[127:96] <- SRC2[127:96]
DEST[159:128] <- SRC1[223:192]
DEST[191:160] <- SRC2[223:192]
DEST[223:192] <- SRC1[255:224]
DEST[255:224] <- SRC2[255:224]
INTERLEAVE_HIGH_DWORDS(SRC1, SRC2)
DEST[31:0] <- SRC1[95:64]
DEST[63:32] <- SRC2[95:64]
DEST[95:64] <- SRC1[127:96]
DEST[127:96] <- SRC2[127:96]
INTERLEAVE_HIGH_QWORDS_256b(SRC1, SRC2)
DEST[63:0] <- SRC1[127:64]
DEST[127:64] <- SRC2[127:64]
DEST[191:128] <- SRC1[255:192]
DEST[255:192] <- SRC2[255:192]
INTERLEAVE_HIGH_QWORDS(SRC1, SRC2)
DEST[63:0] <- SRC1[127:64]
DEST[127:64] <- SRC2[127:64]
PUNPCKHBW (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_HIGH_BYTES(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
VPUNPCKHBW (VEX.128 encoded version)
DEST[127:0] <- INTERLEAVE_HIGH_BYTES(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPUNPCKHBW (VEX.256 encoded version)
DEST[255:0] <- INTERLEAVE_HIGH_BYTES_256b(SRC1, SRC2)
PUNPCKHWD (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_HIGH_WORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
VPUNPCKHWD (VEX.128 encoded version)
DEST[127:0] <- INTERLEAVE_HIGH_WORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPUNPCKHWD (VEX.256 encoded version)
DEST[255:0] <- INTERLEAVE_HIGH_WORDS_256b(SRC1, SRC2)
PUNPCKHDQ (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_HIGH_DWORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
VPUNPCKHDQ (VEX.128 encoded version)
DEST[127:0] <- INTERLEAVE_HIGH_DWORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPUNPCKHDQ (VEX.256 encoded version)
DEST[255:0] <- INTERLEAVE_HIGH_DWORDS_256b(SRC1, SRC2)
PUNPCKHQDQ (128-bit Legacy SSE Version)
DEST[127:0] <- INTERLEAVE_HIGH_QWORDS(DEST, SRC)
DEST[255:127] (Unmodified)
VPUNPCKHQDQ (VEX.128 encoded version)
DEST[127:0] <- INTERLEAVE_HIGH_QWORDS(SRC1, SRC2)
DEST[255:127] <- 0
VPUNPCKHQDQ (VEX.256 encoded version)
DEST[255:0] <- INTERLEAVE_HIGH_QWORDS_256(SRC1, SRC2)

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PUNPCKHBW:    | __m64 _mm_unpackhi_pi8(__m64 m1, __m64       
               | m2)                                          
 (V)PUNPCKHBW: | __m128i _mm_unpackhi_epi8(__m128i m1,        
               | __m128i m2)                                  
 VPUNPCKHBW:   | __m256i _mm256_unpackhi_epi8(__m256i         
               | m1, __m256i m2)                              
 PUNPCKHWD:    | __m64 _mm_unpackhi_pi16(__m64 m1,__m64       
               | m2)                                          
 (V)PUNPCKHWD: | __m128i _mm_unpackhi_epi16(__m128i m1,__m128i
               | m2)                                          
 VPUNPCKHWD:   | __m256i _mm256_unpackhi_epi16(__m256i        
               | m1,__m256i m2)                               
 PUNPCKHDQ:    | __m64 _mm_unpackhi_pi32(__m64 m1, __m64      
               | m2)                                          
 (V)PUNPCKHDQ: | __m128i _mm_unpackhi_epi32(__m128i m1,       
               | __m128i m2)                                  
 VPUNPCKHDQ:   | __m256i _mm256_unpackhi_epi32(__m256i        
               | m1, __m256i m2)                              
 (V)PUNPCKHQDQ:| __m128i _mm_unpackhi_epi64 ( __m128i         
               | a, __m128i b)                                
 VPUNPCKHQDQ:  | __m256i _mm256_unpackhi_epi64 ( __m256i      
               | a, __m256i b)                                

```

 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 0F 68 /r1 PUNPCKHBW mm, mm/m64         | RM   | V/V                   | MMX               | Unpack and interleave high-order bytes      
                                        |      |                       |                   | from mm and mm/m64 into mm.                 
 66 0F 68 /r PUNPCKHBW xmm1, xmm2/m128  | RM   | V/V                   | SSE2              | Unpack and interleave high-order bytes      
                                        |      |                       |                   | from xmm1 and xmm2/m128 into xmm1.          
 0F 69 /r1 PUNPCKHWD mm, mm/m64         | RM   | V/V                   | MMX               | Unpack and interleave high-order words      
                                        |      |                       |                   | from mm and mm/m64 into mm.                 
 66 0F 69 /r PUNPCKHWD xmm1, xmm2/m128  | RM   | V/V                   | SSE2              | Unpack and interleave high-order words      
                                        |      |                       |                   | from xmm1 and xmm2/m128 into xmm1.          
 0F 6A /r1 PUNPCKHDQ mm, mm/m64         | RM   | V/V                   | MMX               | Unpack and interleave high-order doublewords
                                        |      |                       |                   | from mm and mm/m64 into mm.                 
 66 0F 6A /r PUNPCKHDQ xmm1, xmm2/m128  | RM   | V/V                   | SSE2              | Unpack and interleave high-order doublewords
                                        |      |                       |                   | from xmm1 and xmm2/m128 into xmm1.          
 66 0F 6D /r PUNPCKHQDQ xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Unpack and interleave high-order quadwords  
                                        |      |                       |                   | from xmm1 and xmm2/m128 into xmm1.          
 VEX.NDS.128.66.0F.WIG 68/r VPUNPCKHBW  | RVM  | V/V                   | AVX               | Interleave high-order bytes from xmm2       
 xmm1,xmm2, xmm3/m128                   |      |                       |                   | and xmm3/m128 into xmm1.                    
 VEX.NDS.128.66.0F.WIG 69/r VPUNPCKHWD  | RVM  | V/V                   | AVX               | Interleave high-order words from xmm2       
 xmm1,xmm2, xmm3/m128                   |      |                       |                   | and xmm3/m128 into xmm1.                    
 VEX.NDS.128.66.0F.WIG 6A/r VPUNPCKHDQ  | RVM  | V/V                   | AVX               | Interleave high-order doublewords from      
 xmm1, xmm2, xmm3/m128                  |      |                       |                   | xmm2 and xmm3/m128 into xmm1.               
 VEX.NDS.128.66.0F.WIG 6D/r VPUNPCKHQDQ | RVM  | V/V                   | AVX               | Interleave high-order quadword from         
 xmm1, xmm2, xmm3/m128                  |      |                       |                   | xmm2 and xmm3/m128 into xmm1 register.      
 VEX.NDS.256.66.0F.WIG 68 /r VPUNPCKHBW | RVM  | V/V                   | AVX2              | Interleave high-order bytes from ymm2       
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | and ymm3/m256 into ymm1 register.           
 VEX.NDS.256.66.0F.WIG 69 /r VPUNPCKHWD | RVM  | V/V                   | AVX2              | Interleave high-order words from ymm2       
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | and ymm3/m256 into ymm1 register.           
 VEX.NDS.256.66.0F.WIG 6A /r VPUNPCKHDQ | RVM  | V/V                   | AVX2              | Interleave high-order doublewords from      
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | ymm2 and ymm3/m256 into ymm1 register.      
 VEX.NDS.256.66.0F.WIG 6D /r VPUNPCKHQDQ| RVM  | V/V                   | AVX2              | Interleave high-order quadword from         
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | ymm2 and ymm3/m256 into ymm1 register.      
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
Unpacks and interleaves the high-order data elements (bytes, words, doublewords,
or quadwords) of the destination operand (first operand) and source operand
(second operand) into the destination operand. Figure 4-16 shows the unpack
operation for bytes in 64-bit operands. The low-order data elements are ignored.

   | |  
---- | -----
 SRC SRC| Y7 DEST Figure 4-16. 255 Y7| Y6 Y7 Y6 Figure 4-17.| Y5 X7 Y5| Y4 Y6 PUNPCKHBW Instruction Operation| Y3 X6 Y3| Y2 Y5 Y2 256-bit VPUNPCKHDQ Instruction| Y1 X5 Y1 DEST| Y0 X4 0 Y0 255 Y7| X7 255 X7 Y6| X6 X6 X6| X5 X5 Y3| X4 X4 X3| X3 X3 Y2| X2 X2 X2| X1 X1 0| X0 31 X0| DEST 0
        |                            |                      |         | Using 64-bit Operands Y4             |         | Operation                              |              |                  |             |         |         |         |         |         |        |         |       
When the source data comes from a 64-bit memory operand, the full 64-bit operand
is accessed from memory, but the instruction uses only the high-order 32 bits.
When the source data comes from a 128-bit memory operand, an implementation
may fetch only the appropriate 64 bits; however, alignment to a 16-byte boundary
and normal segment checking will still be enforced.

The (V)PUNPCKHBW instruction interleaves the high-order bytes of the source
and destination operands, the (V)PUNPCKHWD instruction interleaves the high-order
words of the source and destination operands, the (V)PUNPCKHDQ instruction interleaves
the high-order doubleword (or doublewords) of the source and destination operands,
and the (V)PUNPCKHQDQ instruction interleaves the high-order quadwords of the
source and destination operands.

These instructions can be used to convert bytes to words, words to doublewords,
doublewords to quadwords, and quadwords to double quadwords, respectively, by
placing all 0s in the source operand. Here, if the source operand contains all
0s, the result (stored in the destination operand) contains zero extensions
of the high-order data elements from the original value in the destination operand.
For example, with the (V)PUNPCKHBW instruction the high-order bytes are zero
extended (that is, unpacked into unsigned word integers), and with the (V)PUNPCKHWD
instruction, the high-order words are zero extended (unpacked into unsigned
doubleword integers).

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE versions: The source operand can be an MMX technology register or
a 64-bit memory location. The destination operand is an MMX technology register.
128-bit Legacy SSE versions: The second source operand is an XMM register or
a 128-bit memory location. The first source operand and destination operands
are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged.

VEX.128 encoded versions: The second source operand is an XMM register or a
128-bit memory location. The first source operand and destination operands are
XMM registers. Bits (VLMAX-1:128) of the destination YMM register are zeroed.
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
