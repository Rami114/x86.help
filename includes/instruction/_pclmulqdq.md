## PCLMULQDQ - Carry-Less Multiplication Quadword

> Operation

``` slim
PCLMULQDQ
IF (Imm8[0] = 0 )
  THEN
     TEMP1 <- SRC1 [63:0];
  ELSE
     TEMP1 <- SRC1 [127:64];
FI
IF (Imm8[4] = 0 )
  THEN
     TEMP2 <- SRC2 [63:0];
  ELSE
     TEMP2 <- SRC2 [127:64];
FI
For i = 0 to 63 {
  TmpB [ i ] <- (TEMP1[ 0 ] and TEMP2[ i ]);
  For j = 1 to i {
     TmpB [ i ] <- TmpB [ i ] xor (TEMP1[ j ] and TEMP2[ i - j ])
  }
  DEST[ i ] <- TmpB[ i ];
}
For i = 64 to 126 {
  TmpB [ i ] <- 0;
  For j = i - 63 to 63 {
     TmpB [ i ] <- TmpB [ i ] xor (TEMP1[ j ] and TEMP2[ i - j ])
  }
  DEST[ i ] <- TmpB[ i ];
}
DEST[127] <- 0;
DEST[VLMAX-1:128] (Unmodified)
VPCLMULQDQ
IF (Imm8[0] = 0 )
  THEN
     TEMP1 <- SRC1 [63:0];
  ELSE
     TEMP1 <- SRC1 [127:64];
FI
IF (Imm8[4] = 0 )
  THEN
     TEMP2 <- SRC2 [63:0];
  ELSE
     TEMP2 <- SRC2 [127:64];
FI
For i = 0 to 63 {
  TmpB [ i ] <- (TEMP1[ 0 ] and TEMP2[ i ]);
  For j = 1 to i {
     TmpB [i] <- TmpB [i] xor (TEMP1[ j ] and TEMP2[ i - j ])
  }
  DEST[i] <- TmpB[i];
}
For i = 64 to 126 {
  TmpB [ i ] <- 0;
  For j = i - 63 to 63 {
     TmpB [i] <- TmpB [i] xor (TEMP1[ j ] and TEMP2[ i - j ])
  }
  DEST[i] <- TmpB[i];
}
DEST[VLMAX-1:127] <- 0;

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 (V)PCLMULQDQ:| __m128i| _mm_clmulepi64_si128 (__m128i, __m128i,
              |        | const int)                             

```

 Opcode/Instruction                          | Op/En| 64/32 bit Mode Support| CPUID Feature Flag      | Description                              
 ---  | --- | --- | --- | ---
 66 0F 3A 44 /r ib PCLMULQDQ xmm1, xmm2/m128,| RMI  | V/V                   | CLMUL                   | Carry-less multiplication of one quadword
 imm8                                        |      |                       |                         | of xmm1 by one quadword of xmm2/m128,    
                                             |      |                       |                         | stores the 128-bit result in xmm1. The   
                                             |      |                       |                         | immediate is used to determine which     
                                             |      |                       |                         | quadwords of xmm1 and xmm2/m128 should   
                                             |      |                       |                         | be used.                                 
 VEX.NDS.128.66.0F3A.WIG 44 /r ib VPCLMULQDQ | RVMI | V/V                   | Both CLMUL and AVX flags| Carry-less multiplication of one quadword
 xmm1, xmm2, xmm3/m128, imm8                 |      |                       |                         | of xmm2 by one quadword of xmm3/m128,    
                                             |      |                       |                         | stores the 128-bit result in xmm1. The   
                                             |      |                       |                         | immediate is used to determine which     
                                             |      |                       |                         | quadwords of xmm2 and xmm3/m128 should   
                                             |      |                       |                         | be used.                                 

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand2     | Operand3     | Operand4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA      
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8    

### Description
Performs a carry-less multiplication of two quadwords, selected from the first
source and second source operand according to the value of the immediate byte.
Bits 4 and 0 are used to select which 64-bit half of each operand to use according
to Table 4-10, other bits of the immediate byte are ignored.


### Table 4-10. PCLMULQDQ Quadword Selection of Immediate Byte
   | |  
---- | -----
 Imm[4]| Imm[0]CL_MUL( SRC21[63:0], SRC1[63:0]     | PCLMULQDQ Operation 0 0 1 1
       | ) CL_MUL( SRC2[63:0], SRC1[127:64] )      |                            
       | CL_MUL( SRC2[127:64], SRC1[63:0] ) CL_MUL(|                            
       | SRC2[127:64], SRC1[127:64] )              |                            
<aside class="notification">
1. SRC2 denotes the second source operand, which can be a register or
memory; SRC1 denotes the first source and destination operand.
</aside>

The first source operand and the destination operand are the same and must be
an XMM register. The second source operand can be an XMM register or a 128-bit
memory location. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged.

Compilers and assemblers may implement the following pseudo-op syntax to simply
programming and emit the required encoding for Imm8.


### Table 4-11. Pseudo-Op and PCLMULQDQ Implementation
   | |  
---- | -----
 Pseudo-Op              | Imm8 Encoding
 PCLMULLQLQDQ xmm1, xmm2| 0000_0000B   
 PCLMULHQLQDQ xmm1, xmm2| 0000_0001B   
 PCLMULLQHDQ xmm1, xmm2 | 0001_0000B   
 PCLMULHQHDQ xmm1, xmm2 | 0001_0001B   


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4.
