## MULSD - Multiply Scalar Double-Precision Floating-Point Values

> Operation
``` slim

MULSD (128-bit Legacy SSE version)
DEST[63:0] <- DEST[63:0] \* SRC[63:0]
DEST[VLMAX-1:64] (Unmodified)
VMULSD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0] \* SRC2[63:0]
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                          | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 F2 0F 59 /r MULSD xmm1, xmm2/m64            | RM   | V/V           | SSE2              | Multiply the low double-precision floatingpoint
                                             |      |               |                   | value in xmm2/mem64 by low doubleprecision     
                                             |      |               |                   | floating-point value in xmm1.                  
 VEX.NDS.LIG.F2.0F.WIG 59/r VMULSD xmm1,xmm2,| RVM  | V/V           | AVX               | Multiply the low double-precision floatingpoint
 xmm3/m64                                    |      |               |                   | value in xmm3/mem64 by low double precision    
                                             |      |               |                   | floating-point value in xmm2.                  

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Multiplies the low double-precision floating-point value in the source operand
(second operand) by the low doubleprecision floating-point value in the destination
operand (first operand), and stores the double-precision floatingpoint result
in the destination operand. The source operand can be an XMM register or a 64-bit
memory location. The destination operand is an XMM register. The high quadword
of the destination operand remains unchanged. See Figure 11-4 in the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1, for an illustration
of a scalar double-precision floating-point operation.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The first source operand
and the destination operand are the same. Bits (VLMAX1:64) of the corresponding
YMM destination register remain unchanged. VEX.128 encoded version: Bits (VLMAX-1:128)
of the destination YMM register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MULSD:| __m128d _mm_mul_sd (m128d a, m128d b)

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3
