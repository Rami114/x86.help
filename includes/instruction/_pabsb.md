## PABSB/PABSW/PABSD  -  Packed Absolute Value

> Operation

``` slim
PABSB (with 64 bit operands)
  Unsigned DEST[7:0] <- ABS(SRC[7:0])
  Repeat operation for 2nd through 7th bytes
  Unsigned DEST[63:56] <- ABS(SRC[63:56])
PABSB (with 128 bit operands)
  Unsigned DEST[7:0] <- ABS(SRC[7:.0])
  Repeat operation for 2nd through 15th bytes
  Unsigned DEST[127:120] <- ABS(SRC[127:120])
PABSW (with 64 bit operands)
  Unsigned DEST[15:0] <- ABS(SRC[15:0])
  Repeat operation for 2nd through 3rd 16-bit words
  Unsigned DEST[63:48] <- ABS(SRC[63:48])
PABSW (with 128 bit operands)
  Unsigned DEST[15:0] <- ABS(SRC[15:0])
  Repeat operation for 2nd through 7th 16-bit words
  Unsigned DEST[127:112] <- ABS(SRC[127:112])
PABSD (with 64 bit operands)
  Unsigned DEST[31:0] <- ABS(SRC[31:0])
  Unsigned DEST[63:32] <- ABS(SRC[63:32])
PABSD (with 128 bit operands)
  Unsigned DEST[31:0] <- ABS(SRC[31:0])
  Repeat operation for 2nd through 3rd 32-bit double words
  Unsigned DEST[127:96] <- ABS(SRC[127:96])
PABSB (128-bit Legacy SSE version)
  DEST[127:0] <- BYTE_ABS(SRC)
  DEST[VLMAX-1:128] (Unmodified)
VPABSB (VEX.128 encoded version)
  DEST[127:0] <- BYTE_ABS(SRC)
  DEST[VLMAX-1:128] <- 0
VPABSB (VEX.256 encoded version)
  Unsigned DEST[7:0]<- ABS(SRC[7:.0])
  Repeat operation for 2nd through 31st bytes
  Unsigned DEST[255:248] <- ABS(SRC[255:248])
PABSW (128-bit Legacy SSE version)
  DEST[127:0] <- WORD_ABS(SRC)
  DEST[VLMAX-1:128] (Unmodified)
VPABSW (VEX.128 encoded version)
  DEST[127:0] <- WORD_ABS(SRC)
  DEST[VLMAX-1:128] <- 0
VPABSW (VEX.256 encoded version)
  Unsigned DEST[15:0]<- ABS(SRC[15:0])
  Repeat operation for 2nd through 15th 16-bit words
  Unsigned DEST[255:240] <- ABS(SRC[255:240])
PABSD (128-bit Legacy SSE version)
  DEST[127:0] <- DWORD_ABS(SRC)
  DEST[VLMAX-1:128] (Unmodified)
VPABSD (VEX.128 encoded version)
  DEST[127:0] <- DWORD_ABS(SRC)
  DEST[VLMAX-1:128] <- 0
VPABSD (VEX.256 encoded version)
  Unsigned DEST[31:0] <- ABS(SRC[31:0])
  Repeat operation for 2nd through 7th 32-bit double words
  Unsigned DEST[255:224] <- ABS(SRC[255:224])

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PABSB:   | __m64 _mm_abs_pi8 (__m64 a)         
 (V)PABSB:| __m128i _mm_abs_epi8 (__m128i a)    
 VPABSB:  | __m256i _mm256_abs_epi8 (__m256i a) 
 PABSW:   | __m64 _mm_abs_pi16 (__m64 a)        
 (V)PABSW:| __m128i _mm_abs_epi16 (__m128i a)   
 VPABSW:  | __m256i _mm256_abs_epi16 (__m256i a)
 PABSD:   | __m64 _mm_abs_pi32 (__m64 a)        
 (V)PABSD:| __m128i _mm_abs_epi32 (__m128i a)   
 VPABSD:  | __m256i _mm256_abs_epi32 (__m256i a)

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F 38 1C /r1 PABSB mm1, mm2/m64       | RM   | V/V                   | SSSE3             | Compute the absolute value of bytes     
                                       |      |                       |                   | in mm2/m64 and store UNSIGNED result    
                                       |      |                       |                   | in mm1.                                 
 66 0F 38 1C /r PABSB xmm1, xmm2/m128  | RM   | V/V                   | SSSE3             | Compute the absolute value of bytes     
                                       |      |                       |                   | in xmm2/m128 and store UNSIGNED result  
                                       |      |                       |                   | in xmm1.                                
 0F 38 1D /r1 PABSW mm1, mm2/m64       | RM   | V/V                   | SSSE3             | Compute the absolute value of 16-bit    
                                       |      |                       |                   | integers in mm2/m64 and store UNSIGNED  
                                       |      |                       |                   | result in mm1.                          
 66 0F 38 1D /r PABSW xmm1, xmm2/m128  | RM   | V/V                   | SSSE3             | Compute the absolute value of 16-bit    
                                       |      |                       |                   | integers in xmm2/m128 and store UNSIGNED
                                       |      |                       |                   | result in xmm1.                         
 0F 38 1E /r1 PABSD mm1, mm2/m64       | RM   | V/V                   | SSSE3             | Compute the absolute value of 32-bit    
                                       |      |                       |                   | integers in mm2/m64 and store UNSIGNED  
                                       |      |                       |                   | result in mm1.                          
 66 0F 38 1E /r PABSD xmm1, xmm2/m128  | RM   | V/V                   | SSSE3             | Compute the absolute value of 32-bit    
                                       |      |                       |                   | integers in xmm2/m128 and store UNSIGNED
                                       |      |                       |                   | result in xmm1.                         
 VEX.128.66.0F38.WIG 1C /r VPABSB xmm1,| RM   | V/V                   | AVX               | Compute the absolute value of bytes     
 xmm2/m128                             |      |                       |                   | in xmm2/m128 and store UNSIGNED result  
                                       |      |                       |                   | in xmm1.                                
 VEX.128.66.0F38.WIG 1D /r VPABSW xmm1,| RM   | V/V                   | AVX               | Compute the absolute value of 16- bit   
 xmm2/m128                             |      |                       |                   | integers in xmm2/m128 and store UNSIGNED
                                       |      |                       |                   | result in xmm1.                         
 VEX.128.66.0F38.WIG 1E /r VPABSD xmm1,| RM   | V/V                   | AVX               | Compute the absolute value of 32- bit   
 xmm2/m128                             |      |                       |                   | integers in xmm2/m128 and store UNSIGNED
                                       |      |                       |                   | result in xmm1.                         
 VEX.256.66.0F38.WIG 1C /r VPABSB ymm1,| RM   | V/V                   | AVX2              | Compute the absolute value of bytes     
 ymm2/m256                             |      |                       |                   | in ymm2/m256 and store UNSIGNED result  
                                       |      |                       |                   | in ymm1.                                
 VEX.256.66.0F38.WIG 1D /r VPABSW ymm1,| RM   | V/V                   | AVX2              | Compute the absolute value of 16-bit    
 ymm2/m256                             |      |                       |                   | integers in ymm2/m256 and store UNSIGNED
                                       |      |                       |                   | result in ymm1.                         
 VEX.256.66.0F38.WIG 1E /r VPABSD ymm1,| RM   | V/V                   | AVX2              | Compute the absolute value of 32-bit    
 ymm2/m256                             |      |                       |                   | integers in ymm2/m256 and store UNSIGNED
                                       |      |                       |                   | result in ymm1.                         
<aside class="notification">
1. See note in Section 2.4, “Instruction Exception Specification” in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 2A
and Section 22.25.3, “Exception Conditions of Legacy SIMD Instructions Operating
on MMX Registers” in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3A.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
(V)PABSB/W/D computes the absolute value of each data element of the source
operand (the second operand) and stores the UNSIGNED results in the destination
operand (the first operand). (V)PABSB operates on signed bytes, (V)PABSW operates
on 16-bit words, and (V)PABSD operates on signed 32-bit integers. The source
operand can be an MMX register or a 64-bit memory location, or it can be an
XMM register, a YMM register, a 128-bit memory location, or a 256-bit memory
location. The destination operand can be an MMX, an XMM or a YMM register. Both
operands can be MMX registers or XMM registers. When the source operand is a
128-bit memory operand, the operand must be aligned on a 16byte boundary or
a general-protection exception (**``#GP)``** will be generated.

In 64-bit mode, use the REX prefix to access additional registers. 128-bit Legacy
SSE version: The source operand can be an XMM register or a 128-bit memory location.
The destination is not distinct from the first source XMM register and the upper
bits (VLMAX-1:128) of the corresponding YMM register destination are unmodified.
VEX.128 encoded version: The source operand is an XMM register or 128-bit memory
location. The destination operand is an XMM register. The upper bits (VLMAX-1:128)
### of the corresponding YMM register destination are zeroed. VEX.256 encoded version
The first source operand is a YMM register or a 256-bit memory location. The
destination operand is a YMM register.

<aside class="notification">
VEX.vvvv is reserved and must be 1111b, VEX.L must be 0; otherwise instructions
will **``#UD.``**
</aside>



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1. If VEX.vvvv != 1111B.
