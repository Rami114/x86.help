## PSHUFB  -  Packed Shuffle Bytes

> Operation

``` slim
PSHUFB (with 64 bit operands)
  for i = 0 to 7 {
     if (SRC[(i \* 8)+7] = 1 ) then
       DEST[(i\*8)+7...(i\*8)+0] <- 0;
     else
       index[2..0] <- SRC[(i\*8)+2 .. (i\*8)+0];
       DEST[(i\*8)+7...(i\*8)+0] <- DEST[(index\*8+7)..(index\*8+0)];
     endif;
  }
PSHUFB (with 128 bit operands)
  for i = 0 to 15 {
     if (SRC[(i \* 8)+7] = 1 ) then
       DEST[(i\*8)+7..(i\*8)+0] <- 0;
     else
       index[3..0] <- SRC[(i\*8)+3 .. (i\*8)+0];
       DEST[(i\*8)+7..(i\*8)+0] <- DEST[(index\*8+7)..(index\*8+0)];
     endif
  }
DEST[VLMAX-1:128] <- 0
VPSHUFB (VEX.128 encoded version)
for i = 0 to 15 {
  if (SRC2[(i \* 8)+7] = 1) then
     DEST[(i\*8)+7..(i\*8)+0] <- 0;
     else
     index[3..0] <- SRC2[(i\*8)+3 .. (i\*8)+0];
     DEST[(i\*8)+7..(i\*8)+0] <- SRC1[(index\*8+7)..(index\*8+0)];
  endif
}
DEST[VLMAX-1:128] <- 0
VPSHUFB (VEX.256 encoded version)
for i = 0 to 15 {
  if (SRC2[(i \* 8)+7] == 1 ) then
     DEST[(i\*8)+7..(i\*8)+0] <- 0;
     else
     index[3..0] <- SRC2[(i\*8)+3 .. (i\*8)+0];
     DEST[(i\*8)+7..(i\*8)+0] <- SRC1[(index\*8+7)..(index\*8+0)];
  endif
  if (SRC2[128 + (i \* 8)+7] == 1 ) then
     DEST[128 + (i\*8)+7..(i\*8)+0] <- 0;
     else
     index[3..0] <- SRC2[128 + (i\*8)+3 .. (i\*8)+0];
     DEST[128 + (i\*8)+7..(i\*8)+0] <- SRC1[128 + (index\*8+7)..(index\*8+0)];
  endif
}
                                 MM2
              07H
                                 MM1
              04H
                                 MM1
              04H
   | |  
---- | -----
 Figure 4-11.| PSHUB with 64-Bit Operands

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PSHUFB:   | __m64 _mm_shuffle_pi8 (__m64 a, __m64
           | b)                                   
 (V)PSHUFB:| __m128i _mm_shuffle_epi8 (__m128i a, 
           | __m128i b)                           
 VPSHUFB:  | __m256i _mm256_shuffle_epi8(__m256i  
           | a, __m256i b)                        

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F 38 00 /r1 PSHUFB mm1, mm2/m64     | RM   | V/V                   | SSSE3             | Shuffle bytes in mm1 according to contents 
                                      |      |                       |                   | of mm2/m64.                                
 66 0F 38 00 /r PSHUFB xmm1, xmm2/m128| RM   | V/V                   | SSSE3             | Shuffle bytes in xmm1 according to contents
                                      |      |                       |                   | of xmm2/m128.                              
 VEX.NDS.128.66.0F38.WIG 00 /r VPSHUFB| RVM  | V/V                   | AVX               | Shuffle bytes in xmm2 according to contents
 xmm1, xmm2, xmm3/m128                |      |                       |                   | of xmm3/m128.                              
 VEX.NDS.256.66.0F38.WIG 00 /r VPSHUFB| RVM  | V/V                   | AVX2              | Shuffle bytes in ymm2 according to contents
 ymm1, ymm2, ymm3/m256                |      |                       |                   | of ymm3/m256.                              
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
PSHUFB performs in-place shuffles of bytes in the destination operand (the first
operand) according to the shuffle control mask in the source operand (the second
operand). The instruction permutes the data in the destination operand, leaving
the shuffle mask unaffected. If the most significant bit (bit[7]) of each byte
of the shuffle control mask is set, then constant zero is written in the result
byte. Each byte in the shuffle control mask forms an index to permute the corresponding
byte in the destination operand. The value of each index is the least significant
4 bits (128-bit operation) or 3 bits (64-bit operation) of the shuffle control
byte. When the source operand is a 128-bit memory operand, the operand must
be aligned on a 16-byte boundary or a general-protection exception (**``#GP)``** will
be generated.

In 64-bit mode, use the REX prefix to access additional registers. Legacy SSE
version: Both operands can be MMX registers.

128-bit Legacy SSE version: The first source operand and the destination operand
are the same. Bits (VLMAX1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: The destination operand is the first
operand, the first source operand is the second operand, the second source operand
is the third operand. Bits (VLMAX-1:128) of the destination YMM register are
zeroed. VEX.256 encoded version: Bits (255:128) of the destination YMM register
stores the 16-byte shuffle result of the upper 16 bytes of the first source
operand, using the upper 16-bytes of the second source operand as control mask.
The value of each index is for the high 128-bit lane is the least significant
4 bits of the respective shuffle control byte. The index value selects a source
data element within each 128-bit lane.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
