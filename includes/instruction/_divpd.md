## DIVPD - Divide Packed Double-Precision Floating-Point Values

> Operation

``` slim
DIVPD (128-bit Legacy SSE version)
DEST[63:0] <- SRC1[63:0] / SRC2[63:0]
DEST[127:64] <- SRC1[127:64] / SRC2[127:64]
DEST[VLMAX-1:128] (Unmodified)
VDIVPD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0] / SRC2[63:0]
DEST[127:64] <- SRC1[127:64] / SRC2[127:64]
DEST[VLMAX-1:128] <- 0
VDIVPD (VEX.256 encoded version)
DEST[63:0] <- SRC1[63:0] / SRC2[63:0]
DEST[127:64] <- SRC1[127:64] / SRC2[127:64]
DEST[191:128] <- SRC1[191:128] / SRC2[191:128]
DEST[255:192] <- SRC1[255:192] / SRC2[255:192]

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                  
 ---  | --- | --- | --- | ---
 66 0F 5E /r DIVPD xmm1, xmm2/m128       | RM   | V/V           | SSE2              | Divide packed double-precision floating-point
                                         |      |               |                   | values in xmm1 by packed double-precision    
                                         |      |               |                   | floating-point values xmm2/m128.             
 VEX.NDS.128.66.0F.WIG 5E /r VDIVPD xmm1,| RVM  | V/V           | AVX               | Divide packed double-precision floating-point
 xmm2, xmm3/m128                         |      |               |                   | values in xmm2 by packed double-precision    
                                         |      |               |                   | floating-point values in xmm3/mem.           
 VEX.NDS.256.66.0F.WIG 5E /r VDIVPD ymm1,| RVM  | V/V           | AVX               | Divide packed double-precision floating-point
 ymm2, ymm3/m256                         |      |               |                   | values in ymm2 by packed double-precision    
                                         |      |               |                   | floating-point values in ymm3/mem.           

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs an SIMD divide of the two or four packed double-precision floating-point
values in the first source operand by the two or four packed double-precision
floating-point values in the second source operand. See Chapter 11 in the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1, for an overview
of a SIMD doubleprecision floating-point operation.

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



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 DIVPD: | __m128d _mm_div_pd(__m128d a, __m128d    
        | b)                                       
 VDIVPD:| __m256d _mm256_div_pd (__m256d a, __m256d
        | b);                                      

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Divide-by-Zero, Precision, Denormal.


### Other Exceptions
See Exceptions Type 2.
