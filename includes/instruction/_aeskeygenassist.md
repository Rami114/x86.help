## AESKEYGENASSIST - AES Round Key Generation Assist
> Operation 

``` slim
// AESKEYGENASSIST
X3[31:0] <- SRC [127: 96];
X2[31:0] <- SRC [95: 64];
X1[31:0] <- SRC [63: 32];
X0[31:0] <- SRC [31: 0];
RCON[31:0] <- ZeroExtend(Imm8[7:0]);
DEST[31:0] <- SubWord(X1);
DEST[63:32] <- RotWord( SubWord(X1) ) XOR RCON;
DEST[95:64] <- SubWord(X3);
DEST[127:96] <- RotWord( SubWord(X3) ) XOR RCON;
DEST[VLMAX-1:128] (Unmodified)

// VAESKEYGENASSIST 
X3[31:0] <- SRC [127: 96];
X2[31:0] <- SRC [95: 64];
X1[31:0] <- SRC [63: 32];
X0[31:0] <- SRC [31: 0];
RCON[31:0] <- ZeroExtend(Imm8[7:0]);
DEST[31:0] <- SubWord(X1);
DEST[63:32] <- RotWord( SubWord(X1) ) XOR RCON;
DEST[95:64] <- SubWord(X3);
DEST[127:96] <- RotWord( SubWord(X3) ) XOR RCON;
DEST[VLMAX-1:128] <- 0;
```

> Intel C/C++ Compiler Intrinsic Equivalent

``` c
// (V)AESKEYGENASSIST      
__m128i _mm_aeskeygenassist (__m128i, const int)
```

Opcode / Instruction | Op/En | 64/32bit Mode Support | CPUID Feature Flag | Description
-------------------- | ----- | ----------- | --------------- | -----------
6 0F 3A DF /r ib AESKEYGENASSIST xmm1, xmm2/m128, imm8 | RMI  |   V/V | AES | Assist in AES round key generation using an 8 bits Round Constant (RCON) specified in the immediate byte, operating on 128 bits of data specified in xmm2/m128 and stores the result in xmm1.
VEX.128.66.0F3A.WIG DF /r ib VAESKEYGENASSIST xmm1, xmm2/m128, imm8 | RMI  |   V/V |  Both AES and AVX flags | Assist in AES round key generation using 8 bits Round  onstant (RCON) specified in the immediate byte, operating on 128 bits of data specified in xmm2/m128 and stores the result in xmm1.
 
### Instruction Operand Encoding
Op/En  | Operand 1  | Operand 2  | Operand 3  | Operand 4
------ | ---------- | ---------- | ---------- | ---------
RMI | ModRM:reg (w) | ModRM:r/m (r ) | imm8 | NA

### Description
Assist in expanding the AES cipher key, by computing steps towards generating a round key for encryption, using 
128-bit data specified in the source operand and an 8-bit round constant specified as an immediate, store the 
result in the destination operand.

The destination operand is an XMM register. The source operand can be an XMM register or a 128-bit memory location.

128-bit Legacy SSE version: Bits (VLMAX-1:128) of the corresponding YMM destination register remain unchanged.

VEX.128 encoded version: Bits (VLMAX-1:128) of the destination YMM register are zeroed.

<aside class="notification">
In VEX-encoded versions, VEX.vvvv is reserved and must be 1111b, otherwise instructions will #UD
</aside>

### SIMD Floating-Point Exceptions
None

### Other Exceptions
See Exceptions Type 4; additionally

      | | 
 ---- | -----
 **`#UD`**   |  If VEX.vvvv ≠ 1111B.
