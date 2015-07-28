## PMAXUD  -  Maximum of Packed Unsigned Dword Integers

> Operation

``` slim
IF (DEST[31:0] > SRC[31:0])
  THEN DEST[31:0] <- DEST[31:0];
  ELSE DEST[31:0] <- SRC[31:0]; FI;
IF (DEST[63:32] > SRC[63:32])
  THEN DEST[63:32] <- DEST[63:32];
  ELSE DEST[63:32] <- SRC[63:32]; FI;
IF (DEST[95:64] > SRC[95:64])
  THEN DEST[95:64] <- DEST[95:64];
  ELSE DEST[95:64] <- SRC[95:64]; FI;
IF (DEST[127:96] > SRC[127:96])
  THEN DEST[127:96] <- DEST[127:96];
  ELSE DEST[127:96] <- SRC[127:96]; FI;
VPMAXUD (VEX.128 encoded version)
  IF SRC1[31:0] > SRC2[31:0] THEN
     DEST[31:0] <- SRC1[31:0];
  ELSE
     DEST[31:0] <- SRC2[31:0]; FI;
  (\* Repeat operation for 2nd through 3rd dwords in source and destination operands \*)
  IF SRC1[127:95] > SRC2[127:95] THEN
     DEST[127:95] <- SRC1[127:95];
  ELSE
     DEST[127:95] <- SRC2[127:95]; FI;
DEST[VLMAX-1:128] <- 0
VPMAXUD (VEX.256 encoded version)
  IF SRC1[31:0] > SRC2[31:0] THEN
     DEST[31:0] <- SRC1[31:0];
  ELSE
     DEST[31:0] <- SRC2[31:0]; FI;
  (\* Repeat operation for 2nd through 7th dwords in source and destination operands \*)
  IF SRC1[255:224] > SRC2[255:224] THEN
     DEST[255:224] <- SRC1[255:224];
  ELSE
     DEST[255:224] <- SRC2[255:224]; FI;

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 66 0F 38 3F /r PMAXUD xmm1, xmm2/m128| RM   | V/V                   | SSE4_1            | Compare packed unsigned dword integers
                                      |      |                       |                   | in xmm1 and xmm2/m128 and store packed
                                      |      |                       |                   | maximum values in xmm1.               
 VEX.NDS.128.66.0F38.WIG 3F /r VPMAXUD| RVM  | V/V                   | AVX               | Compare packed unsigned dword integers
 xmm1, xmm2, xmm3/m128                |      |                       |                   | in xmm2 and xmm3/m128 and store packed
                                      |      |                       |                   | maximum values in xmm1.               
 VEX.NDS.256.66.0F38.WIG 3F /r VPMAXUD| RVM  | V/V                   | AVX2              | Compare packed unsigned dword integers
 ymm1, ymm2, ymm3/m256                |      |                       |                   | in ymm2 and ymm3/m256 and store packed
                                      |      |                       |                   | maximum values in ymm1.               

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Compares packed unsigned dword integers in the destination operand (first operand)
and the source operand (second operand), and returns the maximum for each packed
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
 (V)PMAXUD:| __m128i _mm_max_epu32 ( __m128i a, __m128i
           | b);                                       
 VPMAXUD:  | __m256i _mm256_max_epu32 ( __m256i a,     
           | __m256i b);                               

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
