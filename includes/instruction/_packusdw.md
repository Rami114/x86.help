## PACKUSDW  -  Pack with Unsigned Saturation

> Operation

``` slim
PACKUSDW (Legacy SSE instruction)
TMP[15:0] <- (DEST[31:0] < 0) ? 0 : DEST[15:0];
DEST[15:0] <- (DEST[31:0] > FFFFH) ? FFFFH : TMP[15:0] ;
TMP[31:16] <- (DEST[63:32] < 0) ? 0 : DEST[47:32];
DEST[31:16] <- (DEST[63:32] > FFFFH) ? FFFFH : TMP[31:16] ;
TMP[47:32] <- (DEST[95:64] < 0) ? 0 : DEST[79:64];
DEST[47:32] <- (DEST[95:64] > FFFFH) ? FFFFH : TMP[47:32] ;
TMP[63:48] <- (DEST[127:96] < 0) ? 0 : DEST[111:96];
DEST[63:48] <- (DEST[127:96] > FFFFH) ? FFFFH : TMP[63:48] ;
TMP[79:64] <- (SRC[31:0] < 0) ? 0 : SRC[15:0];
DEST[63:48] <- (SRC[31:0] > FFFFH) ? FFFFH : TMP[79:64] ;
TMP[95:80] <- (SRC[63:32] < 0) ? 0 : SRC[47:32];
DEST[95:80] <- (SRC[63:32] > FFFFH) ? FFFFH : TMP[95:80] ;
TMP[111:96] <- (SRC[95:64] < 0) ? 0 : SRC[79:64];
DEST[111:96] <- (SRC[95:64] > FFFFH) ? FFFFH : TMP[111:96] ;
TMP[127:112] <- (SRC[127:96] < 0) ? 0 : SRC[111:96];
DEST[127:112] <- (SRC[127:96] > FFFFH) ? FFFFH : TMP[127:112] ;
PACKUSDW (VEX.128 encoded version)
TMP[15:0] <- (SRC1[31:0] < 0) ? 0 : SRC1[15:0];
DEST[15:0] <- (SRC1[31:0] > FFFFH) ? FFFFH : TMP[15:0] ;
TMP[31:16] <- (SRC1[63:32] < 0) ? 0 : SRC1[47:32];
DEST[31:16] <- (SRC1[63:32] > FFFFH) ? FFFFH : TMP[31:16] ;
TMP[47:32] <- (SRC1[95:64] < 0) ? 0 : SRC1[79:64];
DEST[47:32] <- (SRC1[95:64] > FFFFH) ? FFFFH : TMP[47:32] ;
TMP[63:48] <- (SRC1[127:96] < 0) ? 0 : SRC1[111:96];
DEST[63:48] <- (SRC1[127:96] > FFFFH) ? FFFFH : TMP[63:48] ;
TMP[79:64] <- (SRC2[31:0] < 0) ? 0 : SRC2[15:0];
DEST[63:48] <- (SRC2[31:0] > FFFFH) ? FFFFH : TMP[79:64] ;
TMP[95:80] <- (SRC2[63:32] < 0) ? 0 : SRC2[47:32];
DEST[95:80] <- (SRC2[63:32] > FFFFH) ? FFFFH : TMP[95:80] ;
TMP[111:96] <- (SRC2[95:64] < 0) ? 0 : SRC2[79:64];
DEST[111:96] <- (SRC2[95:64] > FFFFH) ? FFFFH : TMP[111:96] ;
TMP[127:112] <- (SRC2[127:96] < 0) ? 0 : SRC2[111:96];
DEST[127:112] <- (SRC2[127:96] > FFFFH) ? FFFFH : TMP[127:112];
DEST[VLMAX-1:128] <- 0;
VPACKUSDW (VEX.256 encoded version)
TMP[15:0] <- (SRC1[31:0] < 0) ? 0 : SRC1[15:0];
DEST[15:0] <- (SRC1[31:0] > FFFFH) ? FFFFH : TMP[15:0] ;
TMP[31:16] <- (SRC1[63:32] < 0) ? 0 : SRC1[47:32];
DEST[31:16] <- (SRC1[63:32] > FFFFH) ? FFFFH : TMP[31:16] ;
TMP[47:32] <- (SRC1[95:64] < 0) ? 0 : SRC1[79:64];
DEST[47:32] <- (SRC1[95:64] > FFFFH) ? FFFFH : TMP[47:32] ;
TMP[63:48] <- (SRC1[127:96] < 0) ? 0 : SRC1[111:96];
DEST[63:48] <- (SRC1[127:96] > FFFFH) ? FFFFH : TMP[63:48] ;
TMP[79:64] <- (SRC2[31:0] < 0) ? 0 : SRC2[15:0];
DEST[63:48] <- (SRC2[31:0] > FFFFH) ? FFFFH : TMP[79:64] ;
TMP[95:80] <- (SRC2[63:32] < 0) ? 0 : SRC2[47:32];
DEST[95:80] <- (SRC2[63:32] > FFFFH) ? FFFFH : TMP[95:80] ;
TMP[111:96] <- (SRC2[95:64] < 0) ? 0 : SRC2[79:64];
DEST[111:96] <- (SRC2[95:64] > FFFFH) ? FFFFH : TMP[111:96] ;
TMP[127:112] <- (SRC2[127:96] < 0) ? 0 : SRC2[111:96];
DEST[128:112] <- (SRC2[127:96] > FFFFH) ? FFFFH : TMP[127:112] ;
TMP[143:128] <- (SRC1[159:128] < 0) ? 0 : SRC1[143:128];
DEST[143:128] <- (SRC1[159:128] > FFFFH) ? FFFFH : TMP[143:128] ;
TMP[159:144] <- (SRC1[191:160] < 0) ? 0 : SRC1[175:160];
DEST[159:144] <- (SRC1[191:160] > FFFFH) ? FFFFH : TMP[159:144] ;
TMP[175:160] <- (SRC1[223:192] < 0) ? 0 : SRC1[207:192];
DEST[175:160] <- (SRC1[223:192] > FFFFH) ? FFFFH : TMP[175:160] ;
TMP[191:176] <- (SRC1[255:224] < 0) ? 0 : SRC1[239:224];
DEST[191:176] <- (SRC1[255:224] > FFFFH) ? FFFFH : TMP[191:176] ;
TMP[207:192] <- (SRC2[159:128] < 0) ? 0 : SRC2[143:128];
DEST[207:192] <- (SRC2[159:128] > FFFFH) ? FFFFH : TMP[207:192] ;
TMP[223:208] <- (SRC2[191:160] < 0) ? 0 : SRC2[175:160];
DEST[223:208] <- (SRC2[191:160] > FFFFH) ? FFFFH : TMP[223:208] ;
TMP[239:224] <- (SRC2[223:192] < 0) ? 0 : SRC2[207:192];
DEST[239:224] <- (SRC2[223:192] > FFFFH) ? FFFFH : TMP[239:224] ;
TMP[255:240] <- (SRC2[255:224] < 0) ? 0 : SRC2[239:224];
DEST[255:240] <- (SRC2[255:224] > FFFFH) ? FFFFH : TMP[255:240] ;

```

 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 66 0F 38 2B /r PACKUSDW xmm1, xmm2/m128| RM   | V/V                   | SSE4_1            | Convert 4 packed signed doubleword integers
                                        |      |                       |                   | from xmm1 and 4 packed signed doubleword   
                                        |      |                       |                   | integers from xmm2/m128 into 8 packed      
                                        |      |                       |                   | unsigned word integers in xmm1 using       
                                        |      |                       |                   | unsigned saturation.                       
 VEX.NDS.128.66.0F38.WIG 2B /r VPACKUSDW| RVM  | V/V                   | AVX               | Convert 4 packed signed doubleword integers
 xmm1, xmm2, xmm3/m128                  |      |                       |                   | from xmm2 and 4 packed signed doubleword   
                                        |      |                       |                   | integers from xmm3/m128 into 8 packed      
                                        |      |                       |                   | unsigned word integers in xmm1 using       
                                        |      |                       |                   | unsigned saturation.                       
 VEX.NDS.256.66.0F38.WIG 2B /r VPACKUSDW| RVM  | V/V                   | AVX2              | Convert 8 packed signed doubleword integers
 ymm1, ymm2, ymm3/m256                  |      |                       |                   | from ymm2 and 8 packed signed doubleword   
                                        |      |                       |                   | integers from ymm3/m128 into 16 packed     
                                        |      |                       |                   | unsigned word integers in ymm1 using       
                                        |      |                       |                   | unsigned saturation.                       

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Converts packed signed doubleword integers into packed unsigned word integers
using unsigned saturation to

   | |  
