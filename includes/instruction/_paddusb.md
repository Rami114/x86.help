## PADDUSB/PADDUSW - Add Packed Unsigned Integers with Unsigned Saturation

> Operation
``` slim

PADDUSB (with 64-bit operands)
  DEST[7:0] <- SaturateToUnsignedByte(DEST[7:0] + SRC (7:0] );
  (\* Repeat add operation for 2nd through 7th bytes \*)
  DEST[63:56] <- SaturateToUnsignedByte(DEST[63:56] + SRC[63:56]
PADDUSB (with 128-bit operands)
  DEST[7:0] <- SaturateToUnsignedByte (DEST[7:0] + SRC[7:0]);
  (\* Repeat add operation for 2nd through 14th bytes \*)
  DEST[127:120] <- SaturateToUnSignedByte (DEST[127:120] + SRC[127:120]);
VPADDUSB (VEX.128 encoded version)
  DEST[7:0] <- SaturateToUnsignedByte (SRC1[7:0] + SRC2[7:0]);
  (\* Repeat subtract operation for 2nd through 14th bytes \*)
  DEST[127:120] <- SaturateToUnsignedByte (SRC1[111:120] + SRC2[127:120]);
  DEST[VLMAX-1:128] <- 0
VPADDUSB (VEX.256 encoded version)
  DEST[7:0] <- SaturateToUnsignedByte (SRC1[7:0] + SRC2[7:0]);
  (\* Repeat add operation for 2nd through 31st bytes \*)
  DEST[255:248]<- SaturateToUnsignedByte (SRC1[255:248] + SRC2[255:248]);
PADDUSW (with 64-bit operands)
  DEST[15:0] <- SaturateToUnsignedWord(DEST[15:0] + SRC[15:0] );
  (\* Repeat add operation for 2nd and 3rd words \*)
  DEST[63:48] <- SaturateToUnsignedWord(DEST[63:48] + SRC[63:48] );
PADDUSW (with 128-bit operands)
  DEST[15:0] <- SaturateToUnsignedWord (DEST[15:0] + SRC[15:0]);
  (\* Repeat add operation for 2nd through 7th words \*)
  DEST[127:112] <- SaturateToUnSignedWord (DEST[127:112] + SRC[127:112]);
VPADDUSW (VEX.128 encoded version)
  DEST[15:0] <- SaturateToUnsignedWord (SRC1[15:0] + SRC2[15:0]);
  (\* Repeat subtract operation for 2nd through 7th words \*)
  DEST[127:112] <- SaturateToUnsignedWord (SRC1[127:112] + SRC2[127:112]);
  DEST[VLMAX-1:128] <- 0
VPADDUSW (VEX.256 encoded version)
  DEST[15:0] <- SaturateToUnsignedWord (SRC1[15:0] + SRC2[15:0]);
  (\* Repeat add operation for 2nd through 15th words \*)
  DEST[255:240] <- SaturateToUnsignedWord (SRC1[255:240] + SRC2[255:240])

```

 Opcode/Instruction                  | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F DC /r1 PADDUSB mm, mm/m64        | RM   | V/V                   | MMX               | Add packed unsigned byte integers from     
                                     |      |                       |                   | mm/m64 and mm and saturate the results.    
 66 0F DC /r PADDUSB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Add packed unsigned byte integers from     
                                     |      |                       |                   | xmm2/m128 and xmm1 saturate the results.   
 0F DD /r1 PADDUSW mm, mm/m64        | RM   | V/V                   | MMX               | Add packed unsigned word integers from     
                                     |      |                       |                   | mm/m64 and mm and saturate the results.    
 66 0F DD /r PADDUSW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Add packed unsigned word integers from     
                                     |      |                       |                   | xmm2/m128 to xmm1 and saturate the results.
 VEX.NDS.128.660F.WIG DC /r VPADDUSB | RVM  | V/V                   | AVX               | Add packed unsigned byte integers from     
 xmm1, xmm2, xmm3/m128               |      |                       |                   | xmm3/m128 to xmm2 and saturate the results.
 VEX.NDS.128.66.0F.WIG DD /r VPADDUSW| RVM  | V/V                   | AVX               | Add packed unsigned word integers from     
 xmm1, xmm2, xmm3/m128               |      |                       |                   | xmm3/m128 to xmm2 and saturate the results.
 VEX.NDS.256.66.0F.WIG DC /r VPADDUSB| RVM  | V/V                   | AVX2              | Add packed unsigned byte integers from     
 ymm1, ymm2, ymm3/m256               |      |                       |                   | ymm2, and ymm3/m256 and store the saturated
                                     |      |                       |                   | results in ymm1.                           
 VEX.NDS.256.66.0F.WIG DD /r VPADDUSW| RVM  | V/V                   | AVX2              | Add packed unsigned word integers from     
 ymm1, ymm2, ymm3/m256               |      |                       |                   | ymm2, and ymm3/m256 and store the saturated
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
Performs a SIMD add of the packed unsigned integers from the source operand
(second operand) and the destination operand (first operand), and stores the
packed integer results in the destination operand. See Figure 9-4 in the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1, for an illustration
of a SIMD operation. Overflow is handled with unsigned saturation, as described
in the following paragraphs.

The (V)PADDUSB instruction adds packed unsigned byte integers. When an individual
byte result is beyond the range of an unsigned byte integer (that is, greater
than FFH), the saturated value of FFH is written to the destination operand.

The (V)PADDUSW instruction adds packed unsigned word integers. When an individual
word result is beyond the range of an unsigned word integer (that is, greater
than FFFFH), the saturated value of FFFFH is written to the destination operand.

These instructions can operate on either 64-bit, 128-bit or 256-bit operands.
When operating on 64-bit operands, the destination operand must be an MMX technology
register and the source operand can be either an MMX tech-

nology register or a 64-bit memory location. In 64-bit mode, using a REX prefix
in the form of REX.R permits this instruction to access additional registers
(XMM8-XMM15). 128-bit Legacy SSE version: The first source operand is an XMM
register. The second operand can be an XMM register or a 128-bit memory location.
The destination is not distinct from the first source XMM register and the upper
bits (VLMAX-1:128) of the corresponding YMM register destination are unmodified.

VEX.128 encoded version: The first source operand is an XMM register. The second
source operand is an XMM register or 128-bit memory location. The destination
operand is an XMM register. The upper bits (VLMAX-1:128) of the corresponding
YMM register destination are zeroed. VEX.256 encoded version: The first source
operand is a YMM register. The second source operand is a YMM register or a
256-bit memory location. The destination operand is a YMM register. Note: VEX.L
must be 0, otherwise the instruction will #UD.



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PADDUSB:   | __m64 _mm_adds_pu8(__m64 m1, __m64 m2)    
 PADDUSW:   | __m64 _mm_adds_pu16(__m64 m1, __m64       
            | m2)                                       
 (V)PADDUSB:| __m128i _mm_adds_epu8 ( __m128i a, __m128i
            | b)                                        
 (V)PADDUSW:| __m128i _mm_adds_epu16 ( __m128i a,       
            | __m128i b)                                
 VPADDUSB:  | __m256i _mm256_adds_epu8 ( __m256i a,     
            | __m256i b)                                
 VPADDUSW:  | __m256i _mm256_adds_epu16 ( __m256i       
            | a, __m256i b)                             

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
