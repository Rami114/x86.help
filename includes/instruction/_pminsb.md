## PMINSB  -  Minimum of Packed Signed Byte Integers

> Operation
``` slim

IF (DEST[7:0] < SRC[7:0])
  THEN DEST[7:0] <- DEST[7:0];
  ELSE DEST[7:0] <- SRC[7:0]; FI;
IF (DEST[15:8] < SRC[15:8])
  THEN DEST[15:8] <- DEST[15:8];
  ELSE DEST[15:8] <- SRC[15:8]; FI;
IF (DEST[23:16] < SRC[23:16])
  THEN DEST[23:16] <- DEST[23:16];
  ELSE DEST[23:16] <- SRC[23:16]; FI;
IF (DEST[31:24] < SRC[31:24])
  THEN DEST[31:24] <- DEST[31:24];
  ELSE DEST[31:24] <- SRC[31:24]; FI;
IF (DEST[39:32] < SRC[39:32])
  THEN DEST[39:32] <- DEST[39:32];
  ELSE DEST[39:32] <- SRC[39:32]; FI;
IF (DEST[47:40] < SRC[47:40])
  THEN DEST[47:40] <- DEST[47:40];
  ELSE DEST[47:40] <- SRC[47:40]; FI;
IF (DEST[55:48] < SRC[55:48])
  THEN DEST[55:48] <- DEST[55:48];
  ELSE DEST[55:48] <- SRC[55:48]; FI;
IF (DEST[63:56] < SRC[63:56])
  THEN DEST[63:56] <- DEST[63:56];
  ELSE DEST[63:56] <- SRC[63:56]; FI;
IF (DEST[71:64] < SRC[71:64])
  THEN DEST[71:64] <- DEST[71:64];
  ELSE DEST[71:64] <- SRC[71:64]; FI;
IF (DEST[79:72] < SRC[79:72])
  THEN DEST[79:72] <- DEST[79:72];
  ELSE DEST[79:72] <- SRC[79:72]; FI;
IF (DEST[87:80] < SRC[87:80])
  THEN DEST[87:80] <- DEST[87:80];
  ELSE DEST[87:80] <- SRC[87:80]; FI;
IF (DEST[95:88] < SRC[95:88])
  THEN DEST[95:88] <- DEST[95:88];
  ELSE DEST[95:88] <- SRC[95:88]; FI;
IF (DEST[103:96] < SRC[103:96])
  THEN DEST[103:96] <- DEST[103:96];
  ELSE DEST[103:96] <- SRC[103:96]; FI;
IF (DEST[111:104] < SRC[111:104])
  THEN DEST[111:104] <- DEST[111:104];
  ELSE DEST[111:104] <- SRC[111:104]; FI;
IF (DEST[119:112] < SRC[119:112])
  THEN DEST[119:112] <- DEST[119:112];
  ELSE DEST[119:112] <- SRC[119:112]; FI;
IF (DEST[127:120] < SRC[127:120])
  THEN DEST[127:120] <- DEST[127:120];
  ELSE DEST[127:120] <- SRC[127:120]; FI;
VPMINSB (VEX.128 encoded version)
  IF SRC1[7:0] < SRC2[7:0] THEN
     DEST[7:0] <- SRC1[7:0];
  ELSE
     DEST[7:0] <- SRC2[7:0]; FI;
  (\* Repeat operation for 2nd through 15th bytes in source and destination operands \*)
  IF SRC1[127:120] < SRC2[127:120] THEN
     DEST[127:120] <- SRC1[127:120];
  ELSE
     DEST[127:120] <- SRC2[127:120]; FI;
DEST[VLMAX-1:128] <- 0
VPMINSB (VEX.256 encoded version)
  IF SRC1[7:0] < SRC2[7:0] THEN
     DEST[7:0] <- SRC1[7:0];
  ELSE
     DEST[15:0] <- SRC2[7:0]; FI;
  (\* Repeat operation for 2nd through 31st bytes in source and destination operands \*)
  IF SRC1[255:248] < SRC2[255:248] THEN
     DEST[255:248] <- SRC1[255:248];
  ELSE
     DEST[255:248] <- SRC2[255:248]; FI;

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 66 0F 38 38 /r PMINSB xmm1, xmm2/m128| RM   | V/V                   | SSE4_1            | Compare packed signed byte integers   
                                      |      |                       |                   | in xmm1 and xmm2/m128 and store packed
                                      |      |                       |                   | minimum values in xmm1.               
 VEX.NDS.128.66.0F38.WIG 38 /r VPMINSB| RVM  | V/V                   | AVX               | Compare packed signed byte integers   
 xmm1, xmm2, xmm3/m128                |      |                       |                   | in xmm2 and xmm3/m128 and store packed
                                      |      |                       |                   | minimum values in xmm1.               
 VEX.NDS.256.66.0F38.WIG 38 /r VPMINSB| RVM  | V/V                   | AVX2              | Compare packed signed byte integers   
 ymm1, ymm2, ymm3/m256                |      |                       |                   | in ymm2 and ymm3/m256 and store packed
                                      |      |                       |                   | minimum values in ymm1.               

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Compares packed signed byte integers in the destination operand (first operand)
and the source operand (second operand), and returns the minimum for each packed
value in the destination operand. 128-bit Legacy SSE version: The first source
and destination operands are XMM registers. The second source operand is an
XMM register or a 128-bit memory location. Bits (VLMAX-1:128) of the corresponding
YMM destination register remain unchanged. VEX.128 encoded version: The first
source and destination operands are XMM registers. The second source operand
is an XMM register or a 128-bit memory location. Bits (VLMAX-1:128) of the destination
YMM register are zeroed. VEX.256 encoded version: The second source operand
can be an YMM register or a 256-bit memory location. The first source and destination
operands are YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PMINSB:| __m128i _mm_min_epi8 ( __m128i a, __m128i
           | b);                                      
 VPMINSB:  | __m256i _mm256_min_epi8 ( __m256i a,     
           | __m256i b);                              

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
