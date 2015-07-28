## PSHUFD - Shuffle Packed Doublewords

> Operation
``` slim

PSHUFD (128-bit Legacy SSE version)
DEST[31:0] <- (SRC >> (ORDER[1:0] \* 32))[31:0];
DEST[63:32] <- (SRC >> (ORDER[3:2] \* 32))[31:0];
DEST[95:64] <- (SRC >> (ORDER[5:4] \* 32))[31:0];
DEST[127:96] <- (SRC >> (ORDER[7:6] \* 32))[31:0];
DEST[VLMAX-1:128] (Unmodified)
VPSHUFD (VEX.128 encoded version)
DEST[31:0] <- (SRC >> (ORDER[1:0] \* 32))[31:0];
DEST[63:32] <- (SRC >> (ORDER[3:2] \* 32))[31:0];
DEST[95:64] <- (SRC >> (ORDER[5:4] \* 32))[31:0];
DEST[127:96] <- (SRC >> (ORDER[7:6] \* 32))[31:0];
DEST[VLMAX-1:128] <- 0
VPSHUFD (VEX.256 encoded version)
DEST[31:0] <- (SRC[127:0] >> (ORDER[1:0] \* 32))[31:0];
DEST[63:32] <- (SRC[127:0] >> (ORDER[3:2] \* 32))[31:0];
DEST[95:64] <- (SRC[127:0] >> (ORDER[5:4] \* 32))[31:0];
DEST[127:96] <- (SRC[127:0] >> (ORDER[7:6] \* 32))[31:0];
DEST[159:128] <- (SRC[255:128] >> (ORDER[1:0] \* 32))[31:0];
DEST[191:160] <- (SRC[255:128] >> (ORDER[3:2] \* 32))[31:0];
DEST[223:192] <- (SRC[255:128] >> (ORDER[5:4] \* 32))[31:0];
DEST[255:224] <- (SRC[255:128] >> (ORDER[7:6] \* 32))[31:0];

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 66 0F 70 /r ib PSHUFD xmm1, xmm2/m128,  | RMI  | V/V                   | SSE2              | Shuffle the doublewords in xmm2/m128   
 imm8                                    |      |                       |                   | based on the encoding in imm8 and store
                                         |      |                       |                   | the result in xmm1.                    
 VEX.128.66.0F.WIG 70 /r ib VPSHUFD xmm1,| RMI  | V/V                   | AVX               | Shuffle the doublewords in xmm2/m128   
 xmm2/m128, imm8                         |      |                       |                   | based on the encoding in imm8 and store
                                         |      |                       |                   | the result in xmm1.                    
 VEX.256.66.0F.WIG 70 /r ib VPSHUFD ymm1,| RMI  | V/V                   | AVX2              | Shuffle the doublewords in ymm2/m256   
 ymm2/m256, imm8                         |      |                       |                   | based on the encoding in imm8 and store
                                         |      |                       |                   | the result in ymm1.                    

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8     | NA       

### Description
Copies doublewords from source operand (second operand) and inserts them in
the destination operand (first operand) at the locations selected with the order
operand (third operand). Figure 4-12 shows the operation of the 256-bit VPSHUFD
instruction and the encoding of the order operand. Each 2-bit field in the order
operand selects the contents of one doubleword location within a 128-bit lane
and copy to the target element in the destination operand. For example, bits
0 and 1 of the order operand targets the first doubleword element in the low
and high 128-bit lane of the destination operand for 256-bit VPSHUFD. The encoded
value of bits 1:0 of the order operand (see the field encoding in Figure 4-12)
determines which doubleword element (from the respective 128-bit lane) of the
source operand will be copied to doubleword 0 of the destination operand. For
128-bit operation, only the low 128-bit lane are operative. The source operand
can be an XMM register or a 128-bit memory location. The destination operand
is an XMM register. The order operand is an 8-bit immediate. Note that this
instruction permits a doubleword in the source operand to be copied to more
than one doubleword location in the destination operand.

   | |  
---- | -----
 SRC Encoding of Fields in Operand| X7 Y7 ORDER Figure 4-12.| X6 Y6 00B - X4 01B - X5 10B - X6 11B| X5 Y5 ORDER 256-bit VPSHUFD Instruction| X4 Y4 4 3| X3 Y3 0| X2 Y2 Encoding of Fields in ORDER Operand| X1 Y1 00B - X0 01B - X1 10B - X2 11B| X0 Y0
                                  |                         | - X7                                | Operation                              |          |        |                                          | - X3                                |      
The source operand can be an XMM register or a 128-bit memory location. The
destination operand is an XMM register. The order operand is an 8-bit immediate.
<aside class="notification">
Note that this instruction permits a doubleword in the source operand to be
copied to more than one doubleword location in the destination operand.
</aside>

Legacy SSE instructions: In 64-bit mode using a REX prefix in the form of REX.R
permits this instruction to access additional registers (XMM8-XMM15). 128-bit
Legacy SSE version: Bits (VLMAX-1:128) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: Bits (VLMAX-1:128) of the
destination YMM register are zeroed. VEX.256 encoded version: Bits (255:128)
of the destination stores the shuffled results of the upper 16 bytes of the
source operand using the immediate byte as the order operand.

<aside class="notification">
VEX.vvvv is reserved and must be 1111b, VEX.L must be 0, otherwise the
instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PSHUFD:| __m128i _mm_shuffle_epi32(__m128i a,
           | int n)                              
 VPSHUFD:  | __m256i _mm256_shuffle_epi32(__m256i
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
