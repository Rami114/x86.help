## MOVD/MOVQ - Move Doubleword/Move Quadword

> Operation

``` slim
MOVD (when destination operand is MMX technology register)
  DEST[31:0] <- SRC;
  DEST[63:32] <- 00000000H;
MOVD (when destination operand is XMM register)
  DEST[31:0] <- SRC;
  DEST[127:32] <- 000000000000000000000000H;
  DEST[VLMAX-1:128] (Unmodified)
MOVD (when source operand is MMX technology or XMM register)
  DEST <- SRC[31:0];
VMOVD (VEX-encoded version when destination is an XMM register)
  DEST[31:0] <- SRC[31:0]
  DEST[VLMAX-1:32] <- 0
MOVQ (when destination operand is XMM register)
  DEST[63:0] <- SRC[63:0];
  DEST[127:64] <- 0000000000000000H;
  DEST[VLMAX-1:128] (Unmodified)
MOVQ (when destination operand is r/m64)
  DEST[63:0] <- SRC[63:0];
MOVQ (when source operand is XMM register or r/m64)
  DEST <- SRC[63:0];
VMOVQ (VEX-encoded version when destination is an XMM register)
  DEST[63:0] <- SRC[63:0]
  DEST[VLMAX-1:64] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 MOVD:| __m64 _mm_cvtsi32_si64 (int i )    
 MOVD:| int _mm_cvtsi64_si32 ( __m64m )    
 MOVD:| __m128i _mm_cvtsi32_si128 (int a)  
 MOVD:| int _mm_cvtsi128_si32 ( __m128i a) 
 MOVQ:| __int64 _mm_cvtsi128_si64(__m128i);
 MOVQ:| __m128i _mm_cvtsi64_si128(__int64);

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 0F 6E /r MOVD mm, r/m32                   | RM   | V/V           | MMX               | Move doubleword from r/m32 to mm.        
 REX.W + 0F 6E /r MOVQ mm, r/m64           | RM   | V/N.E.        | MMX               | Move quadword from r/m64 to mm.          
 0F 7E /r MOVD r/m32, mm                   | MR   | V/V           | MMX               | Move doubleword from mm to r/m32.        
 REX.W + 0F 7E /r MOVQ r/m64, mm           | MR   | V/N.E.        | MMX               | Move quadword from mm to r/m64.          
 VEX.128.66.0F.W0 6E /VMOVD xmm1, r32/m32  | RM   | V/V           | AVX               | Move doubleword from r/m32 to xmm1.      
 VEX.128.66.0F.W1 6E /r VMOVQ xmm1, r64/m64| RM   | V/N.E.        | AVX               | Move quadword from r/m64 to xmm1.        
 66 0F 6E /r MOVD xmm, r/m32               | RM   | V/V           | SSE2              | Move doubleword from r/m32 to xmm.       
 66 REX.W 0F 6E /r MOVQ xmm, r/m64         | RM   | V/N.E.        | SSE2              | Move quadword from r/m64 to xmm.         
 66 0F 7E /r MOVD r/m32, xmm               | MR   | V/V           | SSE2              | Move doubleword from xmm register to     
                                           |      |               |                   | r/m32.                                   
 66 REX.W 0F 7E /r MOVQ r/m64, xmm         | MR   | V/N.E.        | SSE2              | Move quadword from xmm register to r/m64.
 VEX.128.66.0F.W0 7E /r VMOVD r32/m32,     | MR   | V/V           | AVX               | Move doubleword from xmm1 register to    
 xmm1                                      |      |               |                   | r/m32.                                   
 VEX.128.66.0F.W1 7E /r VMOVQ r64/m64,     | MR   | V/N.E.        | AVX               | Move quadword from xmm1 register to      
 xmm1                                      |      |               |                   | r/m64.                                   

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
Copies a doubleword from the source operand (second operand) to the destination
operand (first operand). The source and destination operands can be general-purpose
registers, MMX technology registers, XMM registers, or 32-bit memory locations.
This instruction can be used to move a doubleword to and from the low doubleword
of an MMX technology register and a general-purpose register or a 32-bit memory
location, or to and from the low doubleword of an XMM register and a general-purpose
register or a 32-bit memory location. The instruction cannot be used to transfer
data between MMX technology registers, between XMM registers, between general-purpose
registers, or between memory locations.

When the destination operand is an MMX technology register, the source operand
is written to the low doubleword of the register, and the register is zero-extended
to 64 bits. When the destination operand is an XMM register, the source operand
is written to the low doubleword of the register, and the register is zero-extended
to 128 bits.

In 64-bit mode, the instruction's default operation size is 32 bits. Use of
the REX.R prefix permits access to additional registers (R8-R15). Use of the
REX.W prefix promotes operation to 64 bits. See the summary chart at the beginning
of this section for encoding data and limits.



### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1. If VEX.vvvv != 1111B.
