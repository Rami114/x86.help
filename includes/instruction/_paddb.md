## PADDB/PADDW/PADDD - Add Packed Integers

> Operation

``` slim
PADDB (with 64-bit operands)
  DEST[7:0] <- DEST[7:0] + SRC[7:0];
  (\* Repeat add operation for 2nd through 7th byte \*)
  DEST[63:56] <- DEST[63:56] + SRC[63:56];
PADDB (with 128-bit operands)
  DEST[7:0] <- DEST[7:0] + SRC[7:0];
  (\* Repeat add operation for 2nd through 14th byte \*)
  DEST[127:120] <- DEST[111:120] + SRC[127:120];
VPADDB (VEX.128 encoded version)
  DEST[7:0] <- SRC1[7:0]+SRC2[7:0]
  DEST[15:8] <- SRC1[15:8]+SRC2[15:8]
  DEST[23:16] <- SRC1[23:16]+SRC2[23:16]
  DEST[31:24] <- SRC1[31:24]+SRC2[31:24]
  DEST[39:32] <- SRC1[39:32]+SRC2[39:32]
  DEST[47:40] <- SRC1[47:40]+SRC2[47:40]
  DEST[55:48] <- SRC1[55:48]+SRC2[55:48]
  DEST[63:56] <- SRC1[63:56]+SRC2[63:56]
  DEST[71:64] <- SRC1[71:64]+SRC2[71:64]
  DEST[79:72] <- SRC1[79:72]+SRC2[79:72]
  DEST[87:80] <- SRC1[87:80]+SRC2[87:80]
  DEST[95:88] <- SRC1[95:88]+SRC2[95:88]
  DEST[103:96] <- SRC1[103:96]+SRC2[103:96]
  DEST[111:104] <- SRC1[111:104]+SRC2[111:104]
  DEST[119:112] <- SRC1[119:112]+SRC2[119:112]
  DEST[127:120] <- SRC1[127:120]+SRC2[127:120]
  DEST[VLMAX-1:128] <- 0
VPADDB (VEX.256 encoded instruction)
  DEST[7:0]<- SRC1[7:0] + SRC2[7:0];
  (\* Repeat add operation for 2nd through 31th byte \*)
  DEST[255:248]<- SRC1[255:248] + SRC2[255:248];
PADDW (with 64-bit operands)
  DEST[15:0] <- DEST[15:0] + SRC[15:0];
  (\* Repeat add operation for 2nd and 3th word \*)
  DEST[63:48] <- DEST[63:48] + SRC[63:48];
PADDW (with 128-bit operands)
  DEST[15:0]
  (\* Repeat add operation for 2nd through 7th word \*)
  DEST[127:112] <- DEST[127:112] + SRC[127:112];
VPADDW (VEX.128 encoded version)
  DEST[15:0] <- SRC1[15:0]+SRC2[15:0]
  DEST[31:16] <- SRC1[31:16]+SRC2[31:16]
  DEST[47:32] <- SRC1[47:32]+SRC2[47:32]
  DEST[63:48] <- SRC1[63:48]+SRC2[63:48]
  DEST[79:64] <- SRC1[79:64]+SRC2[79:64]
  DEST[95:80] <- SRC1[95:80]+SRC2[95:80]
  DEST[111:96] <- SRC1[111:96]+SRC2[111:96]
  DEST[127:112] <- SRC1[127:112]+SRC2[127:112]
  DEST[VLMAX-1:128] <- 0
VPADDW (VEX.256 encoded instruction)
  DEST[15:0] <- SRC1[15:0] + SRC2[15:0];
  (\* Repeat add operation for 2nd through 15th word \*)
  DEST[255:240]<- SRC1[255:240] + SRC2[255:240];
PADDD (with 64-bit operands)
  DEST[31:0] <- DEST[31:0] + SRC[31:0];
  DEST[63:32] <- DEST[63:32] + SRC[63:32];
PADDD (with 128-bit operands)
  DEST[31:0] <- DEST[31:0]
  (\* Repeat add operation for 2nd and 3th doubleword \*)
  DEST[127:96] <- DEST[127:96] + SRC[127:96];
VPADDD (VEX.128 encoded version)
  DEST[31:0] <- SRC1[31:0]+SRC2[31:0]
  DEST[63:32] <- SRC1[63:32]+SRC2[63:32]
  DEST[95:64] <- SRC1[95:64]+SRC2[95:64]
  DEST[127:96] <- SRC1[127:96]+SRC2[127:96]
  DEST[VLMAX-1:128] <- 0
VPADDD (VEX.256 encoded instruction)
  DEST[31:0]<- SRC1[31:0]
  (\* Repeat add operation for 2nd and 7th doubleword \*)
  DEST[255:224] <- SRC1[255:224] + SRC2[255:224];

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 0F FC /r1 PADDB mm, mm/m64              | RM   | V/V                   | MMX               | Add packed byte integers from mm/m64   
                                         |      |                       |                   | and mm.                                
 66 0F FC /r PADDB xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Add packed byte integers from xmm2/m128
                                         |      |                       |                   | and xmm1.                              
 0F FD /r1 PADDW mm, mm/m64              | RM   | V/V                   | MMX               | Add packed word integers from mm/m64   
                                         |      |                       |                   | and mm.                                
 66 0F FD /r PADDW xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Add packed word integers from xmm2/m128
                                         |      |                       |                   | and xmm1.                              
 0F FE /r1 PADDD mm, mm/m64              | RM   | V/V                   | MMX               | Add packed doubleword integers from    
                                         |      |                       |                   | mm/m64 and mm.                         
 66 0F FE /r PADDD xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Add packed doubleword integers from    
                                         |      |                       |                   | xmm2/m128 and xmm1.                    
 VEX.NDS.128.66.0F.WIG FC /r VPADDB xmm1,| RVM  | V/V                   | AVX               | Add packed byte integers from xmm3/m128
 xmm2, xmm3/m128                         |      |                       |                   | and xmm2.                              
 VEX.NDS.128.66.0F.WIG FD /r VPADDW xmm1,| RVM  | V/V                   | AVX               | Add packed word integers from xmm3/m128
 xmm2, xmm3/m128                         |      |                       |                   | and xmm2.                              
 VEX.NDS.128.66.0F.WIG FE /r VPADDD xmm1,| RVM  | V/V                   | AVX               | Add packed doubleword integers from    
 xmm2, xmm3/m128                         |      |                       |                   | xmm3/m128 and xmm2.                    
 VEX.NDS.256.66.0F.WIG FC /r VPADDB ymm1,| RVM  | V/V                   | AVX2              | Add packed byte integers from ymm2,    
 ymm2, ymm3/m256                         |      |                       |                   | and ymm3/m256 and store in ymm1.       
 VEX.NDS.256.66.0F.WIG FD /r VPADDW ymm1,| RVM  | V/V                   | AVX2              | Add packed word integers from ymm2,    
 ymm2, ymm3/m256                         |      |                       |                   | ymm3/m256 and store in ymm1.           
 VEX.NDS.256.66.0F.WIG FE /r VPADDD ymm1,| RVM  | V/V                   | AVX2              | Add packed doubleword integers from    
 ymm2, ymm3/m256                         |      |                       |                   | ymm2, ymm3/m256 and store in ymm1.     
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
Performs a SIMD add of the packed integers from the source operand (second operand)
and the destination operand (first operand), and stores the packed integer results
in the destination operand. See Figure 9-4 in the Intel® 64 and IA-32 Architectures
Software Developer's Manual, Volume 1, for an illustration of a SIMD operation.
Overflow is handled with wraparound, as described in the following paragraphs.
Adds the packed byte, word, doubleword, or quadword integers in the first source
operand to the second source operand and stores the result in the destination
operand. When a result is too large to be represented in the

8/16/32 integer (overflow), the result is wrapped around and the low bits are
written to the destination element (that is, the carry is ignored).

<aside class="notification">
Note that these instructions can operate on either unsigned or signed (two's
complement notation) integers; however, it does not set bits in the EFLAGS register
to indicate overflow and/or a carry. To prevent undetected overflow conditions,
software must control the ranges of the values operated on.
</aside>

These instructions can operate on either 64-bit, 128-bit or 256-bit operands.
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
destination are zeroed.

VEX.256 encoded version: The first source operand is a YMM register. The second
source operand is a YMM register or a 256-bit memory location. The destination
operand is a YMM register.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PADDB:   | __m64 _mm_add_pi8(__m64 m1, __m64 m2)     
 (V)PADDB:| __m128i _mm_add_epi8 (__m128ia,__m128ib   
          | )                                         
 VPADDB:  | __m256i _mm256_add_epi8 (__m256ia,__m256i 
          | b )                                       
 PADDW:   | __m64 _mm_add_pi16(__m64 m1, __m64 m2)    
 (V)PADDW:| __m128i _mm_add_epi16 ( __m128i a, __m128i
          | b)                                        
 VPADDW:  | __m256i _mm256_add_epi16 ( __m256i a,     
          | __m256i b)                                
 PADDD:   | __m64 _mm_add_pi32(__m64 m1, __m64 m2)    
 (V)PADDD:| __m128i _mm_add_epi32 ( __m128i a, __m128i
          | b)                                        
 VPADDD:  | __m256i _mm256_add_epi32 ( __m256i a,     
          | __m256i b)                                

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