---- | -----
 handle overflow conditions. greater          | If the signed doubleword value is beyond
 than FFFFH or less than 0000H), the          | the range of an unsigned word (that     
 saturated unsigned word integer value        | is,                                     
 of FFFFH or 0000H, respectively, is          |                                         
 stored in the destination. 128-bit Legacy    |                                         
 SSE version: The first source operand        |                                         
 is an XMM register. The second operand       |                                         
 can be an XMM register or a 128-bit          |                                         
 memory location. The destination is          |                                         
 not distinct from the first source XMM       |                                         
 register and the upper bits (VLMAX-1:128)    |                                         
 of the corresponding YMM register destination|                                         
 are unmodified. VEX.128 encoded version:     |                                         
 The first source operand is an XMM register. |                                         
 The second source operand is an XMM          |                                         
 register or 128-bit memory location.         |                                         
 The destination operand is an XMM register.  |                                         
 The upper bits (VLMAX-1:128) of the          |                                         
 corresponding YMM register destination       |                                         
 are zeroed. VEX.256 encoded version:         |                                         
 The first source operand is a YMM register.  |                                         
 The second source operand is a YMM register  |                                         
 or a 256-bit memory location. The destination|                                         
 operand is a YMM register. Note: VEX.L       |                                         
 ---  | ---
 must be 0, otherwise the instruction         |                                         
 will **``#UD.``**                                    |                                         


### Intel C/C++ Compiler Intrinsic Equivalent
(V)PACKUSDW: __m128i _mm_packus_epi32(__m128i m1, __m128i m2);

   | |  
---- | -----
 VPACKUSDW:| __m256i _mm256_packus_epi32(__m256i
           | m1, __m256i m2);                   

### Flags Affected
None.


### SIMD Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
