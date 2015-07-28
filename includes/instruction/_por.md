## POR - Bitwise Logical OR

> Operation

``` slim
POR (128-bit Legacy SSE version)
DEST <- DEST OR SRC
DEST[VLMAX-1:128] (Unmodified)
VPOR (VEX.128 encoded version)
DEST <- SRC1 OR SRC2
DEST[VLMAX-1:128] <- 0
VPOR (VEX.256 encoded version)
DEST <- SRC1 OR SRC2

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 POR:   | __m64 _mm_or_si64(__m64 m1, __m64 m2)   
 (V)POR:| __m128i _mm_or_si128(__m128i m1, __m128i
        | m2)                                     
 VPOR:  | __m256i _mm256_or_si256 ( __m256i a,    
        | __m256i b)                              

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                      
 ---  | --- | --- | --- | ---
 0F EB /r1 POR mm, mm/m64              | RM   | V/V                   | MMX               | Bitwise OR of mm/m64 and mm.     
 66 0F EB /r POR xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Bitwise OR of xmm2/m128 and xmm1.
 VEX.NDS.128.66.0F.WIG EB /r VPOR xmm1,| RVM  | V/V                   | AVX               | Bitwise OR of xmm2/m128 and xmm3.
 xmm2, xmm3/m128                       |      |                       |                   |                                  
 VEX.NDS.256.66.0F.WIG EB /r VPOR ymm1,| RVM  | V/V                   | AVX2              | Bitwise OR of ymm2/m256 and ymm3.
 ymm2, ymm3/m256                       |      |                       |                   |                                  
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
Performs a bitwise logical OR operation on the source operand (second operand)
and the destination operand (first operand) and stores the result in the destination
operand. Each bit of the result is set to 1 if either or both of the corresponding
bits of the first and second operands are 1; otherwise, it is set to 0.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). Legacy SSE version: The source
operand can be an MMX technology register or a 64-bit memory location. The destination
operand is an MMX technology register.

128-bit Legacy SSE version: The second source operand is an XMM register or
a 128-bit memory location. The first source and destination operands can be
XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: The second source operand is an XMM
register or a 128-bit memory location. The first source and destination operands
can be XMM registers. Bits (VLMAX-1:128) of the destination YMM register are
zeroed. VEX.256 encoded version: The second source operand is an YMM register
or a 256-bit memory location. The first source and destination operands can
be YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
