## MAXSD - Return Maximum Scalar Double-Precision Floating-Point Value

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
MAXSD (128-bit Legacy SSE version)
DEST[63:0] <-MAX(DEST[63:0], SRC[63:0])
DEST[VLMAX-1:64] (Unmodified)
VMAXSD (VEX.128 encoded version)
DEST[63:0] <-MAX(SRC1[63:0], SRC2[63:0])
DEST[127:64] <-SRC1[127:64]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 F2 0F 5F /r MAXSD xmm1, xmm2/m64        | RM   | V/V           | SSE2              | Return the maximum scalar double-precision
                                         |      |               |                   | floating-point value between xmm2/mem64   
                                         |      |               |                   | and xmm1.                                 
 VEX.NDS.LIG.F2.0F.WIG 5F /r VMAXSD xmm1,| RVM  | V/V           | AVX               | Return the maximum scalar double-precision
 xmm2, xmm3/m64                          |      |               |                   | floating-point value between xmm3/mem64   
                                         |      |               |                   | and xmm2.                                 

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Compares the low double-precision floating-point values in the first source
operand and second the source operand, and returns the maximum value to the
low quadword of the destination operand. The second source operand can be an
XMM register or a 64-bit memory location. The first source and destination operands
are XMM registers. When the second source operand is a memory operand, only
64 bits are accessed. The high quadword of the destination operand is copied
from the same bits of first source operand. If the values being compared are
both 0.0s (of either sign), the value in the second source operand is returned.
If a value in the second source operand is an SNaN, that SNaN is returned unchanged
to the destination (that is, a QNaN version of the SNaN is not returned). If
only one value is a NaN (SNaN or QNaN) for this instruction, the second source
operand, either a NaN or a valid floating-point value, is written to the result.
If instead of this behavior, it is required that the NaN of either source operand
be returned, the action of MAXSD can be emulated using a sequence of instructions,
such as, a comparison followed by AND, ANDN and OR. The second source operand
can be an XMM register or a 64-bit memory location. The first source and destination
operands are XMM registers.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The destination and first
source operand are the same. Bits (VLMAX-1:64) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: Bits (127:64) of the XMM
register destination are copied from corresponding bits in the first source
operand. Bits (VLMAX-1:128) of the destination YMM register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MAXSD:| __m128d _mm_max_sd(__m128d a, __m128d
       | b)                                   

### SIMD Floating-Point Exceptions
Invalid (including QNaN source operand), Denormal.


### Other Exceptions
See Exceptions Type 3.
