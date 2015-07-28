## UNPCKLPD - Unpack and Interleave Low Packed Double-Precision Floating-Point Values

> Operation

``` slim
UNPCKLPD (128-bit Legacy SSE version)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
DEST[VLMAX-1:128] (Unmodified)
VUNPCKLPD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
DEST[VLMAX-1:128] <- 0
VUNPCKLPD (VEX.256 encoded version)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
DEST[191:128] <- SRC1[191:128]
DEST[255:192] <- SRC2[191:128]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 UNPCKHPD:| __m128d _mm_unpacklo_pd(__m128d a, __m128d
          | b)                                        
 UNPCKLPD:| __m256d _mm256_unpacklo_pd(__m256d a,     
          | __m256d b)                                

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 66 0F 14 /r UNPCKLPD xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Unpacks and Interleaves double-precision
                                      |      |                       |                   | floating-point values from low quadwords
                                      |      |                       |                   | of xmm1 and xmm2/m128.                  
 VEX.NDS.128.66.0F.WIG 14 /r VUNPCKLPD| RVM  | V/V                   | AVX               | Unpacks and Interleaves double precision
 xmm1,xmm2, xmm3/m128                 |      |                       |                   | floating-point values low high quadwords
                                      |      |                       |                   | of xmm2 and xmm3/m128.                  
 VEX.NDS.256.66.0F.WIG 14 /r VUNPCKLPD| RVM  | V/V                   | AVX               | Unpacks and Interleaves double precision
 ymm1,ymm2, ymm3/m256                 |      |                       |                   | floating-point values low high quadwords
                                      |      |                       |                   | of ymm2 and ymm3/m256.                  

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs an interleaved unpack of the low double-precision floating-point values
from the source operand (second operand) and the destination operand (first
operand). See Figure 4-25. The source operand can be an XMM register or a 128-bit
memory location; the destination operand is an XMM register.

   | |  
---- | -----
 DEST| X1                                    | X0             
 SRC | Y1                                    | Y0             
 DEST| Y0 UNPCKLPD Instruction Low Unpack and| X0 Figure 4-25.
     | Interleave Operation                  |                
When unpacking from a memory operand, an implementation may fetch only the appropriate
64 bits; however, alignment to 16-byte boundary and normal segment checking
will still be enforced.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: T second
source can be an XMM register or an 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: the first source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
the corresponding YMM register destination are zeroed.



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4.
