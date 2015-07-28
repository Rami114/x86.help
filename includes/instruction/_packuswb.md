## PACKUSWB - Pack with Unsigned Saturation

> Operation
``` slim

PACKUSWB (with 64-bit operands)
  DEST[7:0] <- SaturateSignedWordToUnsignedByte DEST[15:0];
  DEST[15:8] <- SaturateSignedWordToUnsignedByte DEST[31:16];
  DEST[23:16] <- SaturateSignedWordToUnsignedByte DEST[47:32];
  DEST[31:24] <- SaturateSignedWordToUnsignedByte DEST[63:48];
  DEST[39:32] <- SaturateSignedWordToUnsignedByte SRC[15:0];
  DEST[47:40] <- SaturateSignedWordToUnsignedByte SRC[31:16];
  DEST[55:48] <- SaturateSignedWordToUnsignedByte SRC[47:32];
  DEST[63:56] <- SaturateSignedWordToUnsignedByte SRC[63:48];
PACKUSWB (Legacy SSE instruction)
  DEST[7:0]<-SaturateSignedWordToUnsignedByte (DEST[15:0]);
  DEST[15:8] <-SaturateSignedWordToUnsignedByte (DEST[31:16]);
  DEST[23:16] <-SaturateSignedWordToUnsignedByte (DEST[47:32]);
  DEST[31:24] <- SaturateSignedWordToUnsignedByte (DEST[63:48]);
  DEST[39:32] <- SaturateSignedWordToUnsignedByte (DEST[79:64]);
  DEST[47:40] <- SaturateSignedWordToUnsignedByte (DEST[95:80]);
  DEST[55:48] <- SaturateSignedWordToUnsignedByte (DEST[111:96]);
  DEST[63:56] <- SaturateSignedWordToUnsignedByte (DEST[127:112]);
  DEST[71:64] <- SaturateSignedWordToUnsignedByte (SRC[15:0]);
  DEST[79:72] <- SaturateSignedWordToUnsignedByte (SRC[31:16]);
  DEST[87:80] <- SaturateSignedWordToUnsignedByte (SRC[47:32]);
  DEST[95:88] <- SaturateSignedWordToUnsignedByte (SRC[63:48]);
  DEST[103:96] <- SaturateSignedWordToUnsignedByte (SRC[79:64]);
  DEST[111:104] <- SaturateSignedWordToUnsignedByte (SRC[95:80]);
  DEST[119:112] <- SaturateSignedWordToUnsignedByte (SRC[111:96]);
  DEST[127:120] <- SaturateSignedWordToUnsignedByte (SRC[127:112]);
PACKUSWB (VEX.128 encoded version)
  DEST[7:0]<- SaturateSignedWordToUnsignedByte (SRC1[15:0]);
  DEST[15:8] <-SaturateSignedWordToUnsignedByte (SRC1[31:16]);
  DEST[23:16] <-SaturateSignedWordToUnsignedByte (SRC1[47:32]);
  DEST[31:24] <- SaturateSignedWordToUnsignedByte (SRC1[63:48]);
  DEST[39:32] <- SaturateSignedWordToUnsignedByte (SRC1[79:64]);
  DEST[47:40] <- SaturateSignedWordToUnsignedByte (SRC1[95:80]);
  DEST[55:48] <- SaturateSignedWordToUnsignedByte (SRC1[111:96]);
  DEST[63:56] <- SaturateSignedWordToUnsignedByte (SRC1[127:112]);
  DEST[71:64] <- SaturateSignedWordToUnsignedByte (SRC2[15:0]);
  DEST[79:72] <- SaturateSignedWordToUnsignedByte (SRC2[31:16]);
  DEST[87:80] <- SaturateSignedWordToUnsignedByte (SRC2[47:32]);
  DEST[95:88] <- SaturateSignedWordToUnsignedByte (SRC2[63:48]);
  DEST[103:96] <- SaturateSignedWordToUnsignedByte (SRC2[79:64]);
  DEST[111:104] <- SaturateSignedWordToUnsignedByte (SRC2[95:80]);
  DEST[119:112] <- SaturateSignedWordToUnsignedByte (SRC2[111:96]);
  DEST[127:120] <- SaturateSignedWordToUnsignedByte (SRC2[127:112]);
  DEST[VLMAX-1:128] <- 0;
VPACKUSWB (VEX.256 encoded version)
  DEST[7:0]<- SaturateSignedWordToUnsignedByte (SRC1[15:0]);
  DEST[15:8] <-SaturateSignedWordToUnsignedByte (SRC1[31:16]);
  DEST[23:16] <-SaturateSignedWordToUnsignedByte (SRC1[47:32]);
  DEST[31:24] <- SaturateSignedWordToUnsignedByte (SRC1[63:48]);
  DEST[39:32] <-SaturateSignedWordToUnsignedByte (SRC1[79:64]);
  DEST[47:40] <- SaturateSignedWordToUnsignedByte (SRC1[95:80]);
  DEST[55:48] <- SaturateSignedWordToUnsignedByte (SRC1[111:96]);
  DEST[63:56] <- SaturateSignedWordToUnsignedByte (SRC1[127:112]);
  DEST[71:64] <-SaturateSignedWordToUnsignedByte (SRC2[15:0]);
  DEST[79:72] <- SaturateSignedWordToUnsignedByte (SRC2[31:16]);
  DEST[87:80] <- SaturateSignedWordToUnsignedByte (SRC2[47:32]);
  DEST[95:88] <- SaturateSignedWordToUnsignedByte (SRC2[63:48]);
  DEST[103:96] <- SaturateSignedWordToUnsignedByte (SRC2[79:64]);
  DEST[111:104] <- SaturateSignedWordToUnsignedByte (SRC2[95:80]);
  DEST[119:112] <- SaturateSignedWordToUnsignedByte (SRC2[111:96]);
  DEST[127:120] <- SaturateSignedWordToUnsignedByte (SRC2[127:112]);
  DEST[135:128]<- SaturateSignedWordToUnsignedByte (SRC1[143:128]);
  DEST[143:136] <-SaturateSignedWordToUnsignedByte (SRC1[159:144]);
  DEST[151:144] <-SaturateSignedWordToUnsignedByte (SRC1[175:160]);
  DEST[159:152] <-SaturateSignedWordToUnsignedByte (SRC1[191:176]);
  DEST[167:160] <- SaturateSignedWordToUnsignedByte (SRC1[207:192]);
  DEST[175:168] <- SaturateSignedWordToUnsignedByte (SRC1[223:208]);
  DEST[183:176] <- SaturateSignedWordToUnsignedByte (SRC1[239:224]);
  DEST[191:184] <- SaturateSignedWordToUnsignedByte (SRC1[255:240]);
  DEST[199:192] <- SaturateSignedWordToUnsignedByte (SRC2[143:128]);
  DEST[207:200] <- SaturateSignedWordToUnsignedByte (SRC2[159:144]);
  DEST[215:208] <- SaturateSignedWordToUnsignedByte (SRC2[175:160]);
  DEST[223:216] <- SaturateSignedWordToUnsignedByte (SRC2[191:176]);
  DEST[231:224] <- SaturateSignedWordToUnsignedByte (SRC2[207:192]);
  DEST[239:232] <- SaturateSignedWordToUnsignedByte (SRC2[223:208]);
  DEST[247:240] <- SaturateSignedWordToUnsignedByte (SRC2[239:224]);
  DEST[255:248] <- SaturateSignedWordToUnsignedByte (SRC2[255:240]);

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 0F 67 /r1 PACKUSWB mm, mm/m64        | RM   | V/V                   | MMX               | Converts 4 signed word integers from     
                                      |      |                       |                   | mm and 4 signed word integers from mm/m64
                                      |      |                       |                   | into 8 unsigned byte integers in mm      
                                      |      |                       |                   | using unsigned saturation.               
 66 0F 67 /r PACKUSWB xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Converts 8 signed word integers from     
                                      |      |                       |                   | xmm1 and 8 signed word integers from     
                                      |      |                       |                   | xmm2/m128 into 16 unsigned byte integers 
                                      |      |                       |                   | in xmm1 using unsigned saturation.       
 VEX.NDS.128.66.0F.WIG 67 /r VPACKUSWB| RVM  | V/V                   | AVX               | Converts 8 signed word integers from     
 xmm1, xmm2, xmm3/m128                |      |                       |                   | xmm2 and 8 signed word integers from     
                                      |      |                       |                   | xmm3/m128 into 16 unsigned byte integers 
                                      |      |                       |                   | in xmm1 using unsigned saturation.       
 VEX.NDS.256.66.0F.WIG 67 /r VPACKUSWB| RVM  | V/V                   | AVX2              | Converts 16 signed word integers from    
 ymm1, ymm2, ymm3/m256                |      |                       |                   | ymm2 and 16signed word integers from     
                                      |      |                       |                   | ymm3/m256 into 32 unsigned byte integers 
                                      |      |                       |                   | in ymm1 using unsigned saturation.       
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
Converts 4, 8 or 16 signed word integers from the destination operand (first
operand) and 4, 8 or 16 signed word integers from the source operand (second
operand) into 8, 16 or 32 unsigned byte integers and stores the result in the
destination operand. (See Figure 4-2 for an example of the packing operation.)
If a signed word integer value is beyond the range of an unsigned byte integer
(that is, greater than FFH or less than 00H), the saturated unsigned byte integer
value of FFH or 00H, respectively, is stored in the destination.

The PACKUSWB instruction operates on either 64-bit, 128-bit or 256-bit operands.
When operating on 64-bit operands, the destination operand must be an MMX technology
register and the source operand can be either an MMX technology register or
a 64-bit memory location. In 64-bit mode, using a REX prefix in the form of
REX.R permits this instruction to access additional registers (XMM8-XMM15).
128-bit Legacy SSE version: The first source operand is an XMM register. The
second operand can be an XMM register or a 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: The first source operand is an XMM register. The second source operand
is an XMM register or 128-bit memory location. The destination operand is an
XMM register. The upper bits (VLMAX-1:128) of the corresponding YMM register
destination are zeroed. VEX.256 encoded version: The first source operand is
a YMM register. The second source operand is a YMM register or a 256-bit memory
location. The destination operand is a YMM register.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PACKUSWB:   | __m64 _mm_packs_pu16(__m64 m1, __m64
             | m2)                                 
 (V)PACKUSWB:| __m128i _mm_packus_epi16(__m128i m1,
             | __m128i m2)                         
 VPACKUSWB:  | __m256i _mm256_packus_epi16(__m256i 
             | m1, __m256i m2);                    

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
