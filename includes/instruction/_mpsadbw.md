## MPSADBW  -  Compute Multiple Packed Sums of Absolute Difference

> Operation

``` slim
VMPSADBW (VEX.256 encoded version)
SRC2_OFFSET <- imm8[1:0]\*32
SRC1_OFFSET <- imm8[2]\*32
SRC1_BYTE0 <- SRC1[SRC1_OFFSET+7:SRC1_OFFSET]
SRC1_BYTE1 <- SRC1[SRC1_OFFSET+15:SRC1_OFFSET+8]
SRC1_BYTE2 <- SRC1[SRC1_OFFSET+23:SRC1_OFFSET+16]
SRC1_BYTE3 <- SRC1[SRC1_OFFSET+31:SRC1_OFFSET+24]
SRC1_BYTE4 <-SRC1[SRC1_OFFSET+39:SRC1_OFFSET+32]
SRC1_BYTE5 <- SRC1[SRC1_OFFSET+47:SRC1_OFFSET+40]
SRC1_BYTE6 <- SRC1[SRC1_OFFSET+55:SRC1_OFFSET+48]
SRC1_BYTE7 <- SRC1[SRC1_OFFSET+63:SRC1_OFFSET+56]
SRC1_BYTE8 <- SRC1[SRC1_OFFSET+71:SRC1_OFFSET+64]
SRC1_BYTE9 <- SRC1[SRC1_OFFSET+79:SRC1_OFFSET+72]
SRC1_BYTE10 <- SRC1[SRC1_OFFSET+87:SRC1_OFFSET+80]
SRC2_BYTE0 <-SRC2[SRC2_OFFSET+7:SRC2_OFFSET]
SRC2_BYTE1 <- SRC2[SRC2_OFFSET+15:SRC2_OFFSET+8]
SRC2_BYTE2 <- SRC2[SRC2_OFFSET+23:SRC2_OFFSET+16]
SRC2_BYTE3 <- SRC2[SRC2_OFFSET+31:SRC2_OFFSET+24]
TEMP0 <- ABS(SRC1_BYTE0 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE1 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE2 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE3 - SRC2_BYTE3)
DEST[15:0] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE1 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE2 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE3 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE4 - SRC2_BYTE3)
DEST[31:16] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE2 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE3 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE4 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE5 - SRC2_BYTE3)
DEST[47:32] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE3 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE4 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE5 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE6 - SRC2_BYTE3)
DEST[63:48] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE4 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE5 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE6 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE7 - SRC2_BYTE3)
DEST[79:64] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE5 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE6 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE7 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE8 - SRC2_BYTE3)
DEST[95:80] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE6 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE7 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE8 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE9 - SRC2_BYTE3)
DEST[111:96] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE7 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE8 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE9 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE10 - SRC2_BYTE3)
DEST[127:112] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
SRC2_OFFSET <- imm8[4:3]\*32 + 128
SRC1_OFFSET <- imm8[5]\*32 + 128
SRC1_BYTE0 <- SRC1[SRC1_OFFSET+7:SRC1_OFFSET]
SRC1_BYTE1 <- SRC1[SRC1_OFFSET+15:SRC1_OFFSET+8]
SRC1_BYTE2 <- SRC1[SRC1_OFFSET+23:SRC1_OFFSET+16]
SRC1_BYTE3 <- SRC1[SRC1_OFFSET+31:SRC1_OFFSET+24]
SRC1_BYTE4 <- SRC1[SRC1_OFFSET+39:SRC1_OFFSET+32]
SRC1_BYTE5 <- SRC1[SRC1_OFFSET+47:SRC1_OFFSET+40]
SRC1_BYTE6 <- SRC1[SRC1_OFFSET+55:SRC1_OFFSET+48]
SRC1_BYTE7 <- SRC1[SRC1_OFFSET+63:SRC1_OFFSET+56]
SRC1_BYTE8 <- SRC1[SRC1_OFFSET+71:SRC1_OFFSET+64]
SRC1_BYTE9 <- SRC1[SRC1_OFFSET+79:SRC1_OFFSET+72]
SRC1_BYTE10 <- SRC1[SRC1_OFFSET+87:SRC1_OFFSET+80]
SRC2_BYTE0 <-SRC2[SRC2_OFFSET+7:SRC2_OFFSET]
SRC2_BYTE1 <- SRC2[SRC2_OFFSET+15:SRC2_OFFSET+8]
SRC2_BYTE2 <- SRC2[SRC2_OFFSET+23:SRC2_OFFSET+16]
SRC2_BYTE3 <- SRC2[SRC2_OFFSET+31:SRC2_OFFSET+24]
TEMP0 <- ABS(SRC1_BYTE0 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE1 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE2 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE3 - SRC2_BYTE3)
DEST[143:128] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <-ABS(SRC1_BYTE1 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE2 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE3 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE4 - SRC2_BYTE3)
DEST[159:144] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE2 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE3 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE4 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE5 - SRC2_BYTE3)
DEST[175:160] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <-ABS(SRC1_BYTE3 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE4 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE5 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE6 - SRC2_BYTE3)
DEST[191:176] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE4 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE5 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE6 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE7 - SRC2_BYTE3)
DEST[207:192] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE5 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE6 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE7 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE8 - SRC2_BYTE3)
DEST[223:208] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE6 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE7 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE8 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE9 - SRC2_BYTE3)
DEST[239:224] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE7 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE8 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE9 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE10 - SRC2_BYTE3)
DEST[255:240] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
VMPSADBW (VEX.128 encoded version)
SRC2_OFFSET <- imm8[1:0]\*32
SRC1_OFFSET <- imm8[2]\*32
SRC1_BYTE0 <- SRC1[SRC1_OFFSET+7:SRC1_OFFSET]
SRC1_BYTE1 <- SRC1[SRC1_OFFSET+15:SRC1_OFFSET+8]
SRC1_BYTE2 <- SRC1[SRC1_OFFSET+23:SRC1_OFFSET+16]
SRC1_BYTE3 <- SRC1[SRC1_OFFSET+31:SRC1_OFFSET+24]
SRC1_BYTE4 <- SRC1[SRC1_OFFSET+39:SRC1_OFFSET+32]
SRC1_BYTE5 <- SRC1[SRC1_OFFSET+47:SRC1_OFFSET+40]
SRC1_BYTE6 <- SRC1[SRC1_OFFSET+55:SRC1_OFFSET+48]
SRC1_BYTE7 <- SRC1[SRC1_OFFSET+63:SRC1_OFFSET+56]
SRC1_BYTE8 <- SRC1[SRC1_OFFSET+71:SRC1_OFFSET+64]
SRC1_BYTE9 <- SRC1[SRC1_OFFSET+79:SRC1_OFFSET+72]
SRC1_BYTE10 <- SRC1[SRC1_OFFSET+87:SRC1_OFFSET+80]
SRC2_BYTE0 <-SRC2[SRC2_OFFSET+7:SRC2_OFFSET]
SRC2_BYTE1 <- SRC2[SRC2_OFFSET+15:SRC2_OFFSET+8]
SRC2_BYTE2 <- SRC2[SRC2_OFFSET+23:SRC2_OFFSET+16]
SRC2_BYTE3 <- SRC2[SRC2_OFFSET+31:SRC2_OFFSET+24]
TEMP0 <- ABS(SRC1_BYTE0 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE1 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE2 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE3 - SRC2_BYTE3)
DEST[15:0] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE1 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE2 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE3 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE4 - SRC2_BYTE3)
DEST[31:16] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE2 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE3 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE4 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE5 - SRC2_BYTE3)
DEST[47:32] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE3 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE4 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE5 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE6 - SRC2_BYTE3)
DEST[63:48] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE4 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE5 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE6 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE7 - SRC2_BYTE3)
DEST[79:64] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE5 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE6 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE7 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE8 - SRC2_BYTE3)
DEST[95:80] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE6 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE7 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE8 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE9 - SRC2_BYTE3)
DEST[111:96] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS(SRC1_BYTE7 - SRC2_BYTE0)
TEMP1 <- ABS(SRC1_BYTE8 - SRC2_BYTE1)
TEMP2 <- ABS(SRC1_BYTE9 - SRC2_BYTE2)
TEMP3 <- ABS(SRC1_BYTE10 - SRC2_BYTE3)
DEST[127:112] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
DEST[VLMAX-1:128] <- 0
MPSADBW (128-bit Legacy SSE version)
SRC_OFFSET <- imm8[1:0]\*32
DEST_OFFSET <- imm8[2]\*32
DEST_BYTE0 <- DEST[DEST_OFFSET+7:DEST_OFFSET]
DEST_BYTE1 <- DEST[DEST_OFFSET+15:DEST_OFFSET+8]
DEST_BYTE2 <- DEST[DEST_OFFSET+23:DEST_OFFSET+16]
DEST_BYTE3 <- DEST[DEST_OFFSET+31:DEST_OFFSET+24]
DEST_BYTE4 <- DEST[DEST_OFFSET+39:DEST_OFFSET+32]
DEST_BYTE5 <- DEST[DEST_OFFSET+47:DEST_OFFSET+40]
DEST_BYTE6 <- DEST[DEST_OFFSET+55:DEST_OFFSET+48]
DEST_BYTE7 <- DEST[DEST_OFFSET+63:DEST_OFFSET+56]
DEST_BYTE8 <- DEST[DEST_OFFSET+71:DEST_OFFSET+64]
DEST_BYTE9 <- DEST[DEST_OFFSET+79:DEST_OFFSET+72]
DEST_BYTE10 <- DEST[DEST_OFFSET+87:DEST_OFFSET+80]
SRC_BYTE0 <- SRC[SRC_OFFSET+7:SRC_OFFSET]
SRC_BYTE1 <- SRC[SRC_OFFSET+15:SRC_OFFSET+8]
SRC_BYTE2 <- SRC[SRC_OFFSET+23:SRC_OFFSET+16]
SRC_BYTE3 <- SRC[SRC_OFFSET+31:SRC_OFFSET+24]
TEMP0 <- ABS( DEST_BYTE0 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE1 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE2 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE3 - SRC_BYTE3)
DEST[15:0] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS( DEST_BYTE1 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE2 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE3 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE4 - SRC_BYTE3)
DEST[31:16] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS( DEST_BYTE2 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE3 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE4 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE5 - SRC_BYTE3)
DEST[47:32] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS( DEST_BYTE3 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE4 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE5 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE6 - SRC_BYTE3)
DEST[63:48] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS( DEST_BYTE4 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE5 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE6 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE7 - SRC_BYTE3)
DEST[79:64] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS( DEST_BYTE5 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE6 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE7 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE8 - SRC_BYTE3)
DEST[95:80] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS( DEST_BYTE6 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE7 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE8 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE9 - SRC_BYTE3)
DEST[111:96] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
TEMP0 <- ABS( DEST_BYTE7 - SRC_BYTE0)
TEMP1 <- ABS( DEST_BYTE8 - SRC_BYTE1)
TEMP2 <- ABS( DEST_BYTE9 - SRC_BYTE2)
TEMP3 <- ABS( DEST_BYTE10 - SRC_BYTE3)
DEST[127:112] <- TEMP0 + TEMP1 + TEMP2 + TEMP3
DEST[VLMAX-1:128] (Unmodified)

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 66 0F 3A 42 /r ib MPSADBW xmm1, xmm2/m128,| RMI  | V/V           | SSE4_1            | Sums absolute 8-bit integer difference  
 imm8                                      |      |               |                   | of adjacent groups of 4 byte integers   
                                           |      |               |                   | in xmm1 and xmm2/m128 and writes the    
                                           |      |               |                   | results in xmm1. Starting offsets within
                                           |      |               |                   | xmm1 and xmm2/m128 are determined by    
                                           |      |               |                   | imm8.                                   
 VEX.NDS.128.66.0F3A.WIG 42 /r ib VMPSADBW | RVMI | V/V           | AVX               | Sums absolute 8-bit integer difference  
 xmm1, xmm2, xmm3/m128, imm8               |      |               |                   | of adjacent groups of 4 byte integers   
                                           |      |               |                   | in xmm2 and xmm3/m128 and writes the    
                                           |      |               |                   | results in xmm1. Starting offsets within
                                           |      |               |                   | xmm2 and xmm3/m128 are determined by    
                                           |      |               |                   | imm8.                                   
 VEX.NDS.256.66.0F3A.WIG 42 /r ib VMPSADBW | RVMI | V/V           | AVX2              | Sums absolute 8-bit integer difference  
 ymm1, ymm2, ymm3/m256, imm8               |      |               |                   | of adjacent groups of 4 byte integers   
                                           |      |               |                   | in xmm2 and ymm3/m128 and writes the    
                                           |      |               |                   | results in ymm1. Starting offsets within
                                           |      |               |                   | ymm2 and xmm3/m128 are determined by    
                                           |      |               |                   | imm8.                                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
(V)MPSADBW sums the absolute difference of 4 unsigned bytes (block_2) in the
second source operand with sequential groups of 4 unsigned bytes (block_1) in
the first source operand. The immediate byte provides bit fields that specify
the initial offset of block_1 within the first source operand, and the offset
of block_2 within the second source operand. The offset granularity in both
source operands are 32 bits. The sum-absolute-difference (SAD) operation is
repeated 8 times for (V)MPSADW between the same block_2 (fixed offset within
the second source operand) and a variable block_1 (offset is shifted by 8 bits
for each SAD operation) in the first source operand. Each 16-bit result of eight
SAD operations is written to the respective word in the destination operand.
128-bit Legacy SSE version: Imm8[1:0]\*32 specifies the bit offset of block_2
within the second source operand. Imm[2]\*32 specifies the initial bit offset
of the block_1 within the first source operand. The first source operand and
destination operand are the same. The first source and destination operands
are XMM registers. The second source operand is either an XMM register or a
128-bit memory location. Bits (VLMAX-1:128) of the corresponding YMM destination
register remain unchanged. Bits 7:3 of the immediate byte are ignored. VEX.128
encoded version: Imm8[1:0]\*32 specifies the bit offset of block_2 within the
second source operand. Imm[2]\*32 specifies the initial bit offset of the block_1
within the first source operand. The first source and destination operands are
XMM registers. The second source operand is either an XMM register or a 128-bit
memory location. Bits (127:128) of the corresponding YMM register are zeroed.
Bits 7:3 of the immediate byte are ignored. VEX.256 encoded version: The sum-absolute-difference
(SAD) operation is repeated 8 times for MPSADW between the same block_2 (fixed
offset within the second source operand) and a variable block_1 (offset is shifted
by 8 bits for each SAD operation) in the first source operand. Each 16-bit result
of eight SAD operations between block_2 and block_1 is written to the respective
word in the lower 128 bits of the destination operand. Additionally, VMPSADBW
performs another eight SAD operations on block_4 of the second source operand
and block_3 of the first source operand. (Imm8[4:3]\*32 + 128) specifies the
bit offset of block_4 within the second source operand. (Imm[5]\*32+128) specifies
the initial bit offset of the block_3 within the first source operand. Each
16-bit result of eight SAD operations between block_4 and block_3 is written
to the respective word in the upper 128 bits of the destination operand.

The first source operand is a YMM register. The second source register can be
a YMM register or a 256-bit memory location. The destination operand is a YMM
register. Bits 7:6 of the immediate byte are ignored. Note: If VMPSADBW is encoded
with VEX.L= 1, an attempt to execute the instruction encoded with VEX.L= 1 will
cause an #UD exception.

Imm[4:3]\*32+128

   | |  
---- | -----
 255| 224| 192| 128
Src2 Abs. Diff. Imm[5]\*32+128

Src1 Sum

   | |  
---- | -----
 255| 144| 128
Destination

Imm[1:0]\*32

   | |  
---- | -----
 127| 96| 64| 0
Src2 Abs. Diff. Imm[2]\*32

Src1 Sum

   | |  
---- | -----
 127| 16| 0
Destination

   | |  
---- | -----
 Figure 3-27.| VMPSADBW Operation


### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)MPSADBW:| __m128i _mm_mpsadbw_epu8 (__m128i s1,
            | __m128i s2, const int mask);         
 VMPSADBW:  | __m256i _mm256_mpsadbw_epu8 (__m256i 
            | s1, __m256i s2, const int mask);     

### Flags Affected
None


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
