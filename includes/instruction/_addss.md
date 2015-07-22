## ADDSS - Add Scalar Single-Precision Floating-Point Values 
> Operation 

``` slim
// ADDSS DEST, SRC (128-bit Legacy SSE version)
DEST[31:0] <- DEST[31:0] + SRC[31:0];
DEST[VLMAX-1:32] (Unmodified)

// VADDSS DEST, SRC1, SRC2 (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0] + SRC2[31:0]
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0
```

> Intel C/C++ Compiler Intrinsic Equivalent

``` c
// ADDSS
__m128 _mm_add_ss (m128d a, m128d b)
```

Opcode / Instruction | Op/En | 64/32bit Mode Support | CPUID Feature Flag | Description
-------------------- | ----- | ----------- | --------------- | -----------
F3 0F 58 /r ADDSS xmm1, xmm2/m32 | RM   |   V/V           |    SSE              | Add the low single-precision floating-point value from xmm2/m32 to xmm1.
VEX.NDS.LIG.F3.0F.WIG 58 /r VADDSS xmm1,xmm2, xmm3/m32 | RVM  | V/V  |             AVX |            Add the low single-precision floating-point value from xmm3/mem to xmm2 and store the result in xmm1.

### Instruction Operand Encoding
Op/En  | Operand 1  | Operand 2  | Operand 3  | Operand 4
------ | ---------- | ---------- | ---------- | ---------
RM             |     ModRM:reg (r, w)       |                  ModRM:r/m (r )        |                               NA        |                                         NA
RVM             |     ModRM:reg (w)          |                  VEX.vvvv (r )         |                       ModRM:r/m (r )     |                                   NA

### Description
Adds the low single-precision floating-point values from the source operand (second operand) and the destination 
operand (first operand), and stores the single-precision floating-point result in the destination operand. 

The source operand can be an XMM register or a 32-bit memory location. The destination operand is an XMM 
register. See Chapter 10 in the Intel® 64 and IA-32 Architectures Software Developer’s Manual, Volume 1, for an 
overview of a scalar single-precision floating-point operation.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction to access additional registers 
(XMM8-XMM15).

128-bit Legacy SSE version: Bits (VLMAX-1:32) of the corresponding YMM destination register remain unchanged.

VEX.128 encoded version: Bits (127:32) of the XMM register destination are copied from corresponding bits in the 
first source operand. Bits (VLMAX-1:128) of the destination YMM register are zeroed.

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.

### Other Exceptions
See Exceptions Type 3.
