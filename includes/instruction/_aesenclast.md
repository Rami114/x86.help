## AESENCLAST - Perform Last Round of an AES Decryption Flow 
> Operation 

``` slim
// AESENCLAST 
STATE <-SRC1;
RoundKey <- SRC2;
STATE <- ShiftRows( STATE );
STATE <- SubBytes( STATE );
DEST[127:0] <- STATE XOR RoundKey;
DEST[VLMAX-1:128] (Unmodified)

// VAESENCLAST 
STATE <- SRC1;
RoundKey <- SRC2;
STATE <- ShiftRows( STATE );
STATE <- SubBytes( STATE );
DEST[127:0] <- STATE XOR RoundKey;
DEST[VLMAX-1:128] <- 0
```

> Intel C/C++ Compiler Intrinsic Equivalent

``` c
// (V)AESENCLAST 
__m128i _mm_aesenclast (__m128i, __m128i)
```

Opcode / Instruction | Op/En | 64/32bit Mode Support | CPUID Feature Flag | Description
-------------------- | ----- | ----------- | --------------- | -----------
66 0F 38 DD /r AESENCLAST xmm1, xmm2/m128 | RM  |    V/V              | AES             | Perform the last round of an AES encryption flow, using the Equivalent Inverse Cipher, operating on a 128-bit data (state) from xmm1 with a 128-bit round key from xmm2/m128.
VEX.NDS.128.66.0F38.WIG DD /r VAESENCLAST xmm1, xmm2, xmm3/m128 | RVM |  V/V |               Both AES and AVX flags |Perform the last round of an AES encryption flow, using the Equivalent Inverse Cipher, operating on a 128-bit data (state) from xmm2 with a 128-bit round key from xmm3/m128; store the result in xmm1.

### Instruction Operand Encoding
Op/En  | Operand 1  | Operand 2  | Operand 3  | Operand 4
------ | ---------- | ---------- | ---------- | ---------
RM             |      ModRM:reg (r, w)                         | ModRM:r/m (r )                             |         NA       |                                         NA
RVM             |      ModRM:reg (w)                           |  VEX.vvvv (r )                             | ModRM:r/m (r )  |                                     NA

### Description
This instruction performs the last round of the AES encryption flow using the round key from the second source 
operand, operating on a 128-bit data (state) from the first source operand, and store the result in the destination 
operand. 

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
