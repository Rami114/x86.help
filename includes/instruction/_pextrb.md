## PEXTRB/PEXTRD/PEXTRQ  -  Extract Byte/Dword/Qword

> Operation

``` slim
CASE of
  PEXTRB: SEL <- COUNT[3:0];
       TEMP <- (Src >> SEL\*8) AND FFH;
       IF (DEST = Mem8)
          THEN
          Mem8 <- TEMP[7:0];
       ELSE IF (64-Bit Mode and 64-bit register selected)
          THEN
             R64[7:0] <- TEMP[7:0];
             r64[63:8] <- ZERO_FILL; };
       ELSE
             R32[7:0] <- TEMP[7:0];
             r32[31:8] <- ZERO_FILL; };
       FI;
  PEXTRD:SEL <- COUNT[1:0];
       TEMP <- (Src >> SEL\*32) AND FFFF_FFFFH;
       DEST <- TEMP;
  PEXTRQ: SEL <- COUNT[0];
       TEMP <- (Src >> SEL\*64);
       DEST <- TEMP;```

### EASC
(V)PEXTRTD/(V)PEXTRQ
IF (64-Bit Mode and 64-bit dest operand)
THEN
  Src_Offset <- Imm8[0]
  r64/m64 <-(Src >> Src_Offset * 64)
ELSE
  Src_Offset <- Imm8[1:0]
  r32/m32 <- ((Src >> Src_Offset *32) AND 0FFFFFFFFh);
FI
(V)PEXTRB ( dest=m8)
SRC_Offset <- Imm8[3:0]
Mem8 <- (Src >> Src_Offset*8)
(V)PEXTRB ( dest=reg)
IF (64-Bit Mode )
THEN
  SRC_Offset <- Imm8[3:0]
  DEST[7:0] <- ((Src >> Src_Offset*8) AND 0FFh)
  DEST[63:8] <-ZERO_FILL;
ELSE
  SRC_Offset <-. Imm8[3:0];
  DEST[7:0] <- ((Src >> Src_Offset*8) AND 0FFh);
  DEST[31:8] <-ZERO_FILL;
FI

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PEXTRB:| int _mm_extract_epi8 (__m128i src, const
        | int ndx);                               
 PEXTRD:| int _mm_extract_epi32 (__m128i src,     
        | const int ndx);                         
 PEXTRQ:| __int64 _mm_extract_epi64 (__m128i src, 
        | const int ndx);                         

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 66 0F 3A 14 /r ib PEXTRB reg/m8, xmm2,| MRI  | V/V                   | SSE4_1            | Extract a byte integer value from xmm2 
 imm8                                  |      |                       |                   | at the source byte offset specified    
                                       |      |                       |                   | by imm8 into reg or m8. The upper bits 
                                       |      |                       |                   | of r32 or r64 are zeroed.              
 66 0F 3A 16 /r ib PEXTRD r/m32, xmm2, | MRI  | V/V                   | SSE4_1            | Extract a dword integer value from xmm2
 imm8                                  |      |                       |                   | at the source dword offset specified   
                                       |      |                       |                   | by imm8 into r/m32.                    
 66 REX.W 0F 3A 16 /r ib PEXTRQ r/m64, | MRI  | V/N.E.                | SSE4_1            | Extract a qword integer value from xmm2
 xmm2, imm8                            |      |                       |                   | at the source qword offset specified   
                                       |      |                       |                   | by imm8 into r/m64.                    
 VEX.128.66.0F3A.W0 14 /r ib VPEXTRB   | MRI  | V1/V                  | AVX               | Extract a byte integer value from xmm2 
 reg/m8, xmm2, imm8                    |      |                       |                   | at the source byte offset specified    
                                       |      |                       |                   | by imm8 into reg or m8. The upper bits 
                                       |      |                       |                   | of r64/r32 is filled with zeros.       
 VEX.128.66.0F3A.W0 16 /r ib VPEXTRD   | MRI  | V/V                   | AVX               | Extract a dword integer value from xmm2
 r32/m32, xmm2, imm8                   |      |                       |                   | at the source dword offset specified   
                                       |      |                       |                   | by imm8 into r32/m32.                  
 VEX.128.66.0F3A.W1 16 /r ib VPEXTRQ   | MRI  | V/i                   | AVX               | Extract a qword integer value from xmm2
 r64/m64, xmm2, imm8                   |      |                       |                   | at the source dword offset specified   
                                       |      |                       |                   | by imm8 into r64/m64.                  
<aside class="notification">
1. In 64-bit mode, VEX.W1 is ignored for VPEXTRB (similar to legacy REX.W=1
prefix in PEXTRB).
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 MRI  | ModRM:r/m (w)| ModRM:reg (r)| imm8     | NA       

### Description
Extract a byte/dword/qword integer value from the source XMM register at a byte/dword/qword
offset determined from imm8[3:0]. The destination can be a register or byte/dword/qword
memory location. If the destination is a register, the upper bits of the register
are zero extended. In legacy non-VEX encoded version and if the destination
operand is a register, the default operand size in 64-bit mode for PEXTRB/PEXTRD
is 64 bits, the bits above the least significant byte/dword data are filled
with zeros. PEXTRQ is not encodable in non-64-bit modes and requires REX.W in
64-bit mode. Note: In VEX.128 encoded versions, VEX.vvvv is reserved and must
be 1111b, VEX.L must be 0, otherwise the instruction will **``#UD.``** If the destination
operand is a register, the default operand size in 64-bit mode for VPEXTRB/VPEXTRD
is 64 bits, the bits above the least significant byte/word/dword data are filled
with zeros. Attempt to execute VPEXTRQ in non-64-bit mode will cause **``#UD.``**



### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1. If VEX.vvvv != 1111B.    
    | If VPEXTRQ in non-64-bit mode, VEX.W=1.
