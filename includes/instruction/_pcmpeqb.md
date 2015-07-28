## PCMPEQB/PCMPEQW/PCMPEQD -  Compare Packed Data for Equal

> Operation

``` slim
PCMPEQB (with 64-bit operands)
  IF DEST[7:0] = SRC[7:0]
     THEN DEST[7:0) <- FFH;
     ELSE DEST[7:0] <- 0; FI;
  (\* Continue comparison of 2nd through 7th bytes in DEST and SRC \*)
  IF DEST[63:56] = SRC[63:56]
     THEN DEST[63:56] <- FFH;
     ELSE DEST[63:56] <- 0; FI;
PCMPEQB (with 128-bit operands)
  IF DEST[7:0] = SRC[7:0]
     THEN DEST[7:0) <- FFH;
     ELSE DEST[7:0] <- 0; FI;
  (\* Continue comparison of 2nd through 15th bytes in DEST and SRC \*)
  IF DEST[127:120] = SRC[127:120]
     THEN DEST[127:120] <- FFH;
     ELSE DEST[127:120] <- 0; FI;
VPCMPEQB (VEX.128 encoded version)
DEST[127:0] <-COMPARE_BYTES_EQUAL(SRC1[127:0],SRC2[127:0])
DEST[VLMAX-1:128] <- 0
VPCMPEQB (VEX.256 encoded version)
DEST[127:0] <-COMPARE_BYTES_EQUAL(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_BYTES_EQUAL(SRC1[255:128],SRC2[255:128])
PCMPEQW (with 64-bit operands)
  IF DEST[15:0] = SRC[15:0]
     THEN DEST[15:0] <- FFFFH;
     ELSE DEST[15:0] <- 0; FI;
  (\* Continue comparison of 2nd and 3rd words in DEST and SRC \*)
  IF DEST[63:48] = SRC[63:48]
     THEN DEST[63:48] <- FFFFH;
     ELSE DEST[63:48] <- 0; FI;
PCMPEQW (with 128-bit operands)
  IF DEST[15:0] = SRC[15:0]
     THEN DEST[15:0] <- FFFFH;
     ELSE DEST[15:0] <- 0; FI;
  (\* Continue comparison of 2nd through 7th words in DEST and SRC \*)
  IF DEST[127:112] = SRC[127:112]
     THEN DEST[127:112] <- FFFFH;
     ELSE DEST[127:112] <- 0; FI;
VPCMPEQW (VEX.128 encoded version)
DEST[127:0] <-COMPARE_WORDS_EQUAL(SRC1[127:0],SRC2[127:0])
DEST[VLMAX-1:128] <- 0
VPCMPEQW (VEX.256 encoded version)
DEST[127:0] <-COMPARE_WORDS_EQUAL(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_WORDS_EQUAL(SRC1[255:128],SRC2[255:128])
PCMPEQD (with 64-bit operands)
  IF DEST[31:0] = SRC[31:0]
     THEN DEST[31:0] <- FFFFFFFFH;
     ELSE DEST[31:0] <- 0; FI;
  IF DEST[63:32] = SRC[63:32]
     THEN DEST[63:32] <- FFFFFFFFH;
     ELSE DEST[63:32] <- 0; FI;
PCMPEQD (with 128-bit operands)
  IF DEST[31:0] = SRC[31:0]
     THEN DEST[31:0] <- FFFFFFFFH;
     ELSE DEST[31:0] <- 0; FI;
  (\* Continue comparison of 2nd and 3rd doublewords in DEST and SRC \*)
  IF DEST[127:96] = SRC[127:96]
     THEN DEST[127:96] <- FFFFFFFFH;
     ELSE DEST[127:96] <- 0; FI;
VPCMPEQD (VEX.128 encoded version)
DEST[127:0] <-COMPARE_DWORDS_EQUAL(SRC1[127:0],SRC2[127:0])
DEST[VLMAX-1:128] <- 0
VPCMPEQD (VEX.256 encoded version)
DEST[127:0] <-COMPARE_DWORDS_EQUAL(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_DWORDS_EQUAL(SRC1[255:128],SRC2[255:128])

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 0F 74 /r1 PCMPEQB mm, mm/m64          | RM   | V/V                   | MMX               | Compare packed bytes in mm/m64 and mm  
                                       |      |                       |                   | for equality.                          
 66 0F 74 /r PCMPEQB xmm1, xmm2/m128   | RM   | V/V                   | SSE2              | Compare packed bytes in xmm2/m128 and  
                                       |      |                       |                   | xmm1 for equality.                     
 0F 75 /r1 PCMPEQW mm, mm/m64          | RM   | V/V                   | MMX               | Compare packed words in mm/m64 and mm  
                                       |      |                       |                   | for equality.                          
 66 0F 75 /r PCMPEQW xmm1, xmm2/m128   | RM   | V/V                   | SSE2              | Compare packed words in xmm2/m128 and  
                                       |      |                       |                   | xmm1 for equality.                     
 0F 76 /r1 PCMPEQD mm, mm/m64          | RM   | V/V                   | MMX               | Compare packed doublewords in mm/m64   
                                       |      |                       |                   | and mm for equality.                   
 66 0F 76 /r PCMPEQD xmm1, xmm2/m128   | RM   | V/V                   | SSE2              | Compare packed doublewords in xmm2/m128
                                       |      |                       |                   | and xmm1 for equality.                 
 VEX.NDS.128.66.0F.WIG 74 /r VPCMPEQB  | RVM  | V/V                   | AVX               | Compare packed bytes in xmm3/m128 and  
 xmm1, xmm2, xmm3/m128                 |      |                       |                   | xmm2 for equality.                     
 VEX.NDS.128.66.0F.WIG 75 /r VPCMPEQW  | RVM  | V/V                   | AVX               | Compare packed words in xmm3/m128 and  
 xmm1, xmm2, xmm3/m128                 |      |                       |                   | xmm2 for equality.                     
 VEX.NDS.128.66.0F.WIG 76 /r VPCMPEQD  | RVM  | V/V                   | AVX               | Compare packed doublewords in xmm3/m128
 xmm1, xmm2, xmm3/m128                 |      |                       |                   | and xmm2 for equality.                 
 VEX.NDS.256.66.0F.WIG 75 /r VPCMPEQW  | RVM  | V/V                   | AVX2              | Compare packed words in ymm3/m256 and  
 ymm1, ymm2, ymm3 /m256                |      |                       |                   | ymm2 for equality.                     
 VEX.NDS.256.66.0F.WIG 76 /r VPCMPEQD  | RVM  | V/V                   | AVX2              | Compare packed doublewords in ymm3/m256
 ymm1, ymm2, ymm3 /m256                |      |                       |                   | and ymm2 for equality.                 
 VEX.NDS.256.66.0F38.WIG 29 /r VPCMPEQQ| RVM  | V/V                   | AVX2              | Compare packed quadwords in ymm3/m256  
 ymm1, ymm2, ymm3 /m256                |      |                       |                   | and ymm2 for equality.                 
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
Performs a SIMD compare for equality of the packed bytes, words, or doublewords
in the destination operand (first operand) and the source operand (second operand).
If a pair of data elements is equal, the corresponding data element in the destination
operand is set to all 1s; otherwise, it is set to all 0s.

The (V)PCMPEQB instruction compares the corresponding bytes in the destination
and source operands; the (V)PCMPEQW instruction compares the corresponding words
in the destination and source operands; and the (V)PCMPEQD instruction compares
the corresponding doublewords in the destination and source operands.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE instructions: The source operand can be an MMX technology register
or a 64-bit memory location. The destination operand can be an MMX technology
register. 128-bit Legacy SSE version: The second source operand can be an XMM
register or a 128-bit memory location. The first source and destination operands
are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: The second source operand can be
an XMM register or a 128-bit memory location. The first source and destination
operands are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM register
are zeroed. VEX.256 encoded version: The first source operand is a YMM register.
The second source operand is a YMM register or a 256-bit memory location. The
destination operand is a YMM register. Note: VEX.L must be 0, otherwise the
instruction will #UD.



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PCMPEQB:   | __m64 _mm_cmpeq_pi8 (__m64 m1, __m64 
            | m2)                                  
 PCMPEQW:   | __m64 _mm_cmpeq_pi16 (__m64 m1, __m64
            | m2)                                  
 PCMPEQD:   | __m64 _mm_cmpeq_pi32 (__m64 m1, __m64
            | m2)                                  
 (V)PCMPEQB:| __m128i _mm_cmpeq_epi8 ( __m128i a,  
            | __m128i b)                           
 (V)PCMPEQW:| __m128i _mm_cmpeq_epi16 ( __m128i a, 
            | __m128i b)                           
 (V)PCMPEQD:| __m128i _mm_cmpeq_epi32 ( __m128i a, 
            | __m128i b)                           
 VPCMPEQB:  | __m256i _mm256_cmpeq_epi8 ( __m256i  
            | a, __m256i b)                        
 VPCMPEQW:  | __m256i _mm256_cmpeq_epi16 ( __m256i 
            | a, __m256i b)                        
 VPCMPEQD:  | __m256i _mm256_cmpeq_epi32 ( __m256i 
            | a, __m256i b)                        

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
