## PANDN - Logical AND NOT

> Operation

``` slim
PANDN(128-bit Legacy SSE version)
DEST <- NOT(DEST) AND SRC
DEST[VLMAX-1:128] (Unmodified)
VPANDN (VEX.128 encoded version)
DEST <- NOT(SRC1) AND SRC2
DEST[VLMAX-1:128] <- 0
VPANDN (VEX.256 encoded instruction)
DEST[255:0] <- ((NOT SRC1[255:0]) AND SRC2[255:0])

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PANDN:   | __m64 _mm_andnot_si64 (__m64 m1, __m64
          | m2)                                   
 (V)PANDN:| __m128i _mm_andnot_si128 ( __m128i a, 
          | __m128i b)                            
 VPANDN:  | __m256i _mm256_andnot_si256 ( __m256i 
          | a, __m256i b)                         

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 0F DF /r1 PANDN mm, mm/m64              | RM   | V/V                   | MMX               | Bitwise AND NOT of mm/m64 and mm.     
 66 0F DF /r PANDN xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Bitwise AND NOT of xmm2/m128 and xmm1.
 VEX.NDS.128.66.0F.WIG DF /r VPANDN xmm1,| RVM  | V/V                   | AVX               | Bitwise AND NOT of xmm3/m128 and xmm2.
 xmm2, xmm3/m128                         |      |                       |                   |                                       
 VEX.NDS.256.66.0F.WIG DF /r VPANDN ymm1,| RVM  | V/V                   | AVX2              | Bitwise AND NOT of ymm2, and ymm3/m256
 ymm2, ymm3/m256                         |      |                       |                   | and store result in ymm1.             
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
Performs a bitwise logical NOT operation on the first source operand, then performs
bitwise AND with second source operand and stores the result in the destination
operand. Each bit of the result is set to 1 if the corresponding bit in the
first operand is 0 and the corresponding bit in the second operand is 1, otherwise
it is set to 0.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE instructions: The source operand can be an MMX technology register
or a 64-bit memory location. The destination operand can be an MMX technology
register. 128-bit Legacy SSE version: The first source operand is an XMM register.
The second operand can be an XMM register or a 128-bit memory location. The
destination is not distinct from the first source XMM register and the upper
bits (VLMAX-1:128) of the corresponding YMM register destination are unmodified.
VEX.128 encoded version: The first source operand is an XMM register. The second
source operand is an XMM register or 128-bit memory location. The destination
operand is an XMM register. The upper bits (VLMAX-1:128) of the corresponding
YMM register destination are zeroed. VEX.256 encoded version: The first source
operand is a YMM register. The second source operand is a YMM register or a
256-bit memory location. The destination operand is a YMM register.

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
