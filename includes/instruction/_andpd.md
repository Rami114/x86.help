## ANDPD - Bitwise Logical AND of Packed Double-Precision Floating-Point Values

> Operation

``` slim
ANDPD (128-bit Legacy SSE version)
DEST[63:0] <- DEST[63:0] BITWISE AND SRC[63:0]
DEST[127:64] <- DEST[127:64] BITWISE AND SRC[127:64]
DEST[VLMAX-1:128] (Unmodified)
VANDPD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0] BITWISE AND SRC2[63:0]
DEST[127:64] <- SRC1[127:64] BITWISE AND SRC2[127:64]
DEST[VLMAX-1:128] <- 0
VANDPD (VEX.256 encoded version)
DEST[63:0] <- SRC1[63:0] BITWISE AND SRC2[63:0]
DEST[127:64] <- SRC1[127:64] BITWISE AND SRC2[127:64]
DEST[191:128] <- SRC1[191:128] BITWISE AND SRC2[191:128]
DEST[255:192] <- SRC1[255:192] BITWISE AND SRC2[255:192]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 ANDPD: | __m128d _mm_and_pd(__m128d a, __m128d    
        | b)                                       
 VANDPD:| __m256d _mm256_and_pd (__m256d a, __m256d
        | b)                                       

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 66 0F 54 /r ANDPD xmm1, xmm2/m128       | RM   | V/V           | SSE2              | Return the bitwise logical AND of packed
                                         |      |               |                   | double-precision floating-point values  
                                         |      |               |                   | in xmm1 and xmm2/m128.                  
 VEX.NDS.128.66.0F.WIG 54 /r VANDPD xmm1,| RVM  | V/V           | AVX               | Return the bitwise logical AND of packed
 xmm2, xmm3/m128                         |      |               |                   | double-precision floating-point values  
                                         |      |               |                   | in xmm2 and xmm3/mem.                   
 VEX.NDS.256.66.0F.WIG 54 /r VANDPD ymm1,| RVM  | V/V           | AVX               | Return the bitwise logical AND of packed
 ymm2, ymm3/m256                         |      |               |                   | double-precision floating-point values  
                                         |      |               |                   | in ymm2 and ymm3/mem.                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs a bitwise logical AND of the two packed double-precision floating-point
values from the source operand (second operand) and the destination operand
(first operand), and stores the result in the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
second source can be an XMM register or an 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: the first source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
### the corresponding YMM register destination are zeroed. VEX.256 encoded version
The first source operand is a YMM register. The second source operand can be
a YMM register or a 256-bit memory location. The destination operand is a YMM
register.



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4.
