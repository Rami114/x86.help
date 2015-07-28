## PMAXUB - Maximum of Packed Unsigned Byte Integers

> Operation
``` slim

PMAXUB (64-bit operands)
  IF DEST[7:0] > SRC[17:0]) THEN
     DEST[7:0] <- DEST[7:0];
  ELSE
     DEST[7:0] <- SRC[7:0]; FI;
  (\* Repeat operation for 2nd through 7th bytes in source and destination operands \*)
  IF DEST[63:56] > SRC[63:56]) THEN
     DEST[63:56] <- DEST[63:56];
  ELSE
     DEST[63:56] <- SRC[63:56]; FI;
PMAXUB (128-bit operands)
  IF DEST[7:0] > SRC[17:0]) THEN
     DEST[7:0] <- DEST[7:0];
  ELSE
     DEST[7:0] <- SRC[7:0]; FI;
  (\* Repeat operation for 2nd through 15th bytes in source and destination operands \*)
  IF DEST[127:120] > SRC[127:120]) THEN
     DEST[127:120] <- DEST[127:120];
  ELSE
     DEST[127:120] <- SRC[127:120]; FI;
VPMAXUB (VEX.128 encoded version)
  IF SRC1[7:0] >SRC2[7:0] THEN
     DEST[7:0] <- SRC1[7:0];
  ELSE
     DEST[7:0] <- SRC2[7:0]; FI;
  (\* Repeat operation for 2nd through 15th bytes in source and destination operands \*)
  IF SRC1[127:120] >SRC2[127:120] THEN
     DEST[127:120] <- SRC1[127:120];
  ELSE
     DEST[127:120] <- SRC2[127:120]; FI;
DEST[VLMAX-1:128] <- 0
VPMAXUB (VEX.256 encoded version)
  IF SRC1[7:0] >SRC2[7:0] THEN
     DEST[7:0] <- SRC1[7:0];
  ELSE
     DEST[15:0] <- SRC2[7:0]; FI;
  (\* Repeat operation for 2nd through 31st bytes in source and destination operands \*)
  IF SRC1[255:248] >SRC2[255:248] THEN
     DEST[255:248] <- SRC1[255:248];
  ELSE
     DEST[255:248] <- SRC2[255:248]; FI;

```

 Opcode/Instruction                 | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F DE /r1 PMAXUB mm1, mm2/m64      | RM   | V/V                   | SSE               | Compare unsigned byte integers in mm2/m64  
                                    |      |                       |                   | and mm1 and returns maximum values.        
 66 0F DE /r PMAXUB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Compare unsigned byte integers in xmm2/m128
                                    |      |                       |                   | and xmm1 and returns maximum values.       
 VEX.NDS.128.66.0F.WIG DE /r VPMAXUB| RVM  | V/V                   | AVX               | Compare packed unsigned byte integers      
 xmm1, xmm2, xmm3/m128              |      |                       |                   | in xmm2 and xmm3/m128 and store packed     
                                    |      |                       |                   | maximum values in xmm1.                    
 VEX.NDS.256.66.0F.WIG DE /r VPMAXUB| RVM  | V/V                   | AVX2              | Compare packed unsigned byte integers      
 ymm1, ymm2, ymm3/m256              |      |                       |                   | in ymm2 and ymm3/m256 and store packed     
                                    |      |                       |                   | maximum values in ymm1.                    
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
Performs a SIMD compare of the packed unsigned byte integers in the destination
operand (first operand) and the source operand (second operand), and returns
the maximum value for each pair of byte integers to the destination operand.

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
 PMAXUB:   | __m64 _mm_max_pu8(__m64 a, __m64 b)      
 (V)PMAXUB:| __m128i _mm_max_epu8 ( __m128i a, __m128i
           | b)                                       
 VPMAXUB:  | __m256i _mm256_max_epu8 ( __m256i a,     
           | __m256i b);                              

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
