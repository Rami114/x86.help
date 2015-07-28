## PSHUFLW - Shuffle Packed Low Words

> Operation
``` slim

PSHUFLW (128-bit Legacy SSE version)
DEST[15:0] <- (SRC >> (imm[1:0] \*16))[15:0]
DEST[31:16] <- (SRC >> (imm[3:2] \* 16))[15:0]
DEST[47:32] <- (SRC >> (imm[5:4] \* 16))[15:0]
DEST[63:48] <- (SRC >> (imm[7:6] \* 16))[15:0]
DEST[127:64] <- SRC[127:64]
DEST[VLMAX-1:128] (Unmodified)
VPSHUFLW (VEX.128 encoded version)
DEST[15:0] <- (SRC1 >> (imm[1:0] \*16))[15:0]
DEST[31:16] <- (SRC1 >> (imm[3:2] \* 16))[15:0]
DEST[47:32] <- (SRC1 >> (imm[5:4] \* 16))[15:0]
DEST[63:48] <- (SRC1 >> (imm[7:6] \* 16))[15:0]
DEST[127:64] <- SRC[127:64]
DEST[VLMAX-1:128] <- 0
VPSHUFLW (VEX.256 encoded version)
DEST[15:0] <- (SRC1 >> (imm[1:0] \*16))[15:0]
DEST[31:16] <- (SRC1 >> (imm[3:2] \* 16))[15:0]
DEST[47:32] <- (SRC1 >> (imm[5:4] \* 16))[15:0]
DEST[63:48] <- (SRC1 >> (imm[7:6] \* 16))[15:0]
DEST[127:64] <- SRC1[127:64]
DEST[143:128] <- (SRC1 >> (imm[1:0] \*16))[143:128]
DEST[159:144] <- (SRC1 >> (imm[3:2] \* 16))[143:128]
DEST[175:160] <- (SRC1 >> (imm[5:4] \* 16))[143:128]
DEST[191:176] <- (SRC1 >> (imm[7:6] \* 16))[143:128]
DEST[255:192] <- SRC1[255:192]

```

 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 F2 0F 70 /r ib PSHUFLW xmm1, xmm2/m128,| RMI  | V/V                   | SSE2              | Shuffle the low words in xmm2/m128 based
 imm8                                   |      |                       |                   | on the encoding in imm8 and store the   
                                        |      |                       |                   | result in xmm1.                         
 VEX.128.F2.0F.WIG 70 /r ib VPSHUFLW    | RMI  | V/V                   | AVX               | Shuffle the low words in xmm2/m128 based
 xmm1, xmm2/m128, imm8                  |      |                       |                   | on the encoding in imm8 and store the   
                                        |      |                       |                   | result in xmm1.                         
 VEX.256.F2.0F.WIG 70 /r ib VPSHUFLW    | RMI  | V/V                   | AVX2              | Shuffle the low words in ymm2/m256 based
 ymm1, ymm2/m256, imm8                  |      |                       |                   | on the encoding in imm8 and store the   
                                        |      |                       |                   | result in ymm1.                         

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8     | NA       

### Description
Copies words from the low quadword of a 128-bit lane of the source operand and
inserts them in the low quadword of the destination operand at word locations
(of the respective lane) selected with the immediate operand. The 256-bit operation
is similar to the in-lane operation used by the 256-bit VPSHUFD instruction,
which is illustrated in Figure 4-12. For 128-bit operation, only the low 128-bit
lane is operative. Each 2-bit field in the immediate operand selects the contents
of one word location in the low quadword of the destination operand. The binary
encodings of the immediate operand fields select words (0, 1, 2 or 3) from the
low quadword of the source operand to be copied to the destination operand.
The high quadword of the source operand is copied to the high quadword of the
destination operand, for each 128-bit lane. Note that this instruction permits
a word in the low quadword of the source operand to be copied to more than one
word location in the low quadword of the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
destination operand is an XMM register. The source operand can be an XMM register
or a 128-bit memory location. Bits (VLMAX-1:128) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: The destination operand
is an XMM register. The source operand can be an XMM register or a 128-bit memory
location. Bits (VLMAX-1:128) of the destination YMM register are zeroed. VEX.256
encoded version: The destination operand is an YMM register. The source operand
can be an YMM register or a 256-bit memory location.

<aside class="notification">
VEX.vvvv is reserved and must be 1111b, VEX.L must be 0, otherwise instructions
will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PSHUFLW:| __m128i _mm_shufflelo_epi16(__m128i   
            | a, int n)                             
 VPSHUFLW:  | __m256i _mm256_shufflelo_epi16(__m256i
            | a, const int n)                       

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1. If VEX.vvvv != 1111B.
