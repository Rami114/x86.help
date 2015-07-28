## PAVGB/PAVGW - Average Packed Integers

> Operation

``` slim
PAVGB (with 64-bit operands)
  DEST[7:0] <- (SRC[7:0] + DEST[7:0] + 1) >> 1; (\* Temp sum before shifting is 9 bits \*)
  (\* Repeat operation performed for bytes 2 through 6 \*)
  DEST[63:56] <- (SRC[63:56] + DEST[63:56] + 1) >> 1;
PAVGW (with 64-bit operands)
  DEST[15:0] <- (SRC[15:0] + DEST[15:0] + 1) >> 1; (\* Temp sum before shifting is 17 bits \*)
  (\* Repeat operation performed for words 2 and 3 \*)
  DEST[63:48] <- (SRC[63:48] + DEST[63:48] + 1) >> 1;
PAVGB (with 128-bit operands)
  DEST[7:0] <- (SRC[7:0] + DEST[7:0] + 1) >> 1; (\* Temp sum before shifting is 9 bits \*)
  (\* Repeat operation performed for bytes 2 through 14 \*)
  DEST[127:120] <- (SRC[127:120] + DEST[127:120] + 1) >> 1;
PAVGW (with 128-bit operands)
  DEST[15:0] <- (SRC[15:0] + DEST[15:0] + 1) >> 1; (\* Temp sum before shifting is 17 bits \*)
  (\* Repeat operation performed for words 2 through 6 \*)
  DEST[127:112] <- (SRC[127:112] + DEST[127:112] + 1) >> 1;
VPAVGB (VEX.128 encoded version)
  DEST[7:0] <- (SRC1[7:0] + SRC2[7:0] + 1) >> 1;
  (\* Repeat operation performed for bytes 2 through 15 \*)
  DEST[127:120] <- (SRC1[127:120] + SRC2[127:120] + 1) >> 1
  DEST[VLMAX-1:128] <- 0
VPAVGW (VEX.128 encoded version)
  DEST[15:0] <- (SRC1[15:0] + SRC2[15:0] + 1) >> 1;
  (\* Repeat operation performed for 16-bit words 2 through 7 \*)
  DEST[127:112] <- (SRC1[127:112] + SRC2[127:112] + 1) >> 1
  DEST[VLMAX-1:128] <- 0
VPAVGB (VEX.256 encoded instruction)
  DEST[7:0] <- (SRC1[7:0] + SRC2[7:0] + 1) >> 1; (\* Temp sum before shifting is 9 bits \*)
  (\* Repeat operation performed for bytes 2 through 31)
  DEST[255:248] <- (SRC1[255:248] + SRC2[255:248] + 1) >> 1;
VPAVGW (VEX.256 encoded instruction)
  DEST[15:0] <- (SRC1[15:0] + SRC2[15:0] + 1) >> 1; (\* Temp sum before shifting is 17 bits \*)
  (\* Repeat operation performed for words 2 through 15)
  DEST[255:14]) <- (SRC1[255:240] + SRC2[255:240] + 1) >> 1;

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 0F E0 /r1 PAVGB mm1, mm2/m64            | RM   | V/V                   | SSE               | Average packed unsigned byte integers 
                                         |      |                       |                   | from mm2/m64 and mm1 with rounding.   
 66 0F E0, /r PAVGB xmm1, xmm2/m128      | RM   | V/V                   | SSE2              | Average packed unsigned byte integers 
                                         |      |                       |                   | from xmm2/m128 and xmm1 with rounding.
 0F E3 /r1 PAVGW mm1, mm2/m64            | RM   | V/V                   | SSE               | Average packed unsigned word integers 
                                         |      |                       |                   | from mm2/m64 and mm1 with rounding.   
 66 0F E3 /r PAVGW xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Average packed unsigned word integers 
                                         |      |                       |                   | from xmm2/m128 and xmm1 with rounding.
 VEX.NDS.128.66.0F.WIG E0 /r VPAVGB xmm1,| RVM  | V/V                   | AVX               | Average packed unsigned byte integers 
 xmm2, xmm3/m128                         |      |                       |                   | from xmm3/m128 and xmm2 with rounding.
 VEX.NDS.128.66.0F.WIG E3 /r VPAVGW xmm1,| RVM  | V/V                   | AVX               | Average packed unsigned word integers 
 xmm2, xmm3/m128                         |      |                       |                   | from xmm3/m128 and xmm2 with rounding.
 VEX.NDS.256.66.0F.WIG E0 /r VPAVGB ymm1,| RVM  | V/V                   | AVX2              | Average packed unsigned byte integers 
 ymm2, ymm3/m256                         |      |                       |                   | from ymm2, and ymm3/m256 with rounding
                                         |      |                       |                   | and store to ymm1.                    
 VEX.NDS.256.66.0F.WIG E3 /r VPAVGW ymm1,| RVM  | V/V                   | AVX2              | Average packed unsigned word integers 
 ymm2, ymm3/m256                         |      |                       |                   | from ymm2, ymm3/m256 with rounding to 
                                         |      |                       |                   | ymm1.                                 
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
Performs a SIMD average of the packed unsigned integers from the source operand
(second operand) and the destination operand (first operand), and stores the
results in the destination operand. For each corresponding pair of data elements
in the first and second operands, the elements are added together, a 1 is added
to the temporary sum, and that result is shifted right one bit position.

The (V)PAVGB instruction operates on packed unsigned bytes and the (V)PAVGW
instruction operates on packed unsigned words.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE instructions: The source operand can be an MMX technology register
or a 64-bit memory location. The destination operand can be an MMX technology
register. 128-bit Legacy SSE version: The first source operand is an XMM register.
The second operand can be an XMM register or a 128-bit memory location. The
destination is not distinct from the first source XMM register and the upper
bits (VLMAX-1:128) of the corresponding YMM register destination are unmodified.

VEX.128 encoded version: The first source operand is an XMM register. The second
source operand is an XMM register or 128-bit memory location. The destination
operand is an XMM register. The upper bits (VLMAX-1:128) of the corresponding
YMM register destination are zeroed. VEX.256 encoded version: The first source
operand is a YMM register. The second source operand is a YMM register or a
256-bit memory location. The destination operand is a YMM register.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PAVGB:   | __m64 _mm_avg_pu8 (__m64 a, __m64 b)      
 PAVGW:   | __m64 _mm_avg_pu16 (__m64 a, __m64 b)     
 (V)PAVGB:| __m128i _mm_avg_epu8 ( __m128i a, __m128i 
          | b)                                        
 (V)PAVGW:| __m128i _mm_avg_epu16 ( __m128i a, __m128i
          | b)                                        
 VPAVGB:  | __m256i _mm256_avg_epu8 ( __m256i a,      
          | __m256i b)                                
 VPAVGW:  | __m256i _mm256_avg_epu16 ( __m256i a,     
          | __m256i b)                                

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
