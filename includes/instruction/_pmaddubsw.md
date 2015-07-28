## PMADDUBSW  -  Multiply and Add Packed Signed and Unsigned Bytes

> Operation

``` slim
PMADDUBSW (with 64 bit operands)
  DEST[15-0] = SaturateToSignedWord(SRC[15-8]\*DEST[15-8]+SRC[7-0]\*DEST[7-0]);
  DEST[31-16] = SaturateToSignedWord(SRC[31-24]\*DEST[31-24]+SRC[23-16]\*DEST[23-16]);
  DEST[47-32] = SaturateToSignedWord(SRC[47-40]\*DEST[47-40]+SRC[39-32]\*DEST[39-32]);
  DEST[63-48] = SaturateToSignedWord(SRC[63-56]\*DEST[63-56]+SRC[55-48]\*DEST[55-48]);
PMADDUBSW (with 128 bit operands)
  DEST[15-0] = SaturateToSignedWord(SRC[15-8]\* DEST[15-8]+SRC[7-0]\*DEST[7-0]);
  // Repeat operation for 2nd through 7th word
  SRC1/DEST[127-112] = SaturateToSignedWord(SRC[127-120]\*DEST[127-120]+ SRC[119-112]\* DEST[119-112]);
VPMADDUBSW (VEX.128 encoded version)
DEST[15:0] <- SaturateToSignedWord(SRC2[15:8]\* SRC1[15:8]+SRC2[7:0]\*SRC1[7:0])
// Repeat operation for 2nd through 7th word
DEST[127:112] <- SaturateToSignedWord(SRC2[127:120]\*SRC1[127:120]+ SRC2[119:112]\* SRC1[119:112])
DEST[VLMAX-1:128] <- 0
VPMADDUBSW (VEX.256 encoded version)
DEST[15:0] <- SaturateToSignedWord(SRC2[15:8]\* SRC1[15:8]+SRC2[7:0]\*SRC1[7:0])
// Repeat operation for 2nd through 15th word
DEST[255:240] <- SaturateToSignedWord(SRC2[255:248]\*SRC1[255:248]+ SRC2[247:240]\* SRC1[247:240])

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PMADDUBSW:   | __m64 _mm_maddubs_pi16 (__m64 a, __m64
              | b)                                    
 (V)PMADDUBSW:| __m128i _mm_maddubs_epi16 (__m128i a, 
              | __m128i b)                            
 VPMADDUBSW:  | __m256i _mm256_maddubs_epi16 (__m256i 
              | a, __m256i b)                         

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                         
 ---  | --- | --- | --- | ---
 0F 38 04 /r1 PMADDUBSW mm1, mm2/m64     | RM   | V/V                   | SSSE3             | Multiply signed and unsigned bytes, 
                                         |      |                       |                   | add horizontal pair of signed words,
                                         |      |                       |                   | pack saturated signed-words to mm1. 
 66 0F 38 04 /r PMADDUBSW xmm1, xmm2/m128| RM   | V/V                   | SSSE3             | Multiply signed and unsigned bytes, 
                                         |      |                       |                   | add horizontal pair of signed words,
                                         |      |                       |                   | pack saturated signed-words to xmm1.
 VEX.NDS.128.66.0F38.WIG 04 /r VPMADDUBSW| RVM  | V/V                   | AVX               | Multiply signed and unsigned bytes, 
 xmm1, xmm2, xmm3/m128                   |      |                       |                   | add horizontal pair of signed words,
                                         |      |                       |                   | pack saturated signed-words to xmm1.
 VEX.NDS.256.66.0F38.WIG 04 /r VPMADDUBSW| RVM  | V/V                   | AVX2              | Multiply signed and unsigned bytes, 
 ymm1, ymm2, ymm3/m256                   |      |                       |                   | add horizontal pair of signed words,
                                         |      |                       |                   | pack saturated signed-words to ymm1.
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
(V)PMADDUBSW multiplies vertically each unsigned byte of the destination operand
(first operand) with the corresponding signed byte of the source operand (second
operand), producing intermediate signed 16-bit integers. Each adjacent pair
of signed words is added and the saturated result is packed to the destination
operand. For example, the lowest-order bytes (bits 7-0) in the source and destination
operands are multiplied and the intermediate signed word result is added with
the corresponding intermediate result from the 2nd lowest-order bytes (bits
15-8) of the operands; the sign-saturated result is stored in the lowest word
of the destination register (15-0). The same operation is performed on the other
pairs of adjacent bytes. Both operands can be MMX register or XMM registers.
When the source operand is a 128-bit memory operand, the operand must be aligned
on a 16-byte boundary or a general-protection exception (**``#GP)``** will be generated.

In 64-bit mode, use the REX prefix to access additional registers. 128-bit Legacy
SSE version: Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: Bits (VLMAX-1:128) of the destination
YMM register are zeroed. VEX.256 encoded version: The first source and destination
operands are YMM registers. The second source operand can be an YMM register
or a 256-bit memory location.

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
