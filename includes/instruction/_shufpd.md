## SHUFPD - Shuffle Packed Double-Precision Floating-Point Values

> Operation

``` slim
IF SELECT[0] = 0
  THEN DEST[63:0]
  ELSE DEST[63:0]
IF SELECT[1] = 0
  THEN DEST[127:64]
  ELSE DEST[127:64]
SHUFPD (128-bit Legacy SSE version)
IF IMM0[0] = 0
  THEN DEST[63:0] <- SRC1[63:0]
  ELSE DEST[63:0] <- SRC1[127:64] FI;
IF IMM0[1] = 0
  THEN DEST[127:64] <- SRC2[63:0]
  ELSE DEST[127:64] <- SRC2[127:64] FI;
DEST[VLMAX-1:128] (Unmodified)
VSHUFPD (VEX.128 encoded version)
IF IMM0[0] = 0
  THEN DEST[63:0] <- SRC1[63:0]
  ELSE DEST[63:0] <- SRC1[127:64] FI;
IF IMM0[1] = 0
  THEN DEST[127:64] <- SRC2[63:0]
  ELSE DEST[127:64] <- SRC2[127:64] FI;
DEST[VLMAX-1:128] <- 0
VSHUFPD (VEX.256 encoded version)
IF IMM0[0] = 0
  THEN DEST[63:0] <- SRC1[63:0]
  ELSE DEST[63:0] <- SRC1[127:64] FI;
IF IMM0[1] = 0
  THEN DEST[127:64] <- SRC2[63:0]
  ELSE DEST[127:64] <- SRC2[127:64] FI;
IF IMM0[2] = 0
  THEN DEST[191:128] <- SRC1[191:128]
  ELSE DEST[191:128] <- SRC1[255:192] FI;
IF IMM0[3] = 0
  THEN DEST[255:192] <- SRC2[191:128]
  ELSE DEST[255:192] <- SRC2[255:192] FI;

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 SHUFPD: | __m128d _mm_shuffle_pd(__m128d a, __m128d
         | b, unsigned int imm8)                    
 VSHUFPD:| __m256d _mm256_shuffle_pd (__m256d a,    
         | __m256d b, const int select);            

```

 Opcode*/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                  
 ---  | --- | --- | --- | ---
 66 0F C6 /r ib SHUFPD xmm1, xmm2/m128,| RMI  | V/V                   | SSE2              | Shuffle packed double-precision floatingpoint
 imm8                                  |      |                       |                   | values selected by imm8 from xmm1 and        
                                       |      |                       |                   | xmm2/m128 to xmm1.                           
 VEX.NDS.128.66.0F.WIG C6 /r ib VSHUFPD| RVMI | V/V                   | AVX               | Shuffle Packed double-precision floatingpoint
 xmm1, xmm2, xmm3/m128, imm8           |      |                       |                   | values selected by imm8 from xmm2 and        
                                       |      |                       |                   | xmm3/mem.                                    
 VEX.NDS.256.66.0F.WIG C6 /r ib VSHUFPD| RVMI | V/V                   | AVX               | Shuffle Packed double-precision floatingpoint
 ymm1, ymm2, ymm3/m256, imm8           |      |                       |                   | values selected by imm8 from ymm2 and        
                                       |      |                       |                   | ymm3/mem.                                    

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
Moves either of the two packed double-precision floating-point values from destination
operand (first operand) into the low quadword of the destination operand; moves
either of the two packed double-precision floating-point values from the source
operand into to the high quadword of the destination operand (see Figure 4-21).
The select operand (third operand) determines which values are moved to the
destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
source can be an XMM register or an 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: the first source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
### the corresponding YMM register destination are zeroed. VEX.256 encoded version
The first source operand is a YMM register. The second source operand can be
a YMM register or a 256-bit memory location. The destination operand is a YMM
register.

   | |  
---- | -----
 X1                      | X0 DEST     
 Y1                      | Y0 X1 or X0 
 SHUFPD Shuffle Operation| Figure 4-21.
The source operand can be an XMM register or a 128-bit memory location. The
### destination operand is an XMM register. The select operand is an 8-bit immediate
bit 0 selects which value is moved from the destination operand to the result
(where 0 selects the low quadword and 1 selects the high quadword) and bit 1
selects which value is moved from the source operand to the result. Bits 2 through
7 of the select operand are reserved and must be set to 0.



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4.
