## MAXSS - Return Maximum Scalar Single-Precision Floating-Point Value

> Operation

``` slim
MAX(SRC1, SRC2)
{
  IF ((SRC1 = 0.0) and (SRC2 = 0.0)) THEN DEST <- SRC2;
     ELSE IF (SRC1 = SNaN) THEN DEST <- SRC2; FI;
     ELSE IF SRC2 = SNaN) THEN DEST <- SRC2; FI;
     ELSE IF (SRC1 > SRC2) THEN DEST <- SRC1;
     ELSE DEST <- SRC2;
  FI;
}
MAXSS (128-bit Legacy SSE version)
DEST[31:0] <-MAX(DEST[31:0], SRC[31:0])
DEST[VLMAX-1:32] (Unmodified)
VMAXSS (VEX.128 encoded version)
DEST[31:0] <-MAX(SRC1[31:0], SRC2[31:0])
DEST[127:32] <-SRC1[127:32]
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
__m128d _mm_max_ss(__m128d a, __m128d b)


```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 F3 0F 5F /r MAXSS xmm1, xmm2/m32        | RM   | V/V           | SSE               | Return the maximum scalar single-precision
                                         |      |               |                   | floating-point value between xmm2/mem32   
                                         |      |               |                   | and xmm1.                                 
 VEX.NDS.LIG.F3.0F.WIG 5F /r VMAXSS xmm1,| RVM  | V/V           | AVX               | Return the maximum scalar single-precision
 xmm2, xmm3/m32                          |      |               |                   | floating-point value between xmm3/mem32   
                                         |      |               |                   | and xmm2.                                 

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Compares the low single-precision floating-point values in the first source
operand and the second source operand, and returns the maximum value to the
low doubleword of the destination operand. If the values being compared are
both 0.0s (of either sign), the value in the second source operand is returned.
If a value in the second source operand is an SNaN, that SNaN is returned unchanged
to the destination (that is, a QNaN version of the SNaN is not returned). If
only one value is a NaN (SNaN or QNaN) for this instruction, the second source
operand, either a NaN or a valid floating-point value, is written to the result.
If instead of this behavior, it is required that the NaN from either source
operand be returned, the action of MAXSS can be emulated using a sequence of
instructions, such as, a comparison followed by AND, ANDN and OR. The second
source operand can be an XMM register or a 32-bit memory location. The first
source and destination operands are XMM registers.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The destination and first
source operand are the same. Bits (VLMAX-1:32) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: Bits (127:32) of the XMM
register destination are copied from corresponding bits in the first source
operand. Bits (VLMAX-1:128) of the destination YMM register are zeroed.



### SIMD Floating-Point Exceptions
Invalid (including QNaN source operand), Denormal.


### Other Exceptions
See Exceptions Type 3.
