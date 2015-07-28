## PSRAW/PSRAD - Shift Packed Data Right Arithmetic

> Operation

``` slim
PSRAW (with 64-bit operand)
  IF (COUNT > 15)
     THEN COUNT <- 16;
  FI;
  DEST[15:0] <- SignExtend(DEST[15:0] >> COUNT);
  (\* Repeat shift operation for 2nd and 3rd words \*)
  DEST[63:48] <- SignExtend(DEST[63:48] >> COUNT);
PSRAD (with 64-bit operand)
  IF (COUNT > 31)
     THEN COUNT <- 32;
  FI;
  DEST[31:0] <- SignExtend(DEST[31:0] >> COUNT);
  DEST[63:32] <- SignExtend(DEST[63:32] >> COUNT);
PSRAW (with 128-bit operand)
  COUNT <- COUNT_SOURCE[63:0];
  IF (COUNT > 15)
     THEN COUNT <- 16;
  FI;
  DEST[15:0]
  (\* Repeat shift operation for 2nd through 7th words \*)
  DEST[127:112] <- SignExtend(DEST[127:112] >> COUNT);
PSRAD (with 128-bit operand)
  COUNT <- COUNT_SOURCE[63:0];
  IF (COUNT > 31)
     THEN COUNT <- 32;
  FI;
  DEST[31:0]
  (\* Repeat shift operation for 2nd and 3rd doublewords \*)
  DEST[127:96] <- SignExtend(DEST[127:96] >>COUNT);
PSRAW (xmm, xmm, xmm/m128)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_WORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
PSRAW (xmm, imm8)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_WORDS(DEST, imm8)
DEST[VLMAX-1:128] (Unmodified)
VPSRAW (xmm, xmm, xmm/m128)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_WORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPSRAW (xmm, imm8)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_WORDS(SRC1, imm8)
DEST[VLMAX-1:128] <- 0
PSRAD (xmm, xmm, xmm/m128)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_DWORDS(DEST, SRC)
DEST[VLMAX-1:128] (Unmodified)
PSRAD (xmm, imm8)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_DWORDS(DEST, imm8)
DEST[VLMAX-1:128] (Unmodified)
VPSRAD (xmm, xmm, xmm/m128)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_DWORDS(SRC1, SRC2)
DEST[VLMAX-1:128] <- 0
VPSRAD (xmm, imm8)
DEST[127:0] <- ARITHMETIC_RIGHT_SHIFT_DWORDS(SRC1, imm8)
DEST[VLMAX-1:128] <- 0
VPSRAW (ymm, ymm, xmm/m128)
DEST[255:0] <- ARITHMETIC_RIGHT_SHIFT_WORDS_256b(SRC1, SRC2)
VPSRAW (ymm, imm8)
DEST[255:0] <- ARITHMETIC_RIGHT_SHIFT_WORDS_256b(SRC1, imm8)
VPSRAD (ymm, ymm, xmm/m128)
DEST[255:0] <- ARITHMETIC_RIGHT_SHIFT_DWORDS_256b(SRC1, SRC2)
VPSRAD (ymm, imm8)
DEST[255:0] <- ARITHMETIC_RIGHT_SHIFT_DWORDS_256b(SRC1, imm8)

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 0F E1 /r1 PSRAW mm, mm/m64              | RM   | V/V                   | MMX               | Shift words in mm right by mm/m64 while  
                                         |      |                       |                   | shifting in sign bits.                   
 66 0F E1 /r PSRAW xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Shift words in xmm1 right by xmm2/m128   
                                         |      |                       |                   | while shifting in sign bits.             
 0F 71 /4 ib1 PSRAW mm, imm8             | MI   | V/V                   | MMX               | Shift words in mm right by imm8 while    
                                         |      |                       |                   | shifting in sign bits                    
 66 0F 71 /4 ib PSRAW xmm1, imm8         | MI   | V/V                   | SSE2              | Shift words in xmm1 right by imm8 while  
                                         |      |                       |                   | shifting in sign bits                    
 0F E2 /r1 PSRAD mm, mm/m64              | RM   | V/V                   | MMX               | Shift doublewords in mm right by mm/m64  
                                         |      |                       |                   | while shifting in sign bits.             
 66 0F E2 /r PSRAD xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Shift doubleword in xmm1 right by xmm2   
                                         |      |                       |                   | /m128 while shifting in sign bits.       
 0F 72 /4 ib1 PSRAD mm, imm8             | MI   | V/V                   | MMX               | Shift doublewords in mm right by imm8    
                                         |      |                       |                   | while shifting in sign bits.             
 66 0F 72 /4 ib PSRAD xmm1, imm8         | MI   | V/V                   | SSE2              | Shift doublewords in xmm1 right by imm8  
                                         |      |                       |                   | while shifting in sign bits.             
 VEX.NDS.128.66.0F.WIG E1 /r VPSRAW xmm1,| RVM  | V/V                   | AVX               | Shift words in xmm2 right by amount      
 xmm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting    
                                         |      |                       |                   | in sign bits.                            
 VEX.NDD.128.66.0F.WIG 71 /4 ib VPSRAW   | VMI  | V/V                   | AVX               | Shift words in xmm2 right by imm8 while  
 xmm1, xmm2, imm8                        |      |                       |                   | shifting in sign bits.                   
 VEX.NDS.128.66.0F.WIG E2 /r VPSRAD xmm1,| RVM  | V/V                   | AVX               | Shift doublewords in xmm2 right by amount
 xmm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting    
                                         |      |                       |                   | in sign bits.                            
 VEX.NDD.128.66.0F.WIG 72 /4 ib VPSRAD   | VMI  | V/V                   | AVX               | Shift doublewords in xmm2 right by imm8  
 xmm1, xmm2, imm8                        |      |                       |                   | while shifting in sign bits.             
 VEX.NDS.256.66.0F.WIG E1 /r VPSRAW ymm1,| RVM  | V/V                   | AVX2              | Shift words in ymm2 right by amount      
 ymm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting    
                                         |      |                       |                   | in sign bits.                            
 VEX.NDD.256.66.0F.WIG 71 /4 ib VPSRAW   | VMI  | V/V                   | AVX2              | Shift words in ymm2 right by imm8 while  
 ymm1, ymm2, imm8                        |      |                       |                   | shifting in sign bits.                   
 VEX.NDS.256.66.0F.WIG E2 /r VPSRAD ymm1,| RVM  | V/V                   | AVX2              | Shift doublewords in ymm2 right by amount
 ymm2, xmm3/m128                         |      |                       |                   | specified in xmm3/m128 while shifting    
                                         |      |                       |                   | in sign bits.                            
 VEX.NDD.256.66.0F.WIG 72 /4 ib VPSRAD   | VMI  | V/V                   | AVX2              | Shift doublewords in ymm2 right by imm8  
 ymm1, ymm2, imm8                        |      |                       |                   | while shifting in sign bits.             
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
 MI   | ModRM:r/m (r, w)| imm8         | NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       
 VMI  | VEX.vvvv (w)    | ModRM:r/m (r)| imm8         | NA       

### Description
Shifts the bits in the individual data elements (words or doublewords) in the
destination operand (first operand) to the right by the number of bits specified
in the count operand (second operand). As the bits in the data elements are
shifted right, the empty high-order bits are filled with the initial value of
the sign bit of the data element. If the value specified by the count operand
is greater than 15 (for words) or 31 (for doublewords), each destination data
element is filled with the initial value of the sign bit of the element. (Figure
4-14 gives an example of shifting words in a 64-bit operand.)

Pre-Shift

   | |  
---- | -----
 X3 PSRAW and PSRAD Instruction Operation| X2 X2 >> COUNT| X1 X1 >> COUNT| X0 DEST Shift Right with Sign Extension
 Using a 64-bit Operand                  |               |               | X0 >> COUNT DEST Figure 4-14.          
<aside class="notification">
Note that only the first 64-bits of a 128-bit count operand are checked to compute
the count. If the second source operand is a memory address, 128 bits are loaded.
</aside>

The (V)PSRAW instruction shifts each of the words in the destination operand
to the right by the number of bits specified in the count operand, and the (V)PSRAD
instruction shifts each of the doublewords in the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE instructions: The destination operand is an MMX technology register;
the count operand can be either an MMX technology register or an 64-bit memory
location. 128-bit Legacy SSE version: The destination and first source operands
are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. The count operand can be either an XMM register or a 128-bit
memory location or an 8-bit immediate. If the count operand is a memory address,
### 128 bits are loaded but the upper 64 bits are ignored. VEX.128 encoded version
The destination and first source operands are XMM registers. Bits (VLMAX-1:128)
of the destination YMM register are zeroed. The count operand can be either
an XMM register or a 128-bit memory location or an 8-bit immediate. If the count
operand is a memory address, 128 bits are loaded but the upper 64 bits are ignored.
VEX.256 encoded version: The destination and first source operands are YMM registers.
The count operand can be either an XMM register or a 128-bit memory location
or an 8-bit immediate.

<aside class="notification">
For shifts with an immediate count (VEX.128.66.0F 71-73 /4), VEX.vvvv
encodes the destination register, and VEX.B + ModRM.r/m encodes the source register.
VEX.L must be 0, otherwise instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PSRAW:   | __m64 _mm_srai_pi16 (__m64 m, int count) 
 PSRAW:   | __m64 _mm_sra_pi16 (__m64 m, __m64 count)
 (V)PSRAW:| count)                                   
 (V)PSRAW:| __m128i _mm_sra_epi16(__m128i m, __m128i 
          | count)                                   
 VPSRAW:  | __m256i _mm256_srai_epi16 (__m256i m,    
          | int count)                               
 VPSRAW:  | __m256i _mm256_sra_epi16 (__m256i m,     
          | __m128i count)                           
 PSRAD:   | __m64 _mm_srai_pi32 (__m64 m, int count) 
 PSRAD:   | __m64 _mm_sra_pi32 (__m64 m, __m64 count)
 (V)PSRAD:| count)                                   
 (V)PSRAD:| __m128i _mm_sra_epi32 (__m128i m, __m128i
          | count)                                   
 VPSRAD:  | __m256i _mm256_srai_epi32 (__m256i m,    
          | int count)                               
 VPSRAD:  | __m256i _mm256_sra_epi32 (__m256i m,     
          | __m128i count)                           

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4 and 7 for non-VEX-encoded instructions; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
