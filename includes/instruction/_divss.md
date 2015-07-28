## DIVSS - Divide Scalar Single-Precision Floating-Point Values

> Operation

``` slim
DIVSS (128-bit Legacy SSE version)
DEST[31:0] <- DEST[31:0] / SRC[31:0]
DEST[VLMAX-1:32] (Unmodified)
VDIVSS (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0] / SRC2[31:0]
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 DIVSS:| __m128 _mm_div_ss(__m128 a, __m128 b)

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 F3 0F 5E /r DIVSS xmm1, xmm2/m32        | RM   | V/V           | SSE               | Divide low single-precision floating-point 
                                         |      |               |                   | value in xmm1 by low single-precision      
                                         |      |               |                   | floating-point value in xmm2/m32.          
 VEX.NDS.LIG.F3.0F.WIG 5E /r VDIVSS xmm1,| RVM  | V/V           | AVX               | Divide low single-precision floating       
 xmm2, xmm3/m32                          |      |               |                   | point value in xmm2 by low single precision
                                         |      |               |                   | floating-point value in xmm3/m32.          

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Divides the low single-precision floating-point value in the first source operand
by the low single-precision floatingpoint value in the second source operand,
and stores the single-precision floating-point result in the destination operand.
The second source operand can be an XMM register or a 32-bit memory location.
The first source and destination operands are XMM registers. The three high-order
doublewords of the destination are copied from the same dwords of the first
source operand. See Chapter 10 in the Intel® 64 and IA-32 Architectures Software
Developer's Manual, Volume 1, for an overview of a scalar single-precision floating-point
operation.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The first source operand
and the destination operand are the same. Bits (VLMAX1:32) of the corresponding
YMM destination register remain unchanged. VEX.128 encoded version: Bits (VLMAX-1:128)
of the destination YMM register are zeroed.



### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Divide-by-Zero, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3.
