## PSUBUSB/PSUBUSW - Subtract Packed Unsigned Integers with Unsigned Saturation

> Operation

``` slim
PSUBUSB (with 64-bit operands)
  DEST[7:0] <- SaturateToUnsignedByte (DEST[7:0] − SRC (7:0] );
  (\* Repeat add operation for 2nd through 7th bytes \*)
  DEST[63:56] <- SaturateToUnsignedByte (DEST[63:56] − SRC[63:56];
PSUBUSB (with 128-bit operands)
  DEST[7:0] <- SaturateToUnsignedByte (DEST[7:0] − SRC[7:0]);
  (\* Repeat add operation for 2nd through 14th bytes \*)
  DEST[127:120] <- SaturateToUnSignedByte (DEST[127:120] − SRC[127:120]);
VPSUBUSB (VEX.128 encoded version)
DEST[7:0] <- SaturateToUnsignedByte (SRC1[7:0] - SRC2[7:0]);
(\* Repeat subtract operation for 2nd through 14th bytes \*)
DEST[127:120] <- SaturateToUnsignedByte (SRC1[127:120] - SRC2[127:120]);
DEST[VLMAX-1:128] <- 0
VPSUBUSB (VEX.256 encoded version)
DEST[7:0] <- SaturateToUnsignedByte (SRC1[7:0] - SRC2[7:0]);
(\* Repeat subtract operation for 2nd through 31st bytes \*)
DEST[255:148] <- SaturateToUnsignedByte (SRC1[255:248] - SRC2[255:248]);
PSUBUSW (with 64-bit operands)
  DEST[15:0] <- SaturateToUnsignedWord (DEST[15:0] − SRC[15:0] );
  (\* Repeat add operation for 2nd and 3rd words \*)
  DEST[63:48] <- SaturateToUnsignedWord (DEST[63:48] − SRC[63:48] );
PSUBUSW (with 128-bit operands)
  DEST[15:0]
  (\* Repeat add operation for 2nd through 7th words \*)
  DEST[127:112] <- SaturateToUnSignedWord (DEST[127:112] − SRC[127:112]);
VPSUBUSW (VEX.128 encoded version)
DEST[15:0] <- SaturateToUnsignedWord (SRC1[15:0] - SRC2[15:0]);
(\* Repeat subtract operation for 2nd through 7th words \*)
DEST[127:112] <- SaturateToUnsignedWord (SRC1[127:112] - SRC2[127:112]);
DEST[VLMAX-1:128] <- 0
VPSUBUSW (VEX.256 encoded version)
DEST[15:0] <- SaturateToUnsignedWord (SRC1[15:0] - SRC2[15:0]);
(\* Repeat subtract operation for 2nd through 15th words \*)
DEST[255:240] <- SaturateToUnsignedWord (SRC1[255:240] - SRC2[255:240]);

```

 Opcode/Instruction                  | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F D8 /r1 PSUBUSB mm, mm/m64        | RM   | V/V                   | MMX               | Subtract unsigned packed bytes in mm/m64
                                     |      |                       |                   | from unsigned packed bytes in mm and    
                                     |      |                       |                   | saturate result.                        
 66 0F D8 /r PSUBUSB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Subtract packed unsigned byte integers  
                                     |      |                       |                   | in xmm2/m128 from packed unsigned byte  
                                     |      |                       |                   | integers in xmm1 and saturate result.   
 0F D9 /r1 PSUBUSW mm, mm/m64        | RM   | V/V                   | MMX               | Subtract unsigned packed words in mm/m64
                                     |      |                       |                   | from unsigned packed words in mm and    
                                     |      |                       |                   | saturate result.                        
 66 0F D9 /r PSUBUSW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Subtract packed unsigned word integers  
                                     |      |                       |                   | in xmm2/m128 from packed unsigned word  
                                     |      |                       |                   | integers in xmm1 and saturate result.   
 VEX.NDS.128.66.0F.WIG D8 /r VPSUBUSB| RVM  | V/V                   | AVX               | Subtract packed unsigned byte integers  
 xmm1, xmm2, xmm3/m128               |      |                       |                   | in xmm3/m128 from packed unsigned byte  
                                     |      |                       |                   | integers in xmm2 and saturate result.   
 VEX.NDS.128.66.0F.WIG D9 /r VPSUBUSW| RVM  | V/V                   | AVX               | Subtract packed unsigned word integers  
 xmm1, xmm2, xmm3/m128               |      |                       |                   | in xmm3/m128 from packed unsigned word  
                                     |      |                       |                   | integers in xmm2 and saturate result.   
 VEX.NDS.256.66.0F.WIG D8 /r VPSUBUSB| RVM  | V/V                   | AVX2              | Subtract packed unsigned byte integers  
 ymm1, ymm2, ymm3/m256               |      |                       |                   | in ymm3/m256 from packed unsigned byte  
                                     |      |                       |                   | integers in ymm2 and saturate result.   
 VEX.NDS.256.66.0F.WIG D9 /r VPSUBUSW| RVM  | V/V                   | AVX2              | Subtract packed unsigned word integers  
 ymm1, ymm2, ymm3/m256               |      |                       |                   | in ymm3/m256 from packed unsigned word  
                                     |      |                       |                   | integers in ymm2 and saturate result.   
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
Performs a SIMD subtract of the packed unsigned integers of the source operand
(second operand) from the packed unsigned integers of the destination operand
(first operand), and stores the packed unsigned integer results in the destination
operand. See Figure 9-4 in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 1, for an illustration of a SIMD operation. Overflow is handled
with unsigned saturation, as described in the following paragraphs.

These instructions can operate on either 64-bit or 128-bit operands.

The (V)PSUBUSB instruction subtracts packed unsigned byte integers. When an
individual byte result is less than zero, the saturated value of 00H is written
to the destination operand.

The (V)PSUBUSW instruction subtracts packed unsigned word integers. When an
individual word result is less than zero, the saturated value of 0000H is written
to the destination operand.

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
 PSUBUSB:                                           | __m64 _mm_subs_pu8(__m64 m1, __m64 m2)   
 (V)PSUBUSB:                                        | __m128i _mm_subs_epu8(__m128i m1, __m128i
                                                    | m2)                                      
 VPSUBUSB:                                          | __m256i _mm256_subs_epu8(__m256i m1,     
                                                    | __m256i m2)                              
 PSUBUSW: (V)PSUBUSW: __m128i _mm_subs_epu16(__m128i| __m64 _mm_subs_pu16(__m64 m1, __m64      
 m1, __m128i m2)                                    | m2)                                      
 VPSUBUSW:                                          | __m256i _mm256_subs_epu16(__m256i m1,    
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
