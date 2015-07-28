## PCMPGTB/PCMPGTW/PCMPGTD - Compare Packed Signed Integers for Greater Than

> Operation

``` slim
PCMPGTB (with 64-bit operands)
  IF DEST[7:0] > SRC[7:0]
     THEN DEST[7:0) <- FFH;
     ELSE DEST[7:0] <- 0; FI;
  (\* Continue comparison of 2nd through 7th bytes in DEST and SRC \*)
  IF DEST[63:56] > SRC[63:56]
     THEN DEST[63:56] <- FFH;
     ELSE DEST[63:56] <- 0; FI;
PCMPGTB (with 128-bit operands)
  IF DEST[7:0] > SRC[7:0]
     THEN DEST[7:0) <- FFH;
     ELSE DEST[7:0] <- 0; FI;
  (\* Continue comparison of 2nd through 15th bytes in DEST and SRC \*)
  IF DEST[127:120] > SRC[127:120]
     THEN DEST[127:120] <- FFH;
     ELSE DEST[127:120] <- 0; FI;
VPCMPGTB (VEX.128 encoded version)
DEST[127:0] <-COMPARE_BYTES_GREATER(SRC1,SRC2)
DEST[VLMAX-1:128] <- 0
VPCMPGTB (VEX.256 encoded version)
DEST[127:0] <-COMPARE_BYTES_GREATER(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_BYTES_GREATER(SRC1[255:128],SRC2[255:128])
PCMPGTW (with 64-bit operands)
  IF DEST[15:0] > SRC[15:0]
     THEN DEST[15:0] <- FFFFH;
     ELSE DEST[15:0] <- 0; FI;
  (\* Continue comparison of 2nd and 3rd words in DEST and SRC \*)
  IF DEST[63:48] > SRC[63:48]
     THEN DEST[63:48] <- FFFFH;
     ELSE DEST[63:48] <- 0; FI;
PCMPGTW (with 128-bit operands)
  IF DEST[15:0] > SRC[15:0]
     THEN DEST[15:0] <- FFFFH;
     ELSE DEST[15:0] <- 0; FI;
  (\* Continue comparison of 2nd through 7th words in DEST and SRC \*)
  IF DEST[63:48] > SRC[127:112]
     THEN DEST[127:112] <- FFFFH;
     ELSE DEST[127:112] <- 0; FI;
VPCMPGTW (VEX.128 encoded version)
DEST[127:0] <-COMPARE_WORDS_GREATER(SRC1,SRC2)
DEST[VLMAX-1:128] <- 0
VPCMPGTW (VEX.256 encoded version)
DEST[127:0] <-COMPARE_WORDS_GREATER(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_WORDS_GREATER(SRC1[255:128],SRC2[255:128])
PCMPGTD (with 64-bit operands)
  IF DEST[31:0] > SRC[31:0]
     THEN DEST[31:0] <- FFFFFFFFH;
     ELSE DEST[31:0] <- 0; FI;
  IF DEST[63:32] > SRC[63:32]
     THEN DEST[63:32] <- FFFFFFFFH;
     ELSE DEST[63:32] <- 0; FI;
PCMPGTD (with 128-bit operands)
  IF DEST[31:0] > SRC[31:0]
     THEN DEST[31:0] <- FFFFFFFFH;
     ELSE DEST[31:0] <- 0; FI;
  (\* Continue comparison of 2nd and 3rd doublewords in DEST and SRC \*)
  IF DEST[127:96] > SRC[127:96]
     THEN DEST[127:96] <- FFFFFFFFH;
     ELSE DEST[127:96] <- 0; FI;
VPCMPGTD (VEX.128 encoded version)
DEST[127:0] <-COMPARE_DWORDS_GREATER(SRC1,SRC2)
DEST[VLMAX-1:128] <- 0
VPCMPGTD (VEX.256 encoded version)
DEST[127:0] <-COMPARE_DWORDS_GREATER(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_DWORDS_GREATER(SRC1[255:128],SRC2[255:128])

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PCMPGTB:   | __m64 _mm_cmpgt_pi8 (__m64 m1, __m64  
            | m2)                                   
 PCMPGTW:   | __m64 _mm_pcmpgt_pi16 (__m64 m1, __m64
            | m2)                                   
 DCMPGTD:   | __m64 _mm_pcmpgt_pi32 (__m64 m1, __m64
            | m2)                                   
 (V)PCMPGTB:| __m128i _mm_cmpgt_epi8 ( __m128i a,   
            | __m128i b)                            
 (V)PCMPGTW:| __m128i _mm_cmpgt_epi16 ( __m128i a,  
            | __m128i b)                            
 (V)DCMPGTD:| __m128i _mm_cmpgt_epi32 ( __m128i a,  
            | __m128i b)                            
 VPCMPGTB:  | __m256i _mm256_cmpgt_epi8 ( __m256i   
            | a, __m256i b)                         
 VPCMPGTW:  | __m256i _mm256_cmpgt_epi16 ( __m256i  
            | a, __m256i b)                         
 VPCMPGTD:  | __m256i _mm256_cmpgt_epi32 ( __m256i  
            | a, __m256i b)                         

```

 Opcode/Instruction                  | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 0F 64 /r1 PCMPGTB mm, mm/m64        | RM   | V/V                   | MMX               | Compare packed signed byte integers      
                                     |      |                       |                   | in mm and mm/m64 for greater than.       
 66 0F 64 /r PCMPGTB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Compare packed signed byte integers      
                                     |      |                       |                   | in xmm1 and xmm2/m128 for greater than.  
 0F 65 /r1 PCMPGTW mm, mm/m64        | RM   | V/V                   | MMX               | Compare packed signed word integers      
                                     |      |                       |                   | in mm and mm/m64 for greater than.       
 66 0F 65 /r PCMPGTW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Compare packed signed word integers      
                                     |      |                       |                   | in xmm1 and xmm2/m128 for greater than.  
 0F 66 /r1 PCMPGTD mm, mm/m64        | RM   | V/V                   | MMX               | Compare packed signed doubleword integers
                                     |      |                       |                   | in mm and mm/m64 for greater than.       
 66 0F 66 /r PCMPGTD xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Compare packed signed doubleword integers
                                     |      |                       |                   | in xmm1 and xmm2/m128 for greater than.  
 VEX.NDS.128.66.0F.WIG 64 /r VPCMPGTB| RVM  | V/V                   | AVX               | Compare packed signed byte integers      
 xmm1, xmm2, xmm3/m128               |      |                       |                   | in xmm2 and xmm3/m128 for greater than.  
 VEX.NDS.128.66.0F.WIG 65 /r VPCMPGTW| RVM  | V/V                   | AVX               | Compare packed signed word integers      
 xmm1, xmm2, xmm3/m128               |      |                       |                   | in xmm2 and xmm3/m128 for greater than.  
 VEX.NDS.128.66.0F.WIG 66 /r VPCMPGTD| RVM  | V/V                   | AVX               | Compare packed signed doubleword integers
 xmm1, xmm2, xmm3/m128               |      |                       |                   | in xmm2 and xmm3/m128 for greater than.  
 VEX.NDS.256.66.0F.WIG 64 /r VPCMPGTB| RVM  | V/V                   | AVX2              | Compare packed signed byte integers      
 ymm1, ymm2, ymm3/m256               |      |                       |                   | in ymm2 and ymm3/m256 for greater than.  
 VEX.NDS.256.66.0F.WIG 65 /r VPCMPGTW| RVM  | V/V                   | AVX2              | Compare packed signed word integers      
 ymm1, ymm2, ymm3/m256               |      |                       |                   | in ymm2 and ymm3/m256 for greater than.  
 VEX.NDS.256.66.0F.WIG 66 /r VPCMPGTD| RVM  | V/V                   | AVX2              | Compare packed signed doubleword integers
 ymm1, ymm2, ymm3/m256               |      |                       |                   | in ymm2 and ymm3/m256 for greater than.  
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
Performs an SIMD signed compare for the greater value of the packed byte, word,
or doubleword integers in the destination operand (first operand) and the source
operand (second operand). If a data element in the destination operand is greater
than the corresponding date element in the source operand, the corresponding
data element in the destination operand is set to all 1s; otherwise, it is set
to all 0s.

The PCMPGTB instruction compares the corresponding signed byte integers in the
destination and source operands; the PCMPGTW instruction compares the corresponding
signed word integers in the destination and source

operands; and the PCMPGTD instruction compares the corresponding signed doubleword
integers in the destination and source operands. In 64-bit mode, using a REX
prefix in the form of REX.R permits this instruction to access additional registers
(XMM8-XMM15).

Legacy SSE instructions: The source operand can be an MMX technology register
or a 64-bit memory location. The destination operand can be an MMX technology
register. 128-bit Legacy SSE version: The second source operand can be an XMM
register or a 128-bit memory location. The first source operand and destination
operand are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: The second source operand
can be an XMM register or a 128-bit memory location. The first source operand
and destination operand are XMM registers. Bits (VLMAX-1:128) of the corresponding
YMM register are zeroed. VEX.256 encoded version: The first source operand is
a YMM register. The second source operand is a YMM register or a 256-bit memory
location. The destination operand is a YMM register.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
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
