## PADDQ - Add Packed Quadword Integers

> Operation

``` slim
PADDQ (with 64-Bit operands)
  DEST[63:0] <- DEST[63:0] + SRC[63:0];
PADDQ (with 128-Bit operands)
  DEST[63:0] <- DEST[63:0] + SRC[63:0];
  DEST[127:64] <- DEST[127:64] + SRC[127:64];
VPADDQ (VEX.128 encoded instruction)
  DEST[63:0]<- SRC1[63:0]
  DEST[127:64] <- SRC1[127:64] + SRC2[127:64];
  DEST[VLMAX-1:128] <- 0;
VPADDQ (VEX.256 encoded instruction)
  DEST[63:0]<- SRC1[63:0]
  DEST[127:64] <- SRC1[127:64] + SRC2[127:64];
  DEST[191:128]<- SRC1[191:128]
  DEST[255:192] <- SRC1[255:192] + SRC2[255:192];

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 0F D4 /r1 PADDQ mm1, mm2/m64            | RM   | V/V                   | SSE2              | Add quadword integer mm2/m64 to mm1.   
 66 0F D4 /r PADDQ xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Add packed quadword integers xmm2/m128 
                                         |      |                       |                   | to xmm1.                               
 VEX.NDS.128.66.0F.WIG D4 /r VPADDQ xmm1,| RVM  | V/V                   | AVX               | Add packed quadword integers xmm3/m128 
 xmm2, xmm3/m128                         |      |                       |                   | and xmm2.                              
 VEX.NDS.256.66.0F.WIG D4 /r VPADDQ ymm1,| RVM  | V/V                   | AVX2              | Add packed quadword integers from ymm2,
 ymm2, ymm3/m256                         |      |                       |                   | ymm3/m256 and store in ymm1.           
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
Adds the first operand (destination operand) to the second operand (source operand)
and stores the result in the destination operand. The source operand can be
a quadword integer stored in an MMX technology register or a 64bit memory location,
or it can be two packed quadword integers stored in an XMM register or an 128-bit
memory location. The destination operand can be a quadword integer stored in
an MMX technology register or two packed quadword integers stored in an XMM
register. When packed quadword operands are used, a SIMD add is performed. When
a quadword result is too large to be represented in 64 bits (overflow), the
result is wrapped around and the low 64 bits are written to the destination
element (that is, the carry is ignored).

<aside class="notification">
Note that the (V)PADDQ instruction can operate on either unsigned or signed
(two's complement notation) integers; however, it does not set bits in the EFLAGS
register to indicate overflow and/or a carry. To prevent undetected overflow
conditions, software must control the ranges of the values operated on.
</aside>

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
first source operand is an XMM register. The second operand can be an XMM register
or a 128-bit memory location. The destination is not distinct from the first
source XMM register and the upper bits (VLMAX-1:128) of the corresponding YMM
register destination are unmodified. VEX.128 encoded version: The first source
operand is an XMM register. The second source operand is an XMM register or
128-bit memory location. The destination operand is an XMM register. The upper
bits (VLMAX-1:128) of the corresponding YMM register destination are zeroed.
VEX.256 encoded version: The first source operand is a YMM register. The second
source operand is a YMM register or a 256-bit memory location. The destination
operand is a YMM register.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PADDQ:   | __m64 _mm_add_si64 (__m64 a, __m64 b)     
 (V)PADDQ:| __m128i _mm_add_epi64 ( __m128i a, __m128i
          | b)                                        
 VPADDQ:  | __m256i _mm256_add_epi64 ( __m256i a,     
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
