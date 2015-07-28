## PSUBSB/PSUBSW - Subtract Packed Signed Integers with Signed Saturation

> Operation

``` slim
PSUBSB (with 64-bit operands)
  DEST[7:0] <- SaturateToSignedByte (DEST[7:0] − SRC (7:0]);
  (\* Repeat subtract operation for 2nd through 7th bytes \*)
  DEST[63:56] <- SaturateToSignedByte (DEST[63:56] − SRC[63:56] );
PSUBSB (with 128-bit operands)
  DEST[7:0] <- SaturateToSignedByte (DEST[7:0] − SRC[7:0]);
  (\* Repeat subtract operation for 2nd through 14th bytes \*)
  DEST[127:120] <- SaturateToSignedByte (DEST[127:120] − SRC[127:120]);
VPSUBSB (VEX.128 encoded version)
DEST[7:0] <- SaturateToSignedByte (SRC1[7:0] - SRC2[7:0]);
(\* Repeat subtract operation for 2nd through 14th bytes \*)
DEST[127:120] <- SaturateToSignedByte (SRC1[127:120] - SRC2[127:120]);
DEST[VLMAX-1:128] <- 0
VPSUBSB (VEX.256 encoded version)
DEST[7:0] <- SaturateToSignedByte (SRC1[7:0] - SRC2[7:0]);
(\* Repeat subtract operation for 2nd through 31th bytes \*)
DEST[255:248] <- SaturateToSignedByte (SRC1[255:248] - SRC2[255:248]);
PSUBSW (with 64-bit operands)
  DEST[15:0] <- SaturateToSignedWord (DEST[15:0] − SRC[15:0] );
  (\* Repeat subtract operation for 2nd and 7th words \*)
  DEST[63:48] <- SaturateToSignedWord (DEST[63:48] − SRC[63:48] );
PSUBSW (with 128-bit operands)
  DEST[15:0]
  (\* Repeat subtract operation for 2nd through 7th words \*)
  DEST[127:112] <- SaturateToSignedWord (DEST[127:112] − SRC[127:112]);
VPSUBSW (VEX.128 encoded version)
DEST[15:0] <- SaturateToSignedWord (SRC1[15:0] - SRC2[15:0]);
(\* Repeat subtract operation for 2nd through 7th words \*)
DEST[127:112] <- SaturateToSignedWord (SRC1[127:112] - SRC2[127:112]);
DEST[VLMAX-1:128] <- 0
VPSUBSW (VEX.256 encoded version)
DEST[15:0] <- SaturateToSignedWord (SRC1[15:0] - SRC2[15:0]);
(\* Repeat subtract operation for 2nd through 15th words \*)
DEST[255:240] <- SaturateToSignedWord (SRC1[255:240] - SRC2[255:240]);

```

 Opcode/Instruction                 | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F E8 /r1 PSUBSB mm, mm/m64        | RM   | V/V                   | MMX               | Subtract signed packed bytes in mm/m64     
                                    |      |                       |                   | from signed packed bytes in mm and saturate
                                    |      |                       |                   | results.                                   
 66 0F E8 /r PSUBSB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Subtract packed signed byte integers       
                                    |      |                       |                   | in xmm2/m128 from packed signed byte       
                                    |      |                       |                   | integers in xmm1 and saturate results.     
 0F E9 /r1 PSUBSW mm, mm/m64        | RM   | V/V                   | MMX               | Subtract signed packed words in mm/m64     
                                    |      |                       |                   | from signed packed words in mm and saturate
                                    |      |                       |                   | results.                                   
 66 0F E9 /r PSUBSW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Subtract packed signed word integers       
                                    |      |                       |                   | in xmm2/m128 from packed signed word       
                                    |      |                       |                   | integers in xmm1 and saturate results.     
 VEX.NDS.128.66.0F.WIG E8 /r VPSUBSB| RVM  | V/V                   | AVX               | Subtract packed signed byte integers       
 xmm1, xmm2, xmm3/m128              |      |                       |                   | in xmm3/m128 from packed signed byte       
                                    |      |                       |                   | integers in xmm2 and saturate results.     
 VEX.NDS.128.66.0F.WIG E9 /r VPSUBSW| RVM  | V/V                   | AVX               | Subtract packed signed word integers       
 xmm1, xmm2, xmm3/m128              |      |                       |                   | in xmm3/m128 from packed signed word       
                                    |      |                       |                   | integers in xmm2 and saturate results.     
 VEX.NDS.256.66.0F.WIG E8 /r VPSUBSB| RVM  | V/V                   | AVX2              | Subtract packed signed byte integers       
 ymm1, ymm2, ymm3/m256              |      |                       |                   | in ymm3/m256 from packed signed byte       
                                    |      |                       |                   | integers in ymm2 and saturate results.     
 VEX.NDS.256.66.0F.WIG E9 /r VPSUBSW| RVM  | V/V                   | AVX2              | Subtract packed signed word integers       
 ymm1, ymm2, ymm3/m256              |      |                       |                   | in ymm3/m256 from packed signed word       
                                    |      |                       |                   | integers in ymm2 and saturate results.     
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
Performs a SIMD subtract of the packed signed integers of the source operand
(second operand) from the packed signed integers of the destination operand
(first operand), and stores the packed integer results in the destination operand.
See Figure 9-4 in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 1, for an illustration of a SIMD operation. Overflow is handled
with signed saturation, as described in the following paragraphs.

The (V)PSUBSB instruction subtracts packed signed byte integers. When an individual
byte result is beyond the range of a signed byte integer (that is, greater than
7FH or less than 80H), the saturated value of 7FH or 80H, respectively, is written
to the destination operand.

The (V)PSUBSW instruction subtracts packed signed word integers. When an individual
word result is beyond the range of a signed word integer (that is, greater than
7FFFH or less than 8000H), the saturated value of 7FFFH or 8000H, respectively,
is written to the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE version: When operating on 64-bit operands, the destination operand
must be an MMX technology register and the source operand can be either an MMX
### technology register or a 64-bit memory location. 128-bit Legacy SSE version
The second source operand is an XMM register or a 128-bit memory location. The
first source operand and destination operands are XMM registers. Bits (VLMAX-1:128)
of the corresponding YMM destination register remain unchanged. VEX.128 encoded
version: The second source operand is an XMM register or a 128-bit memory location.
The first source operand and destination operands are XMM registers. Bits (VLMAX-1:128)
of the destination YMM register are zeroed. VEX.256 encoded version: The second
source operand is an YMM register or a 256-bit memory location. The first source
operand and destination operands are YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PSUBSB:   | __m64 _mm_subs_pi8(__m64 m1, __m64 m2)    
 (V)PSUBSB:| __m128i _mm_subs_epi8(__m128i m1, __m128i 
           | m2)                                       
 VPSUBSB:  | __m256i _mm256_subs_epi8(__m256i m1,      
           | __m256i m2)                               
 PSUBSW:   | __m64 _mm_subs_pi16(__m64 m1, __m64       
           | m2)                                       
 (V)PSUBSW:| __m128i _mm_subs_epi16(__m128i m1, __m128i
           | m2)                                       
 VPSUBSW:  | __m256i _mm256_subs_epi16(__m256i m1,     
           | __m256i m2)                               

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
