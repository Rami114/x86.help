## PMADDWD - Multiply and Add Packed Integers

> Operation

``` slim
PMADDWD (with 64-bit operands)
  DEST[31:0] <- (DEST[15:0] \* SRC[15:0]) + (DEST[31:16] \* SRC[31:16]);
  DEST[63:32] <- (DEST[47:32] \* SRC[47:32]) + (DEST[63:48] \* SRC[63:48]);
PMADDWD (with 128-bit operands)
  DEST[31:0] <- (DEST[15:0] \* SRC[15:0]) + (DEST[31:16] \* SRC[31:16]);
  DEST[63:32] <- (DEST[47:32] \* SRC[47:32]) + (DEST[63:48] \* SRC[63:48]);
  DEST[95:64] <- (DEST[79:64] \* SRC[79:64]) + (DEST[95:80] \* SRC[95:80]);
  DEST[127:96] <- (DEST[111:96] \* SRC[111:96]) + (DEST[127:112] \* SRC[127:112]);
VPMADDWD (VEX.128 encoded version)
DEST[31:0] <- (SRC1[15:0] \* SRC2[15:0]) + (SRC1[31:16] \* SRC2[31:16])
DEST[63:32] <- (SRC1[47:32] \* SRC2[47:32]) + (SRC1[63:48] \* SRC2[63:48])
DEST[95:64] <- (SRC1[79:64] \* SRC2[79:64]) + (SRC1[95:80] \* SRC2[95:80])
DEST[127:96] <- (SRC1[111:96] \* SRC2[111:96]) + (SRC1[127:112] \* SRC2[127:112])
DEST[VLMAX-1:128] <- 0
VPMADDWD (VEX.256 encoded version)
DEST[31:0] <- (SRC1[15:0] \* SRC2[15:0]) + (SRC1[31:16] \* SRC2[31:16])
DEST[63:32] <- (SRC1[47:32] \* SRC2[47:32]) + (SRC1[63:48] \* SRC2[63:48])
DEST[95:64] <- (SRC1[79:64] \* SRC2[79:64]) + (SRC1[95:80] \* SRC2[95:80])
DEST[127:96] <- (SRC1[111:96] \* SRC2[111:96]) + (SRC1[127:112] \* SRC2[127:112])
DEST[159:128] <- (SRC1[143:128] \* SRC2[143:128]) + (SRC1[159:144] \* SRC2[159:144])
DEST[191:160] <- (SRC1[175:160] \* SRC2[175:160]) + (SRC1[191:176] \* SRC2[191:176])
DEST[223:192] <- (SRC1[207:192] \* SRC2[207:192]) + (SRC1[223:208] \* SRC2[223:208])
DEST[255:224] <- (SRC1[239:224] \* SRC2[239:224]) + (SRC1[255:240] \* SRC2[255:240])

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PMADDWD:   | __m64 _mm_madd_pi16(__m64 m1, __m64
            | m2)                                
 (V)PMADDWD:| __m128i _mm_madd_epi16 ( __m128i a,
            | __m128i b)                         
 VPMADDWD:  | __m256i _mm256_madd_epi16 ( __m256i
            | a, __m256i b)                      

```

 Opcode/Instruction                  | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F F5 /r1 PMADDWD mm, mm/m64        | RM   | V/V                   | MMX               | Multiply the packed words in mm by the     
                                     |      |                       |                   | packed words in mm/m64, add adjacent       
                                     |      |                       |                   | doubleword results, and store in mm.       
 66 0F F5 /r PMADDWD xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Multiply the packed word integers in       
                                     |      |                       |                   | xmm1 by the packed word integers in        
                                     |      |                       |                   | xmm2/m128, add adjacent doubleword results,
                                     |      |                       |                   | and store in xmm1.                         
 VEX.NDS.128.66.0F.WIG F5 /r VPMADDWD| RVM  | V/V                   | AVX               | Multiply the packed word integers in       
 xmm1, xmm2, xmm3/m128               |      |                       |                   | xmm2 by the packed word integers in        
                                     |      |                       |                   | xmm3/m128, add adjacent doubleword results,
                                     |      |                       |                   | and store in xmm1.                         
 VEX.NDS.256.66.0F.WIG F5 /r VPMADDWD| RVM  | V/V                   | AVX2              | Multiply the packed word integers in       
 ymm1, ymm2, ymm3/m256               |      |                       |                   | ymm2 by the packed word integers in        
                                     |      |                       |                   | ymm3/m256, add adjacent doubleword results,
                                     |      |                       |                   | and store in ymm1.                         
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
Multiplies the individual signed words of the destination operand (first operand)
by the corresponding signed words of the source operand (second operand), producing
temporary signed, doubleword results. The adjacent doubleword results are then
summed and stored in the destination operand. For example, the corresponding
low-order words (15-0) and (31-16) in the source and destination operands are
multiplied by one another and the doubleword results are added together and
stored in the low doubleword of the destination register (31-0). The same operation
is performed on the other pairs of adjacent words. (Figure 4-7 shows this operation
when using 64-bit operands).

The (V)PMADDWD instruction wraps around only in one situation: when the 2 pairs
of words being operated on in a group are all 8000H. In this case, the result
wraps around to 80000000H.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). Legacy SSE version: The first source
and destination operands are MMX registers. The second source operand is an
MMX register or a 64-bit memory location.

128-bit Legacy SSE version: The first source and destination operands are XMM
registers. The second source operand is an XMM register or a 128-bit memory
location. Bits (VLMAX-1:128) of the corresponding YMM destination register remain
unchanged. VEX.128 encoded version: The first source and destination operands
are XMM registers. The second source operand is an XMM register or a 128-bit
memory location. Bits (VLMAX-1:128) of the destination YMM register are zeroed.

VEX.256 encoded version: The second source operand can be an YMM register or
a 256-bit memory location. The first source and destination operands are YMM
registers.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>

   | |  
---- | -----
 SRC | X3        | X2| X1        | X0        
 DEST| Y3 X2 \* Y2| Y2| Y1 X1 \* Y1| Y0 X0 \* Y0
DEST

   | |  
---- | -----
 (X3\*Y3) + (X2\*Y2)                   | (X1\*Y1) + (X0\*Y0)
 PMADDWD Execution Model Using 64-bit| Figure 4-7.      
 Operands                            |                  
 ---  | ---


### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
