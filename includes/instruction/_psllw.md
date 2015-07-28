## PSLLW/PSLLD/PSLLQ - Shift Packed Data Left Logical

> Operation

``` slim
PSLLW (with 64-bit operand)
  IF (COUNT > 15)
  THEN
     DEST[64:0] <- 0000000000000000H;
  ELSE
     DEST[15:0] <- ZeroExtend(DEST[15:0] << COUNT);
     (\* Repeat shift operation for 2nd and 3rd words \*)
     DEST[63:48] <- ZeroExtend(DEST[63:48] << COUNT);
  FI;
PSLLD (with 64-bit operand)
  IF (COUNT > 31)
  THEN
     DEST[64:0] <- 0000000000000000H;
  ELSE
     DEST[31:0] <- ZeroExtend(DEST[31:0] << COUNT);
     DEST[63:32] <- ZeroExtend(DEST[63:32] << COUNT);
  FI;
PSLLQ (with 64-bit operand)
  IF (COUNT > 63)
  THEN
     DEST[64:0] <- 0000000000000000H;
  ELSE
     DEST <- ZeroExtend(DEST << COUNT);
  FI;
PSLLW (with 128-bit operand)
  COUNT <- COUNT_SOURCE[63:0];
  IF (COUNT > 15)
  THEN
     DEST[128:0] <- 00000000000000000000000000000000H;
  ELSE
     DEST[15:0]
     (\* Repeat shift operation for 2nd through 7th words \*)
     DEST[127:112] <- ZeroExtend(DEST[127:112] << COUNT);
  FI;
PSLLD (with 128-bit operand)
  COUNT <- COUNT_SOURCE[63:0];
  IF (COUNT > 31)
  THEN
     DEST[128:0] <- 00000000000000000000000000000000H;
  ELSE
     DEST[31:0]
     (\* Repeat shift operation for 2nd and 3rd doublewords \*)
     DEST[127:96] <- ZeroExtend(DEST[127:96] << COUNT);
  FI;
PSLLQ (with 128-bit operand)
  COUNT <- COUNT_SOURCE[63:0];
  IF (COUNT > 63)
  THEN
     DEST[128:0] <- 00000000000000000000000000000000H;
  ELSE
     DEST[63:0]
     DEST[127:64] <- ZeroExtend(DEST[127:64] << COUNT);
  FI;
PSLLW (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_WORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
PSLLW (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_WORDS(DEST, imm8)
DEST[VLMAX-1:128] (Unmodified)
VPSLLD (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_DWORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPSLLD (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_DWORDS(SRC1, imm8)
DEST[VLMAX-1:128] <- 0
PSLLD (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_DWORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
PSLLD (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_DWORDS(DEST, imm8)
DEST[VLMAX-1:128] (Unmodified)
VPSLLQ (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_QWORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPSLLQ (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_QWORDS(SRC1, imm8)
DEST[VLMAX-1:128] <- 0
PSLLQ (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_QWORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
PSLLQ (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_QWORDS(DEST, imm8)
DEST[VLMAX-1:128] (Unmodified)
VPSLLW (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_WORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPSLLW (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_WORDS(SRC1, imm8)
DEST[VLMAX-1:128] <- 0
PSLLW (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_WORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
PSLLW (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_WORDS(DEST, imm8)
DEST[VLMAX-1:128] (Unmodified)
VPSLLD (xmm, xmm, xmm/m128)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_DWORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPSLLD (xmm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_DWORDS(SRC1, imm8)
DEST[VLMAX-1:128] <- 0
VPSLLW (ymm, ymm, xmm/m128)
DEST[255:0] <- LOGICAL_LEFT_SHIFT_WORDS_256b(SRC1, SRC2)
VPSLLW (ymm, imm8)
DEST[255:0] <- LOGICAL_LEFT_SHIFT_WORD_256bS(SRC1, imm8)
VPSLLD (ymm, ymm, xmm/m128)
DEST[255:0] <- LOGICAL_LEFT_SHIFT_DWORDS_256b(SRC1, SRC2)
VPSLLD (ymm, imm8)
DEST[127:0] <- LOGICAL_LEFT_SHIFT_DWORDS_256b(SRC1, imm8)
VPSLLQ (ymm, ymm, xmm/m128)
DEST[255:0] <- LOGICAL_LEFT_SHIFT_QWORDS_256b(SRC1, SRC2)
VPSLLQ (ymm, imm8)
DEST[255:0] <- LOGICAL_LEFT_SHIFT_QWORDS_256b(SRC1, imm8)

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PSLLW:   | __m64 _mm_slli_pi16 (__m64 m, int count) 
 PSLLW:   | __m64 _mm_sll_pi16(__m64 m, __m64 count) 
 (V)PSLLW:| __m128i _mm_slli_pi16(__m64 m, int count)
 (V)PSLLW:| __m128i _mm_slli_pi16(__m128i m, __m128i 
          | count)                                   
 VPSLLW:  | __m256i _mm256_slli_epi16 (__m256i m,    
          | int count)                               
 VPSLLW:  | __m256i _mm256_sll_epi16 (__m256i m,     
          | __m128i count)                           
 PSLLD:   | count)                                   
 PSLLD:   | __m64 _mm_sll_pi32(__m64 m, __m64 count) 
 (V)PSLLD:| count)                                   
 (V)PSLLD:| __m128i _mm_sll_epi32(__m128i m, __m128i 
          | count)                                   
 VPSLLD:  | __m256i _mm256_slli_epi32 (__m256i m,    
          | int count)                               
 VPSLLD:  | __m256i _mm256_sll_epi32 (__m256i m,     
          | __m128i count)                           
 PSLLQ:   | count)                                   
 PSLLQ:   | __m64 _mm_sll_si64(__m64 m, __m64 count) 
 (V)PSLLQ:| count)                                   
 (V)PSLLQ:| __m128i _mm_sll_epi64(__m128i m, __m128i 
          | count)                                   
 VPSLLQ:  | __m256i _mm256_slli_epi64 (__m256i m,    
          | int count)                               
 VPSLLQ:  | __m256i _mm256_sll_epi64 (__m256i m,     
          | __m128i count)                           

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 0F F1 /r1 PSLLW mm, mm/m64              | RM   | V/V                   | MMX               | Shift words in mm left mm/m64 while         
                                         |      |                       |                   | shifting in 0s.                             
 66 0F F1 /r PSLLW xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Shift words in xmm1 left by xmm2/m128       
                                         |      |                       |                   | while shifting in 0s.                       
 0F 71 /6 ib PSLLW mm1, imm8             | MI   | V/V                   | MMX               | Shift words in mm left by imm8 while        
                                         |      |                       |                   | shifting in 0s.                             
 66 0F 71 /6 ib PSLLW xmm1, imm8         | MI   | V/V                   | SSE2              | Shift words in xmm1 left by imm8 while      
                                         |      |                       |                   | shifting in 0s.                             
 0F F2 /r1 PSLLD mm, mm/m64              | RM   | V/V                   | MMX               | Shift doublewords in mm left by mm/m64      
                                         |      |                       |                   | while shifting in 0s.                       
 66 0F F2 /r PSLLD xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Shift doublewords in xmm1 left by xmm2/m128 
                                         |      |                       |                   | while shifting in 0s.                       
 0F 72 /6 ib1 PSLLD mm, imm8             | MI   | V/V                   | MMX               | Shift doublewords in mm left by imm8        
                                         |      |                       |                   | while shifting in 0s.                       
 66 0F 72 /6 ib PSLLD xmm1, imm8         | MI   | V/V                   | SSE2              | Shift doublewords in xmm1 left by imm8      
                                         |      |                       |                   | while shifting in 0s.                       
 0F F3 /r1 PSLLQ mm, mm/m64              | RM   | V/V                   | MMX               | Shift quadword in mm left by mm/m64         
                                         |      |                       |                   | while shifting in 0s.                       
 66 0F F3 /r PSLLQ xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Shift quadwords in xmm1 left by xmm2/m128   
                                         |      |                       |                   | while shifting in 0s.                       
 0F 73 /6 ib1 PSLLQ mm, imm8             | MI   | V/V                   | MMX               | Shift quadword in mm left by imm8 while     
                                         |      |                       |                   | shifting in 0s.                             
 66 0F 73 /6 ib PSLLQ xmm1, imm8         | MI   | V/V                   | SSE2              | Shift quadwords in xmm1 left by imm8        
                                         |      |                       |                   | while shifting in 0s.                       
 VEX.NDS.128.66.0F.WIG F1 /r VPSLLW xmm1,| RVM  | V/V                   | AVX               | Shift words in xmm2 left by amount specified
 xmm2, xmm3/m128                         |      |                       |                   | in xmm3/m128 while shifting in 0s.          
 VEX.NDD.128.66.0F.WIG 71 /6 ib VPSLLW   | VMI  | V/V                   | AVX               | Shift words in xmm2 left by imm8 while      
 xmm1, xmm2, imm8                        |      |                       |                   | shifting in 0s.                             
 VEX.NDS.128.66.0F.WIG F2 /r VPSLLD xmm1,| RVM  | V/V                   | AVX               | Shift doublewords in xmm2 left by amount    
 xmm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting       
                                         |      |                       |                   | in 0s.                                      
 VEX.NDD.128.66.0F.WIG 72 /6 ib VPSLLD   | VMI  | V/V                   | AVX               | Shift doublewords in xmm2 left by imm8      
 xmm1, xmm2, imm8                        |      |                       |                   | while shifting in 0s.                       
 VEX.NDS.128.66.0F.WIG F3 /r VPSLLQ xmm1,| RVM  | V/V                   | AVX               | Shift quadwords in xmm2 left by amount      
 xmm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting       
                                         |      |                       |                   | in 0s.                                      
 VEX.NDD.128.66.0F.WIG 73 /6 ib VPSLLQ   | VMI  | V/V                   | AVX               | Shift quadwords in xmm2 left by imm8        
 xmm1, xmm2, imm8                        |      |                       |                   | while shifting in 0s.                       
 VEX.NDS.256.66.0F.WIG F1 /r VPSLLW ymm1,| RVM  | V/V                   | AVX2              | Shift words in ymm2 left by amount specified
 ymm2, xmm3/m128                         |      |                       |                   | in xmm3/m128 while shifting in 0s.          
 VEX.NDD.256.66.0F.WIG 71 /6 ib VPSLLW   | VMI  | V/V                   | AVX2              | Shift words in ymm2 left by imm8 while      
 ymm1, ymm2, imm8                        |      |                       |                   | shifting in 0s.                             
 VEX.NDS.256.66.0F.WIG F2 /r VPSLLD ymm1,| RVM  | V/V                   | AVX2              | Shift doublewords in ymm2 left by amount    
 ymm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting       
                                         |      |                       |                   | in 0s.                                      
 VEX.NDD.256.66.0F.WIG 72 /6 ib VPSLLD   | VMI  | V/V                   | AVX2              | Shift doublewords in ymm2 left by imm8      
 ymm1, ymm2, imm8                        |      |                       |                   | while shifting in 0s.                       
 VEX.NDS.256.66.0F.WIG F3 /r VPSLLQ ymm1,| RVM  | V/V                   | AVX2              | Shift quadwords in ymm2 left by amount      
 ymm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting       
                                         |      |                       |                   | in 0s.                                      
 VEX.NDD.256.66.0F.WIG 73 /6 ib VPSLLQ   | VMI  | V/V                   | AVX2              | Shift quadwords in ymm2 left by imm8        
 ymm1, ymm2, imm8                        |      |                       |                   | while shifting in 0s.                       
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
 MI   | ModRM:r/m (r, w)| imm8         | NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       
 VMI  | VEX.vvvv (w)    | ModRM:r/m (r)| imm8         | NA       

### Description
Shifts the bits in the individual data elements (words, doublewords, or quadword)
in the destination operand (first operand) to the left by the number of bits
specified in the count operand (second operand). As the bits in the data elements
are shifted left, the empty low-order bits are cleared (set to 0). If the value
specified by the count operand is greater than 15 (for words), 31 (for doublewords),
or 63 (for a quadword), then the destination operand is set to all 0s. Figure
4-13 gives an example of shifting words in a 64-bit operand.

Pre-Shift

   | |  
---- | -----
 X3 PSLLW, PSLLD, and PSLLQ Instruction| X2 X2 << COUNT| X1 X1 << COUNT| X0 DEST Shift Left with Zero Extension
 Operation Using 64-bit Operand        |               |               | X0 << COUNT DEST Figure 4-13.         
 ---  | --- | --- | ---
The (V)PSLLW instruction shifts each of the words in the destination operand
to the left by the number of bits specified in the count operand; the (V)PSLLD
instruction shifts each of the doublewords in the destination operand; and the
(V)PSLLQ instruction shifts the quadword (or quadwords) in the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE instructions: The destination operand is an MMX technology register;
the count operand can be either an MMX technology register or an 64-bit memory
location. 128-bit Legacy SSE version: The destination and first source operands
are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. The count operand can be either an XMM register or a 128-bit
memory location or an 8-bit immediate. If the count operand is a memory address,
128 bits are loaded but the upper 64 bits are ignored.

VEX.128 encoded version: The destination and first source operands are XMM registers.
Bits (VLMAX-1:128) of the destination YMM register are zeroed. The count operand
can be either an XMM register or a 128-bit memory location or an 8-bit immediate.
If the count operand is a memory address, 128 bits are loaded but the upper
64 bits are ignored. VEX.256 encoded version: The destination and first source
operands are YMM registers. The count operand can be either an XMM register
or a 128-bit memory location or an 8-bit immediate.

<aside class="notification">
For shifts with an immediate count (VEX.128.66.0F 71-73 /6), VEX.vvvv
encodes the destination register, and VEX.B + ModRM.r/m encodes the source register.
VEX.L must be 0, otherwise instructions will **``#UD.``**
</aside>



### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4 and 7 for non-VEX-encoded instructions; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
