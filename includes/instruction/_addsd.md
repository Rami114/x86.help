## ADDSD - Add Scalar Double-Precision Floating-Point Values 
> Operation 

``` slim
// ADDSD (128-bit Legacy SSE version)
DEST[63:0] <- DEST[63:0] + SRC[63:0]
DEST[VLMAX-1:64] (Unmodified)

// VADDSD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0] + SRC2[63:0]
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0
```

> Intel C/C++ Compiler Intrinsic Equivalent

``` c
// ADDSD
__m128d _mm_add_sd (m128d a, m128d b)
```

Opcode / Instruction | Op/En | 64/32bit Mode Support | CPUID Feature Flag | Description
-------------------- | ----- | ----------- | --------------- | -----------
F2 0F 58 /r ADDSD xmm1, xmm2/m64 | RM   |   V/V     |          SSE2      |      Add the low double-precision floating-point value from xmm2/m64 to xmm1.
VEX.NDS.LIG.F2.0F.WIG 58 /r VADDSD xmm1, xmm2, xmm3/m64 | RVM  | V/V     |          AVX      |       Add the low double-precision floating-point value from xmm3/mem to xmm2 and store the result in xmm1.

### Instruction Operand Encoding
Op/En  | Operand 1  | Operand 2  | Operand 3  | Operand 4
------ | ---------- | ---------- | ---------- | ---------
RM         |          ModRM:reg (r, w)        |                  ModRM:r/m (r )           |                           NA                     |                          NA
RVM         |          ModRM:reg (w)           |                  VEX.vvvv (r )            |                 ModRM:r/m (r )                   |                  NA

### Description
Adds the low double-precision floating-point values from the source operand (second operand) and the destination 
operand (first operand), and stores the double-precision floating-point result in the destination operand. 
The source operand can be an XMM register or a 64-bit memory location. The destination operand is an XMM 
register. See Chapter 11 in the Intel® 64 and IA-32 Architectures Software Developer’s Manual, Volume 1, for an 
overview of a scalar double-precision floating-point operation.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction to access additional registers 
(XMM8-XMM15).

128-bit Legacy SSE version: Bits (VLMAX-1:64) of the corresponding YMM destination register remain unchanged.

VEX.128 encoded version: Bits (127:64) of the XMM register destination are copied from corresponding bits in the 
first source operand. Bits (VLMAX-1:128) of the destination YMM register are zeroed. 

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.

### Other Exceptions
See Exceptions Type 3.
