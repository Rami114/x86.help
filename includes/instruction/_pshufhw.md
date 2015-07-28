## PSHUFHW - Shuffle Packed High Words

> Operation
``` slim

PSHUFHW (128-bit Legacy SSE version)
DEST[63:0] <- SRC[63:0]
DEST[79:64] <- (SRC >> (imm[1:0] \*16))[79:64]
DEST[95:80] <- (SRC >> (imm[3:2] \* 16))[79:64]
DEST[111:96] <- (SRC >> (imm[5:4] \* 16))[79:64]
DEST[127:112] <- (SRC >> (imm[7:6] \* 16))[79:64]
DEST[VLMAX-1:128] (Unmodified)
VPSHUFHW (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0]
DEST[79:64] <- (SRC1 >> (imm[1:0] \*16))[79:64]
DEST[95:80] <- (SRC1 >> (imm[3:2] \* 16))[79:64]
DEST[111:96] <- (SRC1 >> (imm[5:4] \* 16))[79:64]
DEST[127:112] <- (SRC1 >> (imm[7:6] \* 16))[79:64]
DEST[VLMAX-1:128] <- 0
VPSHUFHW (VEX.256 encoded version)
DEST[63:0] <- SRC1[63:0]
DEST[79:64] <- (SRC1 >> (imm[1:0] \*16))[79:64]
DEST[95:80] <- (SRC1 >> (imm[3:2] \* 16))[79:64]
DEST[111:96] <- (SRC1 >> (imm[5:4] \* 16))[79:64]
DEST[127:112] <- (SRC1 >> (imm[7:6] \* 16))[79:64]
DEST[191:128] <- SRC1[191:128]
DEST[207192] <- (SRC1 >> (imm[1:0] \*16))[207:192]
DEST[223:208] <- (SRC1 >> (imm[3:2] \* 16))[207:192]
DEST[239:224] <- (SRC1 >> (imm[5:4] \* 16))[207:192]
DEST[255:240] <- (SRC1 >> (imm[7:6] \* 16))[207:192]

```

 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 F3 0F 70 /r ib PSHUFHW xmm1, xmm2/m128,| RMI  | V/V                   | SSE2              | Shuffle the high words in xmm2/m128    
 imm8                                   |      |                       |                   | based on the encoding in imm8 and store
                                        |      |                       |                   | the result in xmm1.                    
 VEX.128.F3.0F.WIG 70 /r ib VPSHUFHW    | RMI  | V/V                   | AVX               | Shuffle the high words in xmm2/m128    
 xmm1, xmm2/m128, imm8                  |      |                       |                   | based on the encoding in imm8 and store
                                        |      |                       |                   | the result in xmm1.                    
 VEX.256.F3.0F.WIG 70 /r ib VPSHUFHW    | RMI  | V/V                   | AVX2              | Shuffle the high words in ymm2/m256    
 ymm1, ymm2/m256, imm8                  |      |                       |                   | based on the encoding in imm8 and store
                                        |      |                       |                   | the result in ymm1.                    

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8     | NA       

### Description
Copies words from the high quadword of a 128-bit lane of the source operand
and inserts them in the high quadword of the destination operand at word locations
(of the respective lane) selected with the immediate operand. This 256-bit operation
is similar to the in-lane operation used by the 256-bit VPSHUFD instruction,
which is illustrated in Figure 4-12. For 128-bit operation, only the low 128-bit
lane is operative. Each 2-bit field in the immediate operand selects the contents
of one word location in the high quadword of the destination operand. The binary
encodings of the immediate operand fields select words (0, 1, 2 or 3, 4) from
the high quadword of the source operand to be copied to the destination operand.
The low quadword of the source operand is copied to the low quadword of the
destination operand, for each 128-bit lane. Note that this instruction permits
a word in the high quadword of the source operand to be copied to more than
one word location in the high quadword of the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
destination operand is an XMM register. The source operand can be an XMM register
or a 128-bit memory location. Bits (VLMAX-1:128) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: The destination operand
is an XMM register. The source operand can be an XMM register or a 128-bit memory
location. Bits (VLMAX-1:128) of the destination YMM register are zeroed. VEX.vvvv
is reserved and must be 1111b, VEX.L must be 0, otherwise the instruction will
#UD. VEX.256 encoded version: The destination operand is an YMM register. The
source operand can be an YMM register or a 256-bit memory location. Note: In
VEX encoded versions VEX.vvvv is reserved and must be 1111b otherwise instructions
will #UD.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PSHUFHW:| __m128i _mm_shufflehi_epi16(__m128i   
            | a, int n)                             
 VPSHUFHW:  | __m256i _mm256_shufflehi_epi16(__m256i
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
