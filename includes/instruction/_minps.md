## MINPS - Return Minimum Packed Single-Precision Floating-Point Values

> Operation
``` slim

MIN(SRC1, SRC2)
{
  IF ((SRC1 = 0.0) and (SRC2 = 0.0)) THEN DEST <- SRC2;
     ELSE IF (SRC1 = SNaN) THEN DEST <- SRC2; FI;
     ELSE IF (SRC2 = SNaN) THEN DEST <- SRC2; FI;
     ELSE IF (SRC1 < SRC2) THEN DEST <- SRC1;
     ELSE DEST <- SRC2;
  FI;
}
MINPS (128-bit Legacy SSE version)
DEST[31:0] <- MIN(SRC1[31:0], SRC2[31:0])
DEST[63:32] <- MIN(SRC1[63:32], SRC2[63:32])
DEST[95:64] <- MIN(SRC1[95:64], SRC2[95:64])
DEST[127:96] <- MIN(SRC1[127:96], SRC2[127:96])
DEST[VLMAX-1:128] (Unmodified)
VMINPS (VEX.128 encoded version)
DEST[31:0] <- MIN(SRC1[31:0], SRC2[31:0])
DEST[63:32] <- MIN(SRC1[63:32], SRC2[63:32])
DEST[95:64] <- MIN(SRC1[95:64], SRC2[95:64])
DEST[127:96] <- MIN(SRC1[127:96], SRC2[127:96])
DEST[VLMAX-1:128] <- 0
VMINPS (VEX.256 encoded version)
DEST[31:0] <- MIN(SRC1[31:0], SRC2[31:0])
DEST[63:32] <- MIN(SRC1[63:32], SRC2[63:32])
DEST[95:64] <- MIN(SRC1[95:64], SRC2[95:64])
DEST[127:96] <- MIN(SRC1[127:96], SRC2[127:96])
DEST[159:128] <- MIN(SRC1[159:128], SRC2[159:128])
DEST[191:160] <- MIN(SRC1[191:160], SRC2[191:160])
DEST[223:192] <- MIN(SRC1[223:192], SRC2[223:192])
DEST[255:224] <- MIN(SRC1[255:224], SRC2[255:224])

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 0F 5D /r MINPS xmm1, xmm2/m128            | RM   | V/V           | SSE               | Return the minimum single-precision       
                                           |      |               |                   | floatingpoint values between xmm2/m128    
                                           |      |               |                   | and xmm1.                                 
 VEX.NDS.128.0F.WIG 5D /r VMINPS xmm1,xmm2,| RVM  | V/V           | AVX               | Return the minimum single-precision       
 xmm3/m128                                 |      |               |                   | floatingpoint values between xmm2 and     
                                           |      |               |                   | xmm3/mem.                                 
 VEX.NDS.256.0F.WIG 5D /r VMINPS ymm1,     | RVM  | V/V           | AVX               | Return the minimum single double-precision
 ymm2, ymm3/m256                           |      |               |                   | floating-point values between ymm2 and    
                                           |      |               |                   | ymm3/mem.                                 

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs an SIMD compare of the packed single-precision floating-point values
in the first source operand and the second source operand and returns the minimum
value for each pair of values to the destination operand. If the values being
compared are both 0.0s (of either sign), the value in the second operand (source
operand) is returned. If a value in the second operand is an SNaN, that SNaN
is forwarded unchanged to the destination (that is, a QNaN version of the SNaN
is not returned). If only one value is a NaN (SNaN or QNaN) for this instruction,
the second operand (source operand), either a NaN or a valid floating-point
value, is written to the result. If instead of this behavior, it is required
that the NaN source operand (from either the first or second operand) be returned,
the action of MINPS can be emulated using a sequence of instructions, such as,
a comparison followed by AND, ANDN and OR.

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
 MINPS: | __m128d _mm_min_ps(__m128d a, __m128d 
        | b);                                   
 VMINPS:| __m256 _mm256_min_ps (__m256 a, __m256
        | b);                                   

### SIMD Floating-Point Exceptions
Invalid (including QNaN source operand), Denormal.


### Other Exceptions
See Exceptions Type 2.
