## PMULUDQ - Multiply Packed Unsigned Doubleword Integers

> Operation
``` slim

PMULUDQ (with 64-Bit operands)
  DEST[63:0] <- DEST[31:0] \* SRC[31:0];
PMULUDQ (with 128-Bit operands)
  DEST[63:0] <- DEST[31:0] \* SRC[31:0];
  DEST[127:64] <- DEST[95:64] \* SRC[95:64];
VPMULUDQ (VEX.128 encoded version)
DEST[63:0] <- SRC1[31:0] \* SRC2[31:0]
DEST[127:64] <- SRC1[95:64] \* SRC2[95:64]
DEST[VLMAX-1:128] <- 0
VPMULUDQ (VEX.256 encoded version)
DEST[63:0] <- SRC1[31:0] \* SRC2[31:0]
DEST[127:64] <- SRC1[95:64] \* SRC2[95:64
DEST[191:128] <- SRC1[159:128] \* SRC2[159:128]
DEST[255:192] <- SRC1[223:192] \* SRC2[223:192]

```

 Opcode/Instruction                  | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 0F F4 /r1 PMULUDQ mm1, mm2/m64      | RM   | V/V                   | SSE2              | Multiply unsigned doubleword integer     
                                     |      |                       |                   | in mm1 by unsigned doubleword integer    
                                     |      |                       |                   | in mm2/m64, and store the quadword result
                                     |      |                       |                   | in mm1.                                  
 66 0F F4 /r PMULUDQ xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Multiply packed unsigned doubleword      
                                     |      |                       |                   | integers in xmm1 by packed unsigned      
                                     |      |                       |                   | doubleword integers in xmm2/m128, and    
                                     |      |                       |                   | store the quadword results in xmm1.      
 VEX.NDS.128.66.0F.WIG F4 /r VPMULUDQ| RVM  | V/V                   | AVX               | Multiply packed unsigned doubleword      
 xmm1, xmm2, xmm3/m128               |      |                       |                   | integers in xmm2 by packed unsigned      
                                     |      |                       |                   | doubleword integers in xmm3/m128, and    
                                     |      |                       |                   | store the quadword results in xmm1.      
 VEX.NDS.256.66.0F.WIG F4 /r VPMULUDQ| RVM  | V/V                   | AVX2              | Multiply packed unsigned doubleword      
 ymm1, ymm2, ymm3/m256               |      |                       |                   | integers in ymm2 by packed unsigned      
                                     |      |                       |                   | doubleword integers in ymm3/m256, and    
                                     |      |                       |                   | store the quadword results in ymm1.      
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
Multiplies the first operand (destination operand) by the second operand (source
operand) and stores the result in the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE version: The source operand can be an unsigned doubleword integer
stored in the low doubleword of an MMX technology register or a 64-bit memory
location. The destination operand can be an unsigned doubleword integer stored
in the low doubleword an MMX technology register. The result is an unsigned
quadword integer stored in the destination an MMX technology register. When
a quadword result is too large to be represented in 64 bits (overflow), the
result is wrapped around and the low 64 bits are written to the destination
element (that is, the carry is ignored).

For 64-bit memory operands, 64 bits are fetched from memory, but only the low
doubleword is used in the computation.

128-bit Legacy SSE version: The second source operand is two packed unsigned
doubleword integers stored in the first (low) and third doublewords of an XMM
register or a 128-bit memory location. For 128-bit memory operands, 128 bits
are fetched from memory, but only the first and third doublewords are used in
the computation.The first source operand is two packed unsigned doubleword integers
stored in the first and third doublewords of an XMM register. The destination
contains two packed unsigned quadword integers stored in an XMM register. Bits
(VLMAX1:128) of the corresponding YMM destination register remain unchanged.
VEX.128 encoded version: The second source operand is two packed unsigned doubleword
integers stored in the first (low) and third doublewords of an XMM register
or a 128-bit memory location. For 128-bit memory operands, 128 bits are fetched
from memory, but only the first and third doublewords are used in the computation.The
first

source operand is two packed unsigned doubleword integers stored in the first
and third doublewords of an XMM register. The destination contains two packed
unsigned quadword integers stored in an XMM register. Bits (VLMAX1:128) of the
destination YMM register are zeroed. VEX.256 encoded version: The second source
operand is four packed unsigned doubleword integers stored in the first (low),
third, fifth and seventh doublewords of a YMM register or a 256-bit memory location.
For 256-bit memory operands, 256 bits are fetched from memory, but only the
first, third, fifth and seventh doublewords are used in the computation.The
first source operand is four packed unsigned doubleword integers stored in the
first, third, fifth and seventh doublewords of an YMM register. The destination
contains four packed unaligned quadword integers stored in an YMM register.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PMULUDQ:   | __m64 _mm_mul_su32 (__m64 a, __m64 b)     
 (V)PMULUDQ:| __m128i _mm_mul_epu32 ( __m128i a, __m128i
            | b)                                        
 VPMULUDQ:  | __m256i _mm256_mul_epu32( __m256i a,      
            | __m256i b);                               

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
