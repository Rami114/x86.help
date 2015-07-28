## PSUBB/PSUBW/PSUBD - Subtract Packed Integers

> Operation
``` slim

PSUBB (with 64-bit operands)
  DEST[7:0] <- DEST[7:0] − SRC[7:0];
  (\* Repeat subtract operation for 2nd through 7th byte \*)
  DEST[63:56] <- DEST[63:56] − SRC[63:56];
PSUBB (with 128-bit operands)
  DEST[7:0] <- DEST[7:0] − SRC[7:0];
  (\* Repeat subtract operation for 2nd through 14th byte \*)
  DEST[127:120] <- DEST[111:120] − SRC[127:120];
VPSUBB (VEX.128 encoded version)
DEST[7:0] <- SRC1[7:0]-SRC2[7:0]
DEST[15:8] <- SRC1[15:8]-SRC2[15:8]
DEST[23:16] <- SRC1[23:16]-SRC2[23:16]
DEST[31:24] <- SRC1[31:24]-SRC2[31:24]
DEST[39:32] <- SRC1[39:32]-SRC2[39:32]
DEST[47:40] <- SRC1[47:40]-SRC2[47:40]
DEST[55:48] <- SRC1[55:48]-SRC2[55:48]
DEST[63:56] <- SRC1[63:56]-SRC2[63:56]
DEST[71:64] <- SRC1[71:64]-SRC2[71:64]
DEST[79:72] <- SRC1[79:72]-SRC2[79:72]
DEST[87:80] <- SRC1[87:80]-SRC2[87:80]
DEST[95:88] <- SRC1[95:88]-SRC2[95:88]
DEST[103:96] <- SRC1[103:96]-SRC2[103:96]
DEST[111:104] <- SRC1[111:104]-SRC2[111:104]
DEST[119:112] <- SRC1[119:112]-SRC2[119:112]
DEST[127:120] <- SRC1[127:120]-SRC2[127:120]
DEST[VLMAX-1:128] <- 00
VPSUBB (VEX.256 encoded version)
DEST[7:0] <- SRC1[7:0]-SRC2[7:0]
DEST[15:8] <- SRC1[15:8]-SRC2[15:8]
DEST[23:16] <- SRC1[23:16]-SRC2[23:16]
DEST[31:24] <- SRC1[31:24]-SRC2[31:24]
DEST[39:32] <- SRC1[39:32]-SRC2[39:32]
DEST[47:40] <- SRC1[47:40]-SRC2[47:40]
DEST[55:48] <- SRC1[55:48]-SRC2[55:48]
DEST[63:56] <- SRC1[63:56]-SRC2[63:56]
DEST[71:64] <- SRC1[71:64]-SRC2[71:64]
DEST[79:72] <- SRC1[79:72]-SRC2[79:72]
DEST[87:80] <- SRC1[87:80]-SRC2[87:80]
DEST[95:88] <- SRC1[95:88]-SRC2[95:88]
DEST[103:96] <- SRC1[103:96]-SRC2[103:96]
DEST[111:104] <- SRC1[111:104]-SRC2[111:104]
DEST[119:112] <- SRC1[119:112]-SRC2[119:112]
DEST[127:120] <- SRC1[127:120]-SRC2[127:120]
DEST[135:128] <- SRC1[135:128]-SRC2[135:128]
DEST[143:136] <- SRC1[143:136]-SRC2[143:136]
DEST[151:144] <- SRC1[151:144]-SRC2[151:144]
DEST[159:152] <- SRC1[159:152]-SRC2[159:152]
DEST[167:160] <- SRC1[167:160]-SRC2[167:160]
DEST[175:168] <- SRC1[175:168]-SRC2[175:168]
DEST[183:176] <- SRC1[183:176]-SRC2[183:176]
DEST[191:184] <- SRC1[191:184]-SRC2[191:184]
DEST[199:192] <- SRC1[199:192]-SRC2[199:192]
DEST[207:200] <- SRC1[207:200]-SRC2[207:200]
DEST[215:208] <- SRC1[215:208]-SRC2[215:208]
DEST[223:216] <- SRC1[223:216]-SRC2[223:216]
DEST[231:224] <- SRC1[231:224]-SRC2[231:224]
DEST[239:232] <- SRC1[239:232]-SRC2[239:232]
DEST[247:240] <- SRC1[247:240]-SRC2[247:240]
DEST[255:248] <- SRC1[255:248]-SRC2[255:248]
PSUBW (with 64-bit operands)
  DEST[15:0] <- DEST[15:0] − SRC[15:0];
  (\* Repeat subtract operation for 2nd and 3rd word \*)
  DEST[63:48] <- DEST[63:48] − SRC[63:48];
PSUBW (with 128-bit operands)
  DEST[15:0]
  (\* Repeat subtract operation for 2nd through 7th word \*)
  DEST[127:112] <- DEST[127:112] − SRC[127:112];
VPSUBW (VEX.128 encoded version)
DEST[15:0] <- SRC1[15:0]-SRC2[15:0]
DEST[31:16] <- SRC1[31:16]-SRC2[31:16]
DEST[47:32] <- SRC1[47:32]-SRC2[47:32]
DEST[63:48] <- SRC1[63:48]-SRC2[63:48]
DEST[79:64] <- SRC1[79:64]-SRC2[79:64]
DEST[95:80] <- SRC1[95:80]-SRC2[95:80]
DEST[111:96] <- SRC1[111:96]-SRC2[111:96]
DEST[127:112] <- SRC1[127:112]-SRC2[127:112]
DEST[VLMAX-1:128] <- 0
VPSUBW (VEX.256 encoded version)
DEST[15:0] <- SRC1[15:0]-SRC2[15:0]
DEST[31:16] <- SRC1[31:16]-SRC2[31:16]
DEST[47:32] <- SRC1[47:32]-SRC2[47:32]
DEST[63:48] <- SRC1[63:48]-SRC2[63:48]
DEST[79:64] <- SRC1[79:64]-SRC2[79:64]
DEST[95:80] <- SRC1[95:80]-SRC2[95:80]
DEST[111:96] <- SRC1[111:96]-SRC2[111:96]
DEST[127:112] <- SRC1[127:112]-SRC2[127:112]
DEST[143:128] <- SRC1[143:128]-SRC2[143:128]
DEST[159:144] <- SRC1[159:144]-SRC2[159:144]
DEST[175:160] <- SRC1[175:160]-SRC2[175:160]
DEST[191:176] <- SRC1[191:176]-SRC2[191:176]
DEST[207:192] <- SRC1207:192]-SRC2[207:192]
DEST[223:208] <- SRC1[223:208]-SRC2[223:208]
DEST[239:224] <- SRC1[239:224]-SRC2[239:224]
DEST[255:240] <- SRC1[255:240]-SRC2[255:240]
PSUBD (with 64-bit operands)
  DEST[31:0] <- DEST[31:0] − SRC[31:0];
  DEST[63:32] <- DEST[63:32] − SRC[63:32];
PSUBD (with 128-bit operands)
  DEST[31:0]
  (\* Repeat subtract operation for 2nd and 3rd doubleword \*)
  DEST[127:96] <- DEST[127:96] − SRC[127:96];
VPSUBD (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0]-SRC2[31:0]
DEST[63:32] <- SRC1[63:32]-SRC2[63:32]
DEST[95:64] <- SRC1[95:64]-SRC2[95:64]
DEST[127:96] <- SRC1[127:96]-SRC2[127:96]
DEST[VLMAX-1:128] <- 0
VPSUBD (VEX.256 encoded version)
DEST[31:0] <- SRC1[31:0]-SRC2[31:0]
DEST[63:32] <- SRC1[63:32]-SRC2[63:32]
DEST[95:64] <- SRC1[95:64]-SRC2[95:64]
DEST[127:96] <- SRC1[127:96]-SRC2[127:96]
DEST[159:128] <- SRC1[159:128]-SRC2[159:128]
DEST[191:160] <- SRC1[191:160]-SRC2[191:160]
DEST[223:192] <- SRC1[223:192]-SRC2[223:192]
DEST[255:224] <- SRC1[255:224]-SRC2[255:224]

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 0F F8 /r1 PSUBB mm, mm/m64              | RM   | V/V                   | MMX               | Subtract packed byte integers in mm/m64   
                                         |      |                       |                   | from packed byte integers in mm.          
 66 0F F8 /r PSUBB xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Subtract packed byte integers in xmm2/m128
                                         |      |                       |                   | from packed byte integers in xmm1.        
 0F F9 /r1 PSUBW mm, mm/m64              | RM   | V/V                   | MMX               | Subtract packed word integers in mm/m64   
                                         |      |                       |                   | from packed word integers in mm.          
 66 0F F9 /r PSUBW xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Subtract packed word integers in xmm2/m128
                                         |      |                       |                   | from packed word integers in xmm1.        
 0F FA /r1 PSUBD mm, mm/m64              | RM   | V/V                   | MMX               | Subtract packed doubleword integers       
                                         |      |                       |                   | in mm/m64 from packed doubleword integers 
                                         |      |                       |                   | in mm.                                    
 66 0F FA /r PSUBD xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Subtract packed doubleword integers       
                                         |      |                       |                   | in xmm2/mem128 from packed doubleword     
                                         |      |                       |                   | integers in xmm1.                         
 VEX.NDS.128.66.0F.WIG F8 /r VPSUBB xmm1,| RVM  | V/V                   | AVX               | Subtract packed byte integers in xmm3/m128
 xmm2, xmm3/m128                         |      |                       |                   | from xmm2.                                
 VEX.NDS.128.66.0F.WIG F9 /r VPSUBW xmm1,| RVM  | V/V                   | AVX               | Subtract packed word integers in xmm3/m128
 xmm2, xmm3/m128                         |      |                       |                   | from xmm2.                                
 VEX.NDS.128.66.0F.WIG FA /r VPSUBD xmm1,| RVM  | V/V                   | AVX               | Subtract packed doubleword integers       
 xmm2, xmm3/m128                         |      |                       |                   | in xmm3/m128 from xmm2.                   
 VEX.NDS.256.66.0F.WIG F8 /r VPSUBB ymm1,| RVM  | V/V                   | AVX2              | Subtract packed byte integers in ymm3/m256
 ymm2, ymm3/m256                         |      |                       |                   | from ymm2.                                
 VEX.NDS.256.66.0F.WIG F9 /r VPSUBW ymm1,| RVM  | V/V                   | AVX2              | Subtract packed word integers in ymm3/m256
 ymm2, ymm3/m256                         |      |                       |                   | from ymm2.                                
 VEX.NDS.256.66.0F.WIG FA /r VPSUBD ymm1,| RVM  | V/V                   | AVX2              | Subtract packed doubleword integers       
 ymm2, ymm3/m256                         |      |                       |                   | in ymm3/m256 from ymm2.                   
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
Performs a SIMD subtract of the packed integers of the source operand (second
operand) from the packed integers of the destination operand (first operand),
and stores the packed integer results in the destination operand. See Figure
9-4 in the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume
1, for an illustration of a SIMD operation. Overflow is handled with wraparound,
as described in the following paragraphs.

The (V)PSUBB instruction subtracts packed byte integers. When an individual
result is too large or too small to be represented in a byte, the result is
wrapped around and the low 8 bits are written to the destination element.

The (V)PSUBW instruction subtracts packed word integers. When an individual
result is too large or too small to be represented in a word, the result is
wrapped around and the low 16 bits are written to the destination element.

The (V)PSUBD instruction subtracts packed doubleword integers. When an individual
result is too large or too small to be represented in a doubleword, the result
is wrapped around and the low 32 bits are written to the destination element.

<aside class="notification">
Note that the (V)PSUBB, (V)PSUBW, and (V)PSUBD instructions can operate on either
unsigned or signed (two's complement notation) packed integers; however, it
does not set bits in the EFLAGS register to indicate overflow and/or a carry.
To prevent undetected overflow conditions, software must control the ranges
of values upon which it operates.
</aside>

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE version: When operating on 64-bit operands, the destination operand
must be an MMX technology register and the source operand can be either an MMX
### technology register or a 64-bit memory location. 128-bit Legacy SSE version
The second source operand is an XMM register or a 128-bit memory location. The
first source operand and destination operands are XMM registers. Bits (VLMAX-1:128)
of the corresponding YMM destination register remain unchanged. VEX.128 encoded
version: The second source operand is an XMM register or a 128-bit memory location.
The first source operand and destination operands are XMM registers. Bits (VLMAX-1:128)
of the destination YMM register are zeroed. VEX.256 encoded version: The second
source operand is an YMM register or a 256-bit memory location. The first source
operand and destination operands are YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PSUBB:   | __m64 _mm_sub_pi8(__m64 m1, __m64 m2)     
 (V)PSUBB:| __m128i _mm_sub_epi8 ( __m128i a, __m128i 
          | b)                                        
 VPSUBB:  | __m256i _mm256_sub_epi8 ( __m256i a,      
          | __m256i b)                                
 PSUBW:   | __m64 _mm_sub_pi16(__m64 m1, __m64 m2)    
 (V)PSUBW:| __m128i _mm_sub_epi16 ( __m128i a, __m128i
          | b)                                        
 VPSUBW:  | __m256i _mm256_sub_epi16 ( __m256i a,     
          | __m256i b)                                
 PSUBD:   | __m64 _mm_sub_pi32(__m64 m1, __m64 m2)    
 (V)PSUBD:| __m128i _mm_sub_epi32 ( __m128i a, __m128i
          | b)                                        
 VPSUBD:  | __m256i _mm256_sub_epi32 ( __m256i a,     
          | __m256i b)                                

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
