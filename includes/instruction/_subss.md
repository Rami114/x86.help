## SUBSS - Subtract Scalar Single-Precision Floating-Point Values

> Operation

``` slim
SUBSS (128-bit Legacy SSE version)
DEST[31:0] <- DEST[31:0] - SRC[31:0]
DEST[VLMAX-1:32] (Unmodified)
VSUBSS (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0] - SRC2[31:0]
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                           | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 F3 0F 5C /r SUBSS xmm1, xmm2/m32             | RM   | V/V                   | SSE               | Subtract the lower single-precision            
                                              |      |                       |                   | floatingpoint values in xmm2/m32 from          
                                              |      |                       |                   | xmm1.                                          
 VEX.NDS.LIG.F3.0F.WIG 5C /r VSUBSS xmm1,xmm2,| RVM  | V/V                   | AVX               | Subtract the low single-precision floatingpoint
 xmm3/m32                                     |      |                       |                   | value in xmm3/mem from xmm2 and store          
                                              |      |                       |                   | the result in xmm1.                            

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Subtracts the low single-precision floating-point value in the source operand
(second operand) from the low singleprecision floating-point value in the destination
operand (first operand), and stores the single-precision floatingpoint result
in the destination operand. The source operand can be an XMM register or a 32-bit
memory location. The destination operand is an XMM register. The three high-order
doublewords of the destination operand remain unchanged. See Figure 10-6 in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 1,
for an illustration of a scalar single-precision floating-point operation.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
destination and first source operand are the same. Bits (VLMAX-1:32) of the
### corresponding YMM destination register remain unchanged. VEX.128 encoded version
Bits (127:32) of the XMM register destination are copied from corresponding
bits in the first source operand. Bits (VLMAX-1:128) of the destination YMM
register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 SUBSS:| __m128 _mm_sub_ss(__m128 a, __m128 b)

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3.
