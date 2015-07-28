## SUBSD - Subtract Scalar Double-Precision Floating-Point Values

> Operation

``` slim
SUBSD (128-bit Legacy SSE version)
DEST[63:0] <- DEST[63:0] - SRC[63:0]
DEST[VLMAX-1:64] (Unmodified)
VSUBSD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0] - SRC2[63:0]
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 SUBSD:| __m128d _mm_sub_sd (m128d a, m128d b)

```

 Opcode/Instruction                           | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                     
 ---  | --- | --- | --- | ---
 F2 0F 5C /r SUBSD xmm1, xmm2/m64             | RM   | V/V                   | SSE2              | Subtracts the low double-precision floatingpoint
                                              |      |                       |                   | values in xmm2/mem64 from xmm1.                 
 VEX.NDS.LIG.F2.0F.WIG 5C /r VSUBSD xmm1,xmm2,| RVM  | V/V                   | AVX               | Subtract the low double-precision floatingpoint 
 xmm3/m64                                     |      |                       |                   | value in xmm3/mem from xmm2 and store           
                                              |      |                       |                   | the result in xmm1.                             

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Subtracts the low double-precision floating-point value in the source operand
(second operand) from the low double-precision floating-point value in the destination
operand (first operand), and stores the double-precision floating-point result
in the destination operand. The source operand can be an XMM register or a 64-bit
memory location. The destination operand is an XMM register. The high quadword
of the destination operand remains unchanged. See Figure 11-4 in the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1, for an illustration
of a scalar double-precision floating-point operation.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
destination and first source operand are the same. Bits (VLMAX-1:64) of the
### corresponding YMM destination register remain unchanged. VEX.128 encoded version
Bits (127:64) of the XMM register destination are copied from corresponding
bits in the first source operand. Bits (VLMAX-1:128) of the destination YMM
register are zeroed.



### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3.
