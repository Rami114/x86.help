## PACKSSWB/PACKSSDW - Pack with Signed Saturation

> Operation

``` slim
PACKSSWB (with 64-bit operands)
  DEST[7:0] <- SaturateSignedWordToSignedByte DEST[15:0];
  DEST[15:8] <- SaturateSignedWordToSignedByte DEST[31:16];
  DEST[23:16] <- SaturateSignedWordToSignedByte DEST[47:32];
  DEST[31:24] <- SaturateSignedWordToSignedByte DEST[63:48];
  DEST[39:32] <- SaturateSignedWordToSignedByte SRC[15:0];
  DEST[47:40] <- SaturateSignedWordToSignedByte SRC[31:16];
  DEST[55:48] <- SaturateSignedWordToSignedByte SRC[47:32];
  DEST[63:56] <- SaturateSignedWordToSignedByte SRC[63:48];
PACKSSDW (with 64-bit operands)
  DEST[15:0] <- SaturateSignedDoublewordToSignedWord DEST[31:0];
  DEST[31:16] <- SaturateSignedDoublewordToSignedWord DEST[63:32];
  DEST[47:32] <- SaturateSignedDoublewordToSignedWord SRC[31:0];
  DEST[63:48] <- SaturateSignedDoublewordToSignedWord SRC[63:32];
PACKSSWB instruction (128-bit Legacy SSE version)
  DEST[7:0]<- SaturateSignedWordToSignedByte (DEST[15:0]);
  DEST[15:8] <- SaturateSignedWordToSignedByte (DEST[31:16]);
  DEST[23:16] <- SaturateSignedWordToSignedByte (DEST[47:32]);
  DEST[31:24] <- SaturateSignedWordToSignedByte (DEST[63:48]);
  DEST[39:32] <- SaturateSignedWordToSignedByte (DEST[79:64]);
  DEST[47:40] <-SaturateSignedWordToSignedByte (DEST[95:80]);
  DEST[55:48] <- SaturateSignedWordToSignedByte (DEST[111:96]);
  DEST[63:56] <- SaturateSignedWordToSignedByte (DEST[127:112]);
  DEST[71:64] <- SaturateSignedWordToSignedByte (SRC[15:0]);
  DEST[79:72] <- SaturateSignedWordToSignedByte (SRC[31:16]);
  DEST[87:80] <- SaturateSignedWordToSignedByte (SRC[47:32]);
  DEST[95:88] <- SaturateSignedWordToSignedByte (SRC[63:48]);
  DEST[103:96] <- SaturateSignedWordToSignedByte (SRC[79:64]);
  DEST[111:104] <- SaturateSignedWordToSignedByte (SRC[95:80]);
  DEST[119:112] <- SaturateSignedWordToSignedByte (SRC[111:96]);
  DEST[127:120] <- SaturateSignedWordToSignedByte (SRC[127:112]);
PACKSSDW instruction (128-bit Legacy SSE version)
  DEST[15:0] <- SaturateSignedDwordToSignedWord (DEST[31:0]);
  DEST[31:16] <- SaturateSignedDwordToSignedWord (DEST[63:32]);
  DEST[47:32] <- SaturateSignedDwordToSignedWord (DEST[95:64]);
  DEST[63:48] <- SaturateSignedDwordToSignedWord (DEST[127:96]);
  DEST[79:64] <- SaturateSignedDwordToSignedWord (SRC[31:0]);
  DEST[95:80] <- SaturateSignedDwordToSignedWord (SRC[63:32]);
  DEST[111:96] <- SaturateSignedDwordToSignedWord (SRC[95:64]);
  DEST[127:112] <- SaturateSignedDwordToSignedWord (SRC[127:96]);
VPACKSSWB instruction (VEX.128 encoded version)
  DEST[7:0]<- SaturateSignedWordToSignedByte (SRC1[15:0]);
  DEST[15:8] <- SaturateSignedWordToSignedByte (SRC1[31:16]);
  DEST[23:16] <- SaturateSignedWordToSignedByte (SRC1[47:32]);
  DEST[31:24] <- SaturateSignedWordToSignedByte (SRC1[63:48]);
  DEST[39:32] <- SaturateSignedWordToSignedByte (SRC1[79:64]);
  DEST[47:40] <- SaturateSignedWordToSignedByte (SRC1[95:80]);
  DEST[55:48] <- SaturateSignedWordToSignedByte (SRC1[111:96]);
  DEST[63:56] <- SaturateSignedWordToSignedByte (SRC1[127:112]);
  DEST[71:64] <- SaturateSignedWordToSignedByte (SRC2[15:0]);
  DEST[79:72] <- SaturateSignedWordToSignedByte (SRC2[31:16]);
  DEST[87:80] <- SaturateSignedWordToSignedByte (SRC2[47:32]);
  DEST[95:88] <- SaturateSignedWordToSignedByte (SRC2[63:48]);
  DEST[103:96] <- SaturateSignedWordToSignedByte (SRC2[79:64]);
  DEST[111:104] <- SaturateSignedWordToSignedByte (SRC2[95:80]);
  DEST[119:112] <- SaturateSignedWordToSignedByte (SRC2[111:96]);
  DEST[127:120] <- SaturateSignedWordToSignedByte (SRC2[127:112]);
  DEST[VLMAX-1:128]<- 0;
VPACKSSDW instruction (VEX.128 encoded version)
  DEST[15:0] <- SaturateSignedDwordToSignedWord (SRC1[31:0]);
  DEST[31:16] <- SaturateSignedDwordToSignedWord (SRC1[63:32]);
  DEST[47:32] <- SaturateSignedDwordToSignedWord (SRC1[95:64]);
  DEST[63:48] <- SaturateSignedDwordToSignedWord (SRC1[127:96]);
  DEST[79:64] <- SaturateSignedDwordToSignedWord (SRC2[31:0]);
  DEST[95:80] <- SaturateSignedDwordToSignedWord (SRC2[63:32]);
  DEST[111:96] <- SaturateSignedDwordToSignedWord (SRC2[95:64]);
  DEST[127:112] <- SaturateSignedDwordToSignedWord (SRC2[127:96]);
  DEST[VLMAX-1:128]<- 0;
VPACKSSWB instruction (VEX.256 encoded version)
  DEST[7:0]<- SaturateSignedWordToSignedByte (SRC1[15:0]);
  DEST[15:8] <- SaturateSignedWordToSignedByte (SRC1[31:16]);
  DEST[23:16] <- SaturateSignedWordToSignedByte (SRC1[47:32]);
  DEST[31:24] <- SaturateSignedWordToSignedByte (SRC1[63:48]);
  DEST[39:32] <- SaturateSignedWordToSignedByte (SRC1[79:64]);
  DEST[47:40] <- SaturateSignedWordToSignedByte (SRC1[95:80]);
  DEST[55:48] <- SaturateSignedWordToSignedByte (SRC1[111:96]);
  DEST[63:56] <- SaturateSignedWordToSignedByte (SRC1[127:112]);
  DEST[71:64] <- SaturateSignedWordToSignedByte (SRC2[15:0]);
  DEST[79:72] <- SaturateSignedWordToSignedByte (SRC2[31:16]);
  DEST[87:80] <- SaturateSignedWordToSignedByte (SRC2[47:32]);
  DEST[95:88] <- SaturateSignedWordToSignedByte (SRC2[63:48]);
  DEST[103:96] <- SaturateSignedWordToSignedByte (SRC2[79:64]);
  DEST[111:104] <- SaturateSignedWordToSignedByte (SRC2[95:80]);
  DEST[119:112] <- SaturateSignedWordToSignedByte (SRC2[111:96]);
  DEST[127:120] <- SaturateSignedWordToSignedByte (SRC2[127:112]);
  DEST[135:128]<- SaturateSignedWordToSignedByte (SRC1[143:128]);
  DEST[143:136] <- SaturateSignedWordToSignedByte (SRC1[159:144]);
  DEST[151:144] <- SaturateSignedWordToSignedByte (SRC1[175:160]);
  DEST[159:152] <- SaturateSignedWordToSignedByte (SRC1[191:176]);
  DEST[167:160] <- SaturateSignedWordToSignedByte (SRC1[207:192]);
  DEST[175:168] <- SaturateSignedWordToSignedByte (SRC1[223:208]);
  DEST[183:176] <- SaturateSignedWordToSignedByte (SRC1[239:224]);
  DEST[191:184] <- SaturateSignedWordToSignedByte (SRC1[255:240]);
  DEST[199:192] <- SaturateSignedWordToSignedByte (SRC2[143:128]);
  DEST[207:200] <- SaturateSignedWordToSignedByte (SRC2[159:144]);
  DEST[215:208] <- SaturateSignedWordToSignedByte (SRC2[175:160]);
  DEST[223:216] <- SaturateSignedWordToSignedByte (SRC2[191:176]);
  DEST[231:224] <- SaturateSignedWordToSignedByte (SRC2[207:192]);
  DEST[239:232] <- SaturateSignedWordToSignedByte (SRC2[223:208]);
  DEST[247:240] <- SaturateSignedWordToSignedByte (SRC2[239:224]);
  DEST[255:248] <- SaturateSignedWordToSignedByte (SRC2[255:240]);
VPACKSSDW instruction (VEX.256 encoded version)
  DEST[15:0] <- SaturateSignedDwordToSignedWord (SRC1[31:0]);
  DEST[31:16] <- SaturateSignedDwordToSignedWord (SRC1[63:32]);
  DEST[47:32] <- SaturateSignedDwordToSignedWord (SRC1[95:64]);
  DEST[63:48] <- SaturateSignedDwordToSignedWord (SRC1[127:96]);
  DEST[79:64] <- SaturateSignedDwordToSignedWord (SRC2[31:0]);
  DEST[95:80] <- SaturateSignedDwordToSignedWord (SRC2[63:32]);
  DEST[111:96] <- SaturateSignedDwordToSignedWord (SRC2[95:64]);
  DEST[127:112] <- SaturateSignedDwordToSignedWord (SRC2[127:96]);
  DEST[143:128] <- SaturateSignedDwordToSignedWord (SRC1[159:128]);
  DEST[159:144] <- SaturateSignedDwordToSignedWord (SRC1[191:160]);
  DEST[175:160] <- SaturateSignedDwordToSignedWord (SRC1[223:192]);
  DEST[191:176] <- SaturateSignedDwordToSignedWord (SRC1[255:224]);
  DEST[207:192] <- SaturateSignedDwordToSignedWord (SRC2[159:128]);
  DEST[223:208] <- SaturateSignedDwordToSignedWord (SRC2[191:160]);
  DEST[239:224] <- SaturateSignedDwordToSignedWord (SRC2[223:192]);
  DEST[255:240] <- SaturateSignedDwordToSignedWord (SRC2[255:224]);

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 PACKSSWB:   | __m64 _mm_packs_pi16(__m64 m1, __m64  
             | m2)                                   
 (V)PACKSSWB:| __m128i _mm_packs_epi16(__m128i m1,   
             | __m128i m2)                           
 VPACKSSWB:  | __m256i _mm256_packs_epi16(__m256i m1,
             | __m256i m2)                           
 PACKSSDW:   | __m64 _mm_packs_pi32 (__m64 m1, __m64 
             | m2)                                   
 (V)PACKSSDW:| __m128i _mm_packs_epi32(__m128i m1,   
             | __m128i m2)                           
 VPACKSSDW:  | __m256i _mm256_packs_epi32(__m256i m1,
             | __m256i m2)                           

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F 63 /r1 PACKSSWB mm1, mm2/m64      | RM   | V/V                   | MMX               | Converts 4 packed signed word integers  
                                      |      |                       |                   | from mm1 and from mm2/m64 into 8 packed 
                                      |      |                       |                   | signed byte integers in mm1 using signed
                                      |      |                       |                   | saturation.                             
 66 0F 63 /r PACKSSWB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Converts 8 packed signed word integers  
                                      |      |                       |                   | from xmm1 and from xxm2/m128 into 16    
                                      |      |                       |                   | packed signed byte integers in xxm1     
                                      |      |                       |                   | using signed saturation.                
 0F 6B /r1 PACKSSDW mm1, mm2/m64      | RM   | V/V                   | MMX               | Converts 2 packed signed doubleword     
                                      |      |                       |                   | integers from mm1 and from mm2/m64 into 
                                      |      |                       |                   | 4 packed signed word integers in mm1    
                                      |      |                       |                   | using signed saturation.                
 66 0F 6B /r PACKSSDW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Converts 4 packed signed doubleword     
                                      |      |                       |                   | integers from xmm1 and from xxm2/m128   
                                      |      |                       |                   | into 8 packed signed word integers in   
                                      |      |                       |                   | xxm1 using signed saturation.           
 VEX.NDS.128.66.0F.WIG 63 /r VPACKSSWB| RVM  | V/V                   | AVX               | Converts 8 packed signed word integers  
 xmm1,xmm2, xmm3/m128                 |      |                       |                   | from xmm2 and from xmm3/m128 into 16    
                                      |      |                       |                   | packed signed byte integers in xmm1     
                                      |      |                       |                   | using signed saturation.                
 VEX.NDS.128.66.0F.WIG 6B /r VPACKSSDW| RVM  | V/V                   | AVX               | Converts 4 packed signed doubleword     
 xmm1,xmm2, xmm3/m128                 |      |                       |                   | integers from xmm2 and from xmm3/m128   
                                      |      |                       |                   | into 8 packed signed word integers in   
                                      |      |                       |                   | xmm1 using signed saturation.           
 VEX.NDS.256.66.0F.WIG 63 /r VPACKSSWB| RVM  | V/V                   | AVX2              | Converts 16 packed signed word integers 
 ymm1, ymm2, ymm3/m256                |      |                       |                   | from ymm2 and from ymm3/m256 into 32    
                                      |      |                       |                   | packed signed byte integers in ymm1     
                                      |      |                       |                   | using signed saturation.                
 VEX.NDS.256.66.0F.WIG 6B /r VPACKSSDW| RVM  | V/V                   | AVX2              | Converts 8 packed signed doubleword     
 ymm1, ymm2, ymm3/m256                |      |                       |                   | integers from ymm2 and from ymm3/m256   
                                      |      |                       |                   | into 16 packed signed word integers     
                                      |      |                       |                   | in ymm1using signed saturation.         
<aside class="notification">
1. See note in Section 2.4, “Instruction Exception Specification” in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 2A
and Section 22.25.3, “Exception Conditions of Legacy SIMD Instructions Operating
on MMX Registers” in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3A.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Converts packed signed word integers into packed signed byte integers (PACKSSWB)
or converts packed signed doubleword integers into packed signed word integers
(PACKSSDW), using saturation to handle overflow conditions. See Figure 4-2 for
an example of the packing operation.

   | |  
---- | -----
 64-Bit SRC                           | 64-Bit DEST
 C                                    | A          
 D'64-Bit DEST                        | A'         
 Operation of the PACKSSDW Instruction| Figure 4-2.
 ---  | ---
 Using 64-bit Operands                |            
The (V)PACKSSWB instruction converts 4, 8 or 16 signed word integers from the
destination operand (first operand) and 4, 8 or 16 signed word integers from
the source operand (second operand) into 8, 16 or 32 signed byte integers and
stores the result in the destination operand. If a signed word integer value
is beyond the range of a signed byte integer (that is, greater than 7FH for
a positive integer or greater than 80H for a negative integer), the saturated
signed byte integer value of 7FH or 80H, respectively, is stored in the destination.

The (V)PACKSSDW instruction packs 2, 4 or 8 signed doublewords from the destination
operand (first operand) and 2, 4 or 8 signed doublewords from the source operand
(second operand) into 4, 8 or 16 signed words in the destination operand (see
word (that is, greater than 7FFFH for a positive integer or greater than 8000H
for a negative integer), the saturated signed word integer value of 7FFFH or
8000H, respectively, is stored into the destination.

The (V)PACKSSWB and (V)PACKSSDW instructions operate on either 64-bit, 128-bit
operands or 256-bit operands. When operating on 64-bit operands, the destination
operand must be an MMX technology register and the source operand can be either
an MMX technology register or a 64-bit memory location. In 64-bit mode, using
a REX prefix in the form of REX.R permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The first source operand
is an XMM register. The second operand can be an XMM register or a 128-bit memory
location. The destination is not distinct from the first source XMM register
and the upper bits (VLMAX-1:128) of the corresponding YMM register destination
are unmodified. VEX.128 encoded version: The first source operand is an XMM
register. The second source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
the corresponding YMM register destination are zeroed.

VEX.256 encoded version: The first source operand is a YMM register. The second
source operand is a YMM register or a 256-bit memory location. The destination
operand is a YMM register.

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
