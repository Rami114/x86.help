## PALIGNR  -  Packed Align Right

> Operation
``` slim

PALIGNR (with 64-bit operands)
  temp1[127:0] = CONCATENATE(DEST,SRC)>>(imm8\*8)
  DEST[63:0] = temp1[63:0]
PALIGNR (with 128-bit operands)
temp1[255:0] <- ((DEST[127:0] << 128) OR SRC[127:0])>>(imm8\*8);
DEST[127:0] <- temp1[127:0]
DEST[VLMAX-1:128] (Unmodified)
VPALIGNR (VEX.128 encoded version)
temp1[255:0] <- ((SRC1[127:0] << 128) OR SRC2[127:0])>>(imm8\*8);
DEST[127:0] <- temp1[127:0]
DEST[VLMAX-1:128] <- 0
VPALIGNR (VEX.256 encoded version)
temp1[255:0] <- ((SRC1[127:0] << 128) OR SRC2[127:0])>>(imm8[7:0]\*8);
DEST[127:0] <- temp1[127:0]
temp1[255:0] <- ((SRC1[255:128] << 128) OR SRC2[255:128])>>(imm8[7:0]\*8);
DEST[255:128] <- temp1[127:0]

```

 Opcode/Instruction                        | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 0F 3A 0F /r ib1 PALIGNR mm1, mm2/m64,     | RMI  | V/V                   | SSSE3             | Concatenate destination and source operands,
 imm8                                      |      |                       |                   | extract byte-aligned result shifted         
                                           |      |                       |                   | to the right by constant value in imm8      
                                           |      |                       |                   | into mm1.                                   
 66 0F 3A 0F /r ib PALIGNR xmm1, xmm2/m128,| RMI  | V/V                   | SSSE3             | Concatenate destination and source operands,
 imm8                                      |      |                       |                   | extract byte-aligned result shifted         
                                           |      |                       |                   | to the right by constant value in imm8      
                                           |      |                       |                   | into xmm1.                                  
 VEX.NDS.128.66.0F3A.WIG 0F /r ib VPALIGNR | RVMI | V/V                   | AVX               | Concatenate xmm2 and xmm3/m128, extract     
 xmm1, xmm2, xmm3/m128, imm8               |      |                       |                   | byte aligned result shifted to the right    
                                           |      |                       |                   | by constant value in imm8 and result        
                                           |      |                       |                   | is stored in xmm1.                          
 VEX.NDS.256.66.0F3A.WIG 0F /r ib VPALIGNR | RVMI | V/V                   | AVX2              | Concatenate pairs of 16 bytes in ymm2       
 ymm1, ymm2, ymm3/m256, imm8               |      |                       |                   | and ymm3/m256 into 32-byte intermediate     
                                           |      |                       |                   | result, extract byte-aligned, 16-byte       
                                           |      |                       |                   | result shifted to the right by constant     
                                           |      |                       |                   | values in imm8 from each intermediate       
                                           |      |                       |                   | result, and two 16-byte results are         
                                           |      |                       |                   | stored in ymm1.                             
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
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
(V)PALIGNR concatenates the destination operand (the first operand) and the
source operand (the second operand) into an intermediate composite, shifts the
composite at byte granularity to the right by a constant immediate, and extracts
the right-aligned result into the destination. The first and the second operands
can be an MMX, XMM or a YMM register. The immediate value is considered unsigned.
Immediate shift counts larger than the 2L (i.e. 32 for 128-bit operands, or
16 for 64-bit operands) produce a zero result. Both operands can be MMX registers,
XMM registers or YMM registers. When the source operand is a 128-bit memory
operand, the operand must be aligned on a 16-byte boundary or a general-protection
exception (#GP) will be generated.

In 64-bit mode, use the REX prefix to access additional registers. 128-bit Legacy
SSE version: Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: The first source operand is an XMM
register. The second source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
### the corresponding YMM register destination are zeroed. VEX.256 encoded version
The first source operand is a YMM register and contains two 16-byte blocks.
The second source operand is a YMM register or a 256-bit memory location containing
two 16-byte block. The destination operand is a YMM register and contain two
16-byte results. The imm8[7:0] is the common shift count used for the two lower
16-byte block sources and the two upper 16-byte block sources. The low 16-byte
block of the two source

operands produce the low 16-byte result of the destination operand, the high
16-byte block of the two source operands produce the high 16-byte result of
the destination operand. Concatenation is done with 128-bit data in the first
and second source operand for both 128-bit and 256-bit instructions. The high
128-bits of the intermediate composite 256-bit result came from the 128-bit
data from the first source operand; the low 128-bits of the intermediate result
came from the 128-bit data of the second source operand. Note: VEX.L must be
0, otherwise the instruction will #UD.

   | |  
---- | -----
 127| 0| 127| 0 SRC2
Imm8[7:0]\*8

   | |  
---- | -----
 255| 128| 255| 128 SRC2
Imm8[7:0]\*8

   | |  
---- | -----
 255 Figure 4-3.| 128 256-bit VPALIGN Instruction Operation| 127| 0 DEST


### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PALIGNR:   | __m64 _mm_alignr_pi8 (__m64 a, __m64
            | b, int n)                           
 (V)PALIGNR:| __m128i _mm_alignr_epi8 (__m128i a, 
            | __m128i b, int n)                   
 VPALIGNR:  | __m256i _mm256_alignr_epi8 (__m256i 
            | a, __m256i b, const int n)          

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
