## DIVPS - Divide Packed Single-Precision Floating-Point Values

> Operation

``` slim
DIVPS (128-bit Legacy SSE version)
DEST[31:0] <- SRC1[31:0] / SRC2[31:0]
DEST[63:32] <- SRC1[63:32] / SRC2[63:32]
DEST[95:64] <- SRC1[95:64] / SRC2[95:64]
DEST[127:96] <- SRC1[127:96] / SRC2[127:96]
DEST[VLMAX-1:128] (Unmodified)
VDIVPS (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0] / SRC2[31:0]
DEST[63:32] <- SRC1[63:32] / SRC2[63:32]
DEST[95:64] <- SRC1[95:64] / SRC2[95:64]
DEST[127:96] <- SRC1[127:96] / SRC2[127:96]
DEST[VLMAX-1:128] <- 0
VDIVPS (VEX.256 encoded version)
DEST[31:0] <- SRC1[31:0] / SRC2[31:0]
DEST[63:32] <- SRC1[63:32] / SRC2[63:32]
DEST[95:64] <- SRC1[95:64] / SRC2[95:64]
DEST[127:96] <- SRC1[127:96] / SRC2[127:96]
DEST[159:128] <- SRC1[159:128] / SRC2[159:128]
DEST[191:160]<- SRC1[191:160] / SRC2[191:160]
DEST[223:192] <- SRC1[223:192] / SRC2[223:192]
DEST[255:224] <- SRC1[255:224] / SRC2[255:224].

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 DIVPS: | __m128 _mm_div_ps(__m128 a, __m128 b) 
 VDIVPS:| __m256 _mm256_div_ps (__m256 a, __m256
        | b);                                   

```

 Opcode/Instruction                   | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                  
 ---  | --- | --- | --- | ---
 0F 5E /r DIVPS xmm1, xmm2/m128       | RM   | V/V           | SSE               | Divide packed single-precision floating-point
                                      |      |               |                   | values in xmm1 by packed single-precision    
                                      |      |               |                   | floating-point values xmm2/m128.             
 VEX.NDS.128.0F.WIG 5E /r VDIVPS xmm1,| RVM  | V/V           | AVX               | Divide packed single-precision floating-point
 xmm2, xmm3/m128                      |      |               |                   | values in xmm2 by packed double-precision    
                                      |      |               |                   | floating-point values in xmm3/mem.           
 VEX.NDS.256.0F.WIG 5E /r VDIVPS ymm1,| RVM  | V/V           | AVX               | Divide packed single-precision floating-point
 ymm2, ymm3/m256                      |      |               |                   | values in ymm2 by packed double-precision    
                                      |      |               |                   | floating-point values in ymm3/mem.           

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs an SIMD divide of the four or eight packed single-precision floating-point
values in the first source operand by the four or eight packed single-precision
floating-point values in the second source operand. See Chapter 10 in the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1, for an overview
of a SIMD single-precision floating-point operation.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The second source can be
an XMM register or an 128-bit memory location. The destination is not distinct
from the first source XMM register and the upper bits (VLMAX-1:128) of the corresponding
YMM register destination are unmodified. VEX.128 encoded version: the first
source operand is an XMM register or 128-bit memory location. The destination
operand is an XMM register. The upper bits (VLMAX-1:128) of the corresponding
YMM register destination are zeroed. VEX.256 encoded version: The first source
operand is a YMM register. The second source operand can be a YMM register or
a 256-bit memory location. The destination operand is a YMM register.



### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Divide-by-Zero, Precision, Denormal.


### Other Exceptions
See Exceptions Type 2.
