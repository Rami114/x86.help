## AESENC - Perform One Round of an AES Encryption Flow 
> Operation 

``` slim
// AESENC 
STATE <- SRC1;
RoundKey <- SRC2;
STATE <- ShiftRows( STATE );
STATE <- SubBytes( STATE );
STATE <- MixColumns( STATE );
DEST[127:0] <- STATE XOR RoundKey;
DEST[VLMAX-1:128] (Unmodified)

// VAESENC 
STATE <- SRC1;
RoundKey <- SRC2;
STATE <- ShiftRows( STATE );
STATE <- SubBytes( STATE );
STATE <- MixColumns( STATE );
DEST[127:0] <- STATE XOR RoundKey;
DEST[VLMAX-1:128] <- 0
```

> Intel C/C++ Compiler Intrinsic Equivalent

``` c
// (V)AESENC
__m128i _mm_aesenc (__m128i, __m128i)
```

Opcode / Instruction | Op/En | 64/32bit Mode Support | CPUID Feature Flag | Description
-------------------- | ----- | ----------- | --------------- | -----------
66 0F 38 DC /r AESENC xmm1, xmm2/m128 | RM  |    V/V              | AES             | Perform one round of an AES encryption flow, using the Equivalent Inverse Cipher, operating on a 128-bit data (state) from xmm1 with a 128-bit round key from xmm2/m128.
VEX.NDS.128.66.0F38.WIG DC /r VAESENC xmm1, xmm2, xmm3/m128 | RVM |  V/V |               Both AES and AVX flags |Perform one round of an AES encryption flow, using the Equivalent Inverse Cipher, operating on a 128-bit data (state) from xmm2 with a 128-bit round key from xmm3/m128; store the result in xmm1.

### Instruction Operand Encoding
Op/En  | Operand 1  | Operand 2  | Operand 3  | Operand 4
------ | ---------- | ---------- | ---------- | ---------
RM             |      ModRM:reg (r, w)                         | ModRM:r/m (r )                             |         NA       |                                         NA
RVM             |      ModRM:reg (w)                           |  VEX.vvvv (r )                             | ModRM:r/m (r )  |                                     NA

### Description
This instruction performs a single round of an AES encryption flow using a round key from the second source 
operand, operating on 128-bit data (state) from the first source operand, and store the result in the destination 
operand. 

<aside class="notice">
Use the AESENC instruction for all but the last encryption round. For the last encryption round, use the AESENCLAST instruction.
</aside>

128-bit Legacy SSE version: The first source operand and the destination operand are the same and must be an 
XMM register. The second source operand can be an XMM register or a 128-bit memory location. Bits (VLMAX-
1:128) of the corresponding YMM destination register remain unchanged.

VEX.128 encoded version: The first source operand and the destination operand are XMM registers. The second 
source operand can be an XMM register or a 128-bit memory location. Bits (VLMAX-1:128) of the destination YMM 
register are zeroed.

### SIMD Floating-Point Exceptions
None

### Other Exceptions
See Exceptions Type 4.
