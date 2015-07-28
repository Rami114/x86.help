## PMINSW - Minimum of Packed Signed Word Integers

> Operation
``` slim

PMINSW (64-bit operands)
  IF DEST[15:0] < SRC[15:0] THEN
     DEST[15:0] <- DEST[15:0];
  ELSE
     DEST[15:0] <- SRC[15:0]; FI;
  (\* Repeat operation for 2nd and 3rd words in source and destination operands \*)
  IF DEST[63:48] < SRC[63:48] THEN
     DEST[63:48] <- DEST[63:48];
  ELSE
     DEST[63:48] <- SRC[63:48]; FI;
PMINSW (128-bit operands)
  IF DEST[15:0] < SRC[15:0] THEN
     DEST[15:0] <- DEST[15:0];
  ELSE
     DEST[15:0] <- SRC[15:0]; FI;
  (\* Repeat operation for 2nd through 7th words in source and destination operands \*)
  IF DEST[127:112] < SRC/m64[127:112] THEN
     DEST[127:112] <- DEST[127:112];
  ELSE
     DEST[127:112] <- SRC[127:112]; FI;
VPMINSW (VEX.128 encoded version)
  IF SRC1[15:0] < SRC2[15:0] THEN
     DEST[15:0] <- SRC1[15:0];
  ELSE
     DEST[15:0] <- SRC2[15:0]; FI;
  (\* Repeat operation for 2nd through 7th words in source and destination operands \*)
  IF SRC1[127:112] < SRC2[127:112] THEN
     DEST[127:112] <- SRC1[127:112];
  ELSE
     DEST[127:112] <- SRC2[127:112]; FI;
DEST[VLMAX-1:128] <- 0
VPMINSW (VEX.256 encoded version)
  IF SRC1[15:0] < SRC2[15:0] THEN
     DEST[15:0] <- SRC1[15:0];
  ELSE
     DEST[15:0] <- SRC2[15:0]; FI;
  (\* Repeat operation for 2nd through 15th words in source and destination operands \*)
  IF SRC1[255:240] < SRC2[255:240] THEN
     DEST[255:240] <- SRC1[255:240];
  ELSE
     DEST[255:240] <- SRC2[255:240]; FI;

```

 Opcode/Instruction                 | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 0F EA /r1 PMINSW mm1, mm2/m64      | RM   | V/V                   | SSE               | Compare signed word integers in mm2/m64  
                                    |      |                       |                   | and mm1 and return minimum values.       
 66 0F EA /r PMINSW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Compare signed word integers in xmm2/m128
                                    |      |                       |                   | and xmm1 and return minimum values.      
 VEX.NDS.128.66.0F.WIG EA /r VPMINSW| RVM  | V/V                   | AVX               | Compare packed signed word integers      
 xmm1, xmm2, xmm3/m128              |      |                       |                   | in xmm3/m128 and xmm2 and return packed  
                                    |      |                       |                   | minimum values in xmm1.                  
 VEX.NDS.256.66.0F.WIG EA /r VPMINSW| RVM  | V/V                   | AVX2              | Compare packed signed word integers      
 ymm1, ymm2, ymm3/m256              |      |                       |                   | in ymm3/m256 and ymm2 and return packed  
                                    |      |                       |                   | minimum values in ymm1.                  
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
Performs a SIMD compare of the packed signed word integers in the destination
operand (first operand) and the source operand (second operand), and returns
the minimum value for each pair of word integers to the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). Legacy SSE version: The source
operand can be an MMX technology register or a 64-bit memory location. The destination
operand can be an MMX technology register.

128-bit Legacy SSE version: The first source and destination operands are XMM
registers. The second source operand is an XMM register or a 128-bit memory
location. Bits (VLMAX-1:128) of the corresponding YMM destination register remain
unchanged. VEX.128 encoded version: The first source and destination operands
are XMM registers. The second source operand is an XMM register or a 128-bit
memory location. Bits (VLMAX-1:128) of the destination YMM register are zeroed.
VEX.256 encoded version: The second source operand can be an YMM register or
a 256-bit memory location. The first source and destination operands are YMM
registers.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PMINSW:   | __m64 _mm_min_pi16 (__m64 a, __m64 b)     
 (V)PMINSW:| __m128i _mm_min_epi16 ( __m128i a, __m128i
           | b)                                        
 VPMINSW:  | __m256i _mm256_min_epi16 ( __m256i a,     
           | __m256i b)                                

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.                       
 #MF| (64-bit operations only) If there is
    | a pending x87 FPU exception.        
