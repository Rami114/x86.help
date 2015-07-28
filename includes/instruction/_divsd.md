## DIVSD - Divide Scalar Double-Precision Floating-Point Values

> Operation
``` slim

DIVSD (128-bit Legacy SSE version)
DEST[63:0] <- DEST[63:0] / SRC[63:0]
DEST[VLMAX-1:64] (Unmodified)
VDIVSD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0] / SRC2[63:0]
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 F2 0F 5E /r DIVSD xmm1, xmm2/m64        | RM   | V/V           | SSE2              | Divide low double-precision floating-point  
                                         |      |               |                   | value in xmm1 by low double-precision       
                                         |      |               |                   | floating-point value in xmm2/mem64.         
 VEX.NDS.LIG.F2.0F.WIG 5E /r VDIVSD xmm1,| RVM  | V/V           | AVX               | Divide low double-precision floating        
 xmm2, xmm3/m64                          |      |               |                   | point values in xmm2 by low double precision
                                         |      |               |                   | floating-point value in xmm3/mem64.         

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Divides the low double-precision floating-point value in the first source operand
by the low double-precision floating-point value in the second source operand,
and stores the double-precision floating-point result in the destination operand.
The second source operand can be an XMM register or a 64-bit memory location.
The first source and destination hyperons are XMM registers. The high quadword
of the destination operand is copied from the high quadword of the first source
operand. See Chapter 11 in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 1, for an overview of a scalar double-precision floating-point
operation.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The first source operand
and the destination operand are the same. Bits (VLMAX1:64) of the corresponding
YMM destination register remain unchanged. VEX.128 encoded version: Bits (VLMAX-1:128)
of the destination YMM register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 DIVSD:| __m128d _mm_div_sd (m128d a, m128d b)

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Divide-by-Zero, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3.
