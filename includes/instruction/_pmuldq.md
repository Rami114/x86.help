## PMULDQ  -  Multiply Packed Signed Dword Integers

> Operation
``` slim

PMULDQ (128-bit Legacy SSE version)
DEST[63:0] <- DEST[31:0] \* SRC[31:0]
DEST[127:64] <- DEST[95:64] \* SRC[95:64]
DEST[VLMAX-1:128] (Unmodified)
VPMULDQ (VEX.128 encoded version)
DEST[63:0] <- SRC1[31:0] \* SRC2[31:0]
DEST[127:64] <- SRC1[95:64] \* SRC2[95:64]
DEST[VLMAX-1:128] <- 0
VPMULDQ (VEX.256 encoded version)
DEST[63:0] <- SRC1[31:0] \* SRC2[31:0]
DEST[127:64] <- SRC1[95:64] \* SRC2[95:64]
DEST[191:128] <- SRC1[159:128] \* SRC2[159:128]
DEST[255:192] <- SRC1[223:192] \* SRC2[223:192]

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 66 0F 38 28 /r PMULDQ xmm1, xmm2/m128| RM   | V/V                   | SSE4_1            | Multiply the packed signed dword integers 
                                      |      |                       |                   | in xmm1 and xmm2/m128 and store the       
                                      |      |                       |                   | quadword product in xmm1.                 
 VEX.NDS.128.66.0F38.WIG 28 /r VPMULDQ| RVM  | V/V                   | AVX               | Multiply packed signed doubleword integers
 xmm1, xmm2, xmm3/m128                |      |                       |                   | in xmm2 by packed signed doubleword       
                                      |      |                       |                   | integers in xmm3/m128, and store the      
                                      |      |                       |                   | quadword results in xmm1.                 
 VEX.NDS.256.66.0F38.WIG 28 /r VPMULDQ| RVM  | V/V                   | AVX2              | Multiply packed signed doubleword integers
 ymm1, ymm2, ymm3/m256                |      |                       |                   | in ymm2 by packed signed doubleword       
                                      |      |                       |                   | integers in ymm3/m256, and store the      
                                      |      |                       |                   | quadword results in ymm1.                 

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Multiplies the first source operand by the second source operand and stores
the result in the destination operand. For PMULDQ and VPMULDQ (VEX.128 encoded
version), the second source operand is two packed signed doubleword integers
stored in the first (low) and third doublewords of an XMM register or a 128-bit
memory location. The first source operand is two packed signed doubleword integers
stored in the first and third doublewords of an XMM register. The destination
contains two packed signed quadword integers stored in an XMM register. For
128-bit memory operands, 128 bits are fetched from memory, but only the first
and third doublewords are used in the computation. For VPMULDQ (VEX.256 encoded
version), the second source operand is four packed signed doubleword integers
stored in the first (low), third, fifth and seventh doublewords of an YMM register
or a 256-bit memory location. The first source operand is four packed signed
doubleword integers stored in the first, third, fifth and seventh doublewords
of an XMM register. The destination contains four packed signed quadword integers
stored in an YMM register. For 256-bit memory operands, 256 bits are fetched
from memory, but only the first, third, fifth and seventh doublewords are used
in the computation. When a quadword result is too large to be represented in
64 bits (overflow), the result is wrapped around and the low 64 bits are written
to the destination element (that is, the carry is ignored). 128-bit Legacy SSE
version: The first source and destination operands are XMM registers. The second
source operand is an XMM register or a 128-bit memory location. Bits (VLMAX-1:128)
of the corresponding YMM destination register remain unchanged. VEX.128 encoded
version: The first source and destination operands are XMM registers. The second
source operand is an XMM register or a 128-bit memory location. Bits (VLMAX-1:128)
of the destination YMM register are zeroed. VEX.L must be 0, otherwise the instruction
will #UD. VEX.256 encoded version: The second source operand can be an YMM register
or a 256-bit memory location. The first source and destination operands are
YMM registers.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PMULDQ:| __m128i _mm_mul_epi32( __m128i a, __m128i
           | b);                                      
 VPMULDQ:  | __m256i _mm256_mul_epi32( __m256i a,     
           | __m256i b);                              

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1. If VEX.vvvv != 1111B.
