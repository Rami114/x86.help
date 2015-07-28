## PXOR - Logical Exclusive OR

> Operation

``` slim
PXOR (128-bit Legacy SSE version)
DEST <- DEST XOR SRC
DEST[VLMAX-1:128] (Unmodified)
VPXOR (VEX.128 encoded version)
DEST <- SRC1 XOR SRC2
DEST[VLMAX-1:128] <- 0
VPXOR (VEX.256 encoded version)
DEST <- SRC1 XOR SRC2

```

 Opcode\*/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                       
 ---  | --- | --- | --- | ---
 0F EF /r1 PXOR mm, mm/m64              | RM   | V/V                   | MMX               | Bitwise XOR of mm/m64 and mm.     
 66 0F EF /r PXOR xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Bitwise XOR of xmm2/m128 and xmm1.
 VEX.NDS.128.66.0F.WIG EF /r VPXOR xmm1,| RVM  | V/V                   | AVX               | Bitwise XOR of xmm3/m128 and xmm2.
 xmm2, xmm3/m128                        |      |                       |                   |                                   
 VEX.NDS.256.66.0F.WIG EF /r VPXOR ymm1,| RVM  | V/V                   | AVX2              | Bitwise XOR of ymm3/m256 and ymm2.
 ymm2, ymm3/m256                        |      |                       |                   |                                   
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
Performs a bitwise logical exclusive-OR (XOR) operation on the source operand
(second operand) and the destination operand (first operand) and stores the
result in the destination operand. Each bit of the result is 1 if the corresponding
bits of the two operands are different; each bit is 0 if the corresponding bits
of the operands are the same.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE instructions: The source operand can be an MMX technology register
or a 64-bit memory location. The destination operand is an MMX technology register.
128-bit Legacy SSE version: The second source operand is an XMM register or
a 128-bit memory location. The first source operand and destination operands
are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: The second source operand is an XMM
register or a 128-bit memory location. The first source operand and destination
operands are XMM registers. Bits (VLMAX-1:128) of the destination YMM register
are zeroed. VEX.256 encoded version: The second source operand is an YMM register
or a 256-bit memory location. The first source operand and destination operands
are YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PXOR:   | __m64 _mm_xor_si64 (__m64 m1, __m64       
         | m2)                                       
 (V)PXOR:| __m128i _mm_xor_si128 ( __m128i a, __m128i
         | b)                                        
 VPXOR:  | __m256i _mm256_xor_si256 ( __m256i a,     
         | __m256i b)                                

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
