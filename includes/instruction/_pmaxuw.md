## PMAXUW  -  Maximum of Packed Word Integers

> Operation

``` slim
IF (DEST[15:0] > SRC[15:0])
  THEN DEST[15:0] <- DEST[15:0];
  ELSE DEST[15:0] <- SRC[15:0]; FI;
IF (DEST[31:16] > SRC[31:16])
  THEN DEST[31:16] <- DEST[31:16];
  ELSE DEST[31:16] <- SRC[31:16]; FI;
IF (DEST[47:32] > SRC[47:32])
  THEN DEST[47:32] <- DEST[47:32];
  ELSE DEST[47:32] <- SRC[47:32]; FI;
IF (DEST[63:48] > SRC[63:48])
  THEN DEST[63:48] <- DEST[63:48];
  ELSE DEST[63:48] <- SRC[63:48]; FI;
IF (DEST[79:64] > SRC[79:64])
  THEN DEST[79:64] <- DEST[79:64];
  ELSE DEST[79:64] <- SRC[79:64]; FI;
IF (DEST[95:80] > SRC[95:80])
  THEN DEST[95:80] <- DEST[95:80];
  ELSE DEST[95:80] <- SRC[95:80]; FI;
IF (DEST[111:96] > SRC[111:96])
  THEN DEST[111:96] <- DEST[111:96];
  ELSE DEST[111:96] <- SRC[111:96]; FI;
IF (DEST[127:112] > SRC[127:112])
  THEN DEST[127:112] <- DEST[127:112];
  ELSE DEST[127:112] <- SRC[127:112]; FI;
VPMAXUW (VEX.128 encoded version)
  IF SRC1[15:0] > SRC2[15:0] THEN
     DEST[15:0] <- SRC1[15:0];
  ELSE
     DEST[15:0] <- SRC2[15:0]; FI;
  (\* Repeat operation for 2nd through 7th words in source and destination operands \*)
  IF SRC1[127:112] >SRC2[127:112] THEN
     DEST[127:112] <- SRC1[127:112];
  ELSE
     DEST[127:112] <- SRC2[127:112]; FI;
DEST[VLMAX-1:128] <- 0
VPMAXUW (VEX.256 encoded version)
  IF SRC1[15:0] > SRC2[15:0] THEN
     DEST[15:0] <- SRC1[15:0];
  ELSE
     DEST[15:0] <- SRC2[15:0]; FI;
  (\* Repeat operation for 2nd through 15th words in source and destination operands \*)
  IF SRC1[255:240] >SRC2[255:240] THEN
     DEST[255:240] <- SRC1[255:240];
  ELSE
     DEST[255:240] <- SRC2[255:240]; FI;

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 (V)PMAXUW:| __m128i _mm_max_epu16 ( __m128i a, __m128i
           | b);                                       
 VPMAXUW:  | __m256i _mm256_max_epu16 ( __m256i a,     
           | __m256i b)                                

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 66 0F 38 3E /r PMAXUW xmm1, xmm2/m128| RM   | V/V                   | SSE4_1            | Compare packed unsigned word integers  
                                      |      |                       |                   | in xmm1 and xmm2/m128 and store packed 
                                      |      |                       |                   | maximum values in xmm1.                
 VEX.NDS.128.66.0F38.WIG 3E/r VPMAXUW | RVM  | V/V                   | AVX               | Compare packed unsigned word integers  
 xmm1, xmm2, xmm3/m128                |      |                       |                   | in xmm3/m128 and xmm2 and store maximum
                                      |      |                       |                   | packed values in xmm1.                 
 VEX.NDS.256.66.0F38.WIG 3E /r VPMAXUW| RVM  | V/V                   | AVX2              | Compare packed unsigned word integers  
 ymm1, ymm2, ymm3/m256                |      |                       |                   | in ymm3/m256 and ymm2 and store maximum
                                      |      |                       |                   | packed values in ymm1.                 

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Compares packed unsigned word integers in the destination operand (first operand)
and the source operand (second operand), and returns the maximum for each packed
value in the destination operand. 128-bit Legacy SSE version: The first source
and destination operands are XMM registers. The second source operand is an
XMM register or a 128-bit memory location. Bits (VLMAX-1:128) of the corresponding
YMM destination register remain unchanged. VEX.128 encoded version: The first
source and destination operands are XMM registers. The second source operand
is an XMM register or a 128-bit memory location. Bits (VLMAX-1:128) of the destination
YMM register are zeroed.

VEX.256 encoded version: The second source operand can be an YMM register or
a 256-bit memory location. The first source and destination operands are YMM
registers.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
