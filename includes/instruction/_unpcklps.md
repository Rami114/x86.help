## UNPCKLPS - Unpack and Interleave Low Packed Single-Precision Floating-Point Values

> Operation
``` slim

UNPCKLPS (128-bit Legacy SSE version)
DEST[31:0] <- SRC1[31:0]
DEST[63:32] <- SRC2[31:0]
DEST[95:64] <- SRC1[63:32]
DEST[127:96] <- SRC2[63:32]
DEST[VLMAX-1:128] (Unmodified)
VUNPCKLPS (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0]
DEST[63:32] <- SRC2[31:0]
DEST[95:64] <- SRC1[63:32]
DEST[127:96] <- SRC2[63:32]
DEST[VLMAX-1:128] <- 0
UNPCKLPS (VEX.256 encoded version)
DEST[31:0] <- SRC1[31:0]
DEST[63:32] <- SRC2[31:0]
DEST[95:64] <- SRC1[63:32]
DEST[127:96] <- SRC2[63:32]
DEST[159:128] <- SRC1[159:128]
DEST[191:160] <- SRC2[159:128]
DEST[223:192] <- SRC1[191:160]
DEST[255:224] <- SRC2[191:160]

```

 Opcode/Instruction                                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F 14 /r UNPCKLPS xmm1, xmm2/m128                     | RM   | V/V                   | SSE               | Unpacks and Interleaves single-precision
                                                       |      |                       |                   | floating-point values from low quadwords
                                                       |      |                       |                   | of xmm1 and xmm2/mem into xmm1.         
 VEX.NDS.128.0F.WIG 14 /r VUNPCKLPS xmm1,xmm2,         | RVM  | V/V                   | AVX               | Unpacks and Interleaves single-precision
 xmm3/m128                                             |      |                       |                   | floating-point values from low quadwords
                                                       |      |                       |                   | of xmm2 and xmm3/m128.                  
 VEX.NDS.256.0F.WIG 14 /r VUNPCKLPS ymm1,ymm2,ymm3/m256| RVM  | V/V                   | AVX               | Unpacks and Interleaves single-precision
                                                       |      |                       |                   | floating-point values from low quadwords
                                                       |      |                       |                   | of ymm2 and ymm3/m256.                  

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs an interleaved unpack of the low-order single-precision floating-point
values from the source operand (second operand) and the destination operand
(first operand). See Figure 4-26. The source operand can be an XMM register
or a 128-bit memory location; the destination operand is an XMM register.

   | |  
---- | -----
 DEST| X3                                    | X2| X1| X0             
 SRC | Y3                                    | Y2| Y1| Y0             
 DEST| Y1 UNPCKLPS Instruction Low Unpack and| X1| Y0| X0 Figure 4-26.
     | Interleave Operation                  |   |   |                
When unpacking from a memory operand, an implementation may fetch only the appropriate
64 bits; however, alignment to 16-byte boundary and normal segment checking
will still be enforced.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
second source can be an XMM register or an 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: The first source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
the corresponding YMM register destination are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 UNPCKLPS:| __m128 _mm_unpacklo_ps(__m128 a, __m128
          | b)                                     
 UNPCKLPS:| __m256 _mm256_unpacklo_ps (__m256 a,   
          | __m256 b);                             

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4.
