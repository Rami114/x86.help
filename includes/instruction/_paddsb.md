## PADDSB/PADDSW - Add Packed Signed Integers with Signed Saturation

> Operation

``` slim
PADDSB (with 64-bit operands)
  DEST[7:0] <- SaturateToSignedByte(DEST[7:0] + SRC (7:0]);
  (\* Repeat add operation for 2nd through 7th bytes \*)
  DEST[63:56] <- SaturateToSignedByte(DEST[63:56] + SRC[63:56] );
PADDSB (with 128-bit operands)
  DEST[7:0] <-SaturateToSignedByte (DEST[7:0] + SRC[7:0]);
  (\* Repeat add operation for 2nd through 14th bytes \*)
  DEST[127:120] <- SaturateToSignedByte (DEST[111:120] + SRC[127:120]);
VPADDSB (VEX.128 encoded version)
  DEST[7:0] <- SaturateToSignedByte (SRC1[7:0] + SRC2[7:0]);
  (\* Repeat subtract operation for 2nd through 14th bytes \*)
  DEST[127:120] <- SaturateToSignedByte (SRC1[111:120] + SRC2[127:120]);
  DEST[VLMAX-1:128] <- 0
VPADDSB (VEX.256 encoded version)
  DEST[7:0] <- SaturateToSignedByte (SRC1[7:0] + SRC2[7:0]);
  (\* Repeat add operation for 2nd through 31st bytes \*)
  DEST[255:248]<- SaturateToSignedByte (SRC1[255:248] + SRC2[255:248]);
PADDSW (with 64-bit operands)
  DEST[15:0] <- SaturateToSignedWord(DEST[15:0] + SRC[15:0] );
  (\* Repeat add operation for 2nd and 7th words \*)
  DEST[63:48] <- SaturateToSignedWord(DEST[63:48] + SRC[63:48] );
PADDSW (with 128-bit operands)
  DEST[15:0]
  (\* Repeat add operation for 2nd through 7th words \*)
  DEST[127:112] <- SaturateToSignedWord (DEST[127:112] + SRC[127:112]);
VPADDSW (VEX.128 encoded version)
  DEST[15:0] <- SaturateToSignedWord (SRC1[15:0] + SRC2[15:0]);
  (\* Repeat subtract operation for 2nd through 7th words \*)
  DEST[127:112] <- SaturateToSignedWord (SRC1[127:112] + SRC2[127:112]);
  DEST[VLMAX-1:128] <- 0
VPADDSW (VEX.256 encoded version)
  DEST[15:0] <- SaturateToSignedWord (SRC1[15:0] + SRC2[15:0]);
  (\* Repeat add operation for 2nd through 15th words \*)
  DEST[255:240] <- SaturateToSignedWord (SRC1[255:240] + SRC2[255:240])

```

 Opcode/Instruction                 | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F EC /r1 PADDSB mm, mm/m64        | RM   | V/V                   | MMX               | Add packed signed byte integers from       
                                    |      |                       |                   | mm/m64 and mm and saturate the results.    
 66 0F EC /r PADDSB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Add packed signed byte integers from       
                                    |      |                       |                   | xmm2/m128 and xmm1 saturate the results.   
 0F ED /r1 PADDSW mm, mm/m64        | RM   | V/V                   | MMX               | Add packed signed word integers from       
                                    |      |                       |                   | mm/m64 and mm and saturate the results.    
 66 0F ED /r PADDSW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Add packed signed word integers from       
                                    |      |                       |                   | xmm2/m128 and xmm1 and saturate the        
                                    |      |                       |                   | results.                                   
 VEX.NDS.128.66.0F.WIG EC /r VPADDSB| RVM  | V/V                   | AVX               | Add packed signed byte integers from       
 xmm1, xmm2, xmm3/m128              |      |                       |                   | xmm3/m128 and xmm2 saturate the results.   
 VEX.NDS.128.66.0F.WIG ED /r VPADDSW| RVM  | V/V                   | AVX               | Add packed signed word integers from       
 xmm1, xmm2, xmm3/m128              |      |                       |                   | xmm3/m128 and xmm2 and saturate the        
                                    |      |                       |                   | results.                                   
 VEX.NDS.256.66.0F.WIG EC /r VPADDSB| RVM  | V/V                   | AVX2              | Add packed signed byte integers from       
 ymm1, ymm2, ymm3/m256              |      |                       |                   | ymm2, and ymm3/m256 and store the saturated
                                    |      |                       |                   | results in ymm1.                           
 VEX.NDS.256.66.0F.WIG ED /r VPADDSW| RVM  | V/V                   | AVX2              | Add packed signed word integers from       
 ymm1, ymm2, ymm3/m256              |      |                       |                   | ymm2, and ymm3/m256 and store the saturated
                                    |      |                       |                   | results in ymm1.                           
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
Performs a SIMD add of the packed signed integers from the source operand (second
operand) and the destination operand (first operand), and stores the packed
integer results in the destination operand. See Figure 9-4 in the Intel® 64
and IA-32 Architectures Software Developer's Manual, Volume 1, for an illustration
of a SIMD operation. Overflow is handled with signed saturation, as described
in the following paragraphs.

The PADDSB instruction adds packed signed byte integers. When an individual
byte result is beyond the range of a signed byte integer (that is, greater than
7FH or less than 80H), the saturated value of 7FH or 80H, respectively, is written
to the destination operand.

The PADDSW instruction adds packed signed word integers. When an individual
word result is beyond the range of a signed word integer (that is, greater than
7FFFH or less than 8000H), the saturated value of 7FFFH or 8000H, respectively,
is written to the destination operand.

These instructions can operate on either 64-bit, 128-bit or 256-bit operands.
When operating on 64-bit operands, the destination operand must be an MMX technology
register and the source operand can be either an MMX technology register or
a 64-bit memory location. In 64-bit mode, using a REX prefix in the form of
REX.R permits this instruction to access additional registers (XMM8-XMM15).

128-bit Legacy SSE version: The first source operand is an XMM register. The
second operand can be an XMM register or a 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: The first source operand is an XMM register. The second source operand
is an XMM register or 128-bit memory location. The destination operand is an
XMM register. The upper bits (VLMAX-1:128) of the corresponding YMM register
destination are zeroed. VEX.256 encoded version: The first source operand is
a YMM register. The second source operand is a YMM register or a 256-bit memory
location. The destination operand is a YMM register.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PADDSB:   | __m64 _mm_adds_pi8(__m64 m1, __m64 m2)    
 (V)PADDSB:| __m128i _mm_adds_epi8 ( __m128i a, __m128i
           | b)                                        
 VPADDSB:  | __m256i _mm256_adds_epi8 ( __m256i a,     
           | __m256i b)                                
 PADDSW:   | __m64 _mm_adds_pi16(__m64 m1, __m64       
           | m2)                                       
 (V)PADDSW:| __m128i _mm_adds_epi16 ( __m128i a,       
           | __m128i b)                                
 VPADDSW:  | __m256i _mm256_adds_epi16 ( __m256i       
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
