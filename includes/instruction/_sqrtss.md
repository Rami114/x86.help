## SQRTSS - Compute Square Root of Scalar Single-Precision Floating-Point Value

> Operation
``` slim

SQRTSS (128-bit Legacy SSE version)
DEST[31:0] <- SQRT(SRC2[31:0])
DEST[VLMAX-1:32] (Unmodified)
VSQRTSS (VEX.128 encoded version)
DEST[31:0] <- SQRT(SRC2[31:0])
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0

```

 Opcode\*/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 F3 0F 51 /r SQRTSS xmm1, xmm2/m32       | RM   | V/V                   | SSE               | Computes square root of the low singleprecision
                                         |      |                       |                   | floating-point value in xmm2/m32 and           
                                         |      |                       |                   | stores the results in xmm1.                    
 VEX.NDS.LIG.F3.0F.WIG 51/r VSQRTSS xmm1,| RVM  | V/V                   | AVX               | Computes square root of the low singleprecision
 xmm2, xmm3/m32                          |      |                       |                   | floating-point value in xmm3/m32 and           
                                         |      |                       |                   | stores the results in xmm1. Also, upper        
                                         |      |                       |                   | single precision floating-point values         
                                         |      |                       |                   | (bits[127:32]) from xmm2 are copied            
                                         |      |                       |                   | to xmm1[127:32].                               

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Computes the square root of the low single-precision floating-point value in
the source operand (second operand) and stores the single-precision floating-point
result in the destination operand. The source operand can be an XMM register
or a 32-bit memory location. The destination operand is an XMM register. The
three high-order doublewords of the destination operand remain unchanged. See
Figure 10-6 in the Intel® 64 and IA-32 Architectures Software Developer's Manual,
Volume 1, for an illustration of a scalar single-precision floating-point operation.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
first source operand and the destination operand are the same. Bits (VLMAX1:32)
of the corresponding YMM destination register remain unchanged. VEX.128 encoded
version: Bits (VLMAX-1:128) of the destination YMM register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 SQRTSS:| __m128 _mm_sqrt_ss(__m128 a)

### SIMD Floating-Point Exceptions
Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3.
