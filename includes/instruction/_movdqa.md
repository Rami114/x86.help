## MOVDQA - Move Aligned Double Quadword

> Operation

``` slim
MOVDQA (128-bit load- and register- form Legacy SSE version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] (Unmodified)
(* #GP if SRC or DEST unaligned memory operand *)
(V)MOVDQA (128-bit store forms)
DEST[127:0] <- SRC[127:0]
VMOVDQA (VEX.128 encoded version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] <- 0
VMOVDQA (VEX.256 encoded version)
DEST[255:0] <- SRC[255:0]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 MOVDQA: | __m128i _mm_load_si128 ( __m128i *p)      
 MOVDQA: | void _mm_store_si128 ( __m128i *p, __m128i
         | a)                                        
 VMOVDQA:| __m256i _mm256_load_si256 (__m256i *      
         | p);                                       
 VMOVDQA:| _mm256_store_si256(_m256i *p, __m256i     
         | a);                                       

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 66 0F 6F /r MOVDQA xmm1, xmm2/m128        | RM   | V/V           | SSE2              | Move aligned double quadword from xmm2/m128
                                           |      |               |                   | to xmm1.                                   
 66 0F 7F /r MOVDQA xmm2/m128, xmm1        | MR   | V/V           | SSE2              | Move aligned double quadword from xmm1     
                                           |      |               |                   | to xmm2/m128.                              
 VEX.128.66.0F.WIG 6F /r VMOVDQA xmm1,     | RM   | V/V           | AVX               | Move aligned packed integer values from    
 xmm2/m128                                 |      |               |                   | xmm2/mem to xmm1.                          
 VEX.128.66.0F.WIG 7F /r VMOVDQA xmm2/m128,| MR   | V/V           | AVX               | Move aligned packed integer values from    
 xmm1                                      |      |               |                   | xmm1 to xmm2/mem.                          
 VEX.256.66.0F.WIG 6F /r VMOVDQA ymm1,     | RM   | V/V           | AVX               | Move aligned packed integer values from    
 ymm2/m256                                 |      |               |                   | ymm2/mem to ymm1.                          
 VEX.256.66.0F.WIG 7F /r VMOVDQA ymm2/m256,| MR   | V/V           | AVX               | Move aligned packed integer values from    
 ymm1                                      |      |               |                   | ymm1 to ymm2/mem.                          

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
128-bit versions: Moves 128 bits of packed integer values from the source operand
(second operand) to the destination operand (first operand). This instruction
can be used to load an XMM register from a 128-bit memory location, to store
the contents of an XMM register into a 128-bit memory location, or to move data
between two XMM registers. When the source or destination operand is a memory
operand, the operand must be aligned on a 16-byte boundary or a general-protection
exception (**``#GP)``** will be generated. To move integer data to and from unaligned
memory locations, use the VMOVDQU instruction.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: Bits (VLMAX-1:128) of the
### corresponding YMM destination register remain unchanged. VEX.128 encoded version
Bits (VLMAX-1:128) of the destination YMM register are zeroed. VEX.256 encoded
version: Moves 256 bits of packed integer values from the source operand (second
operand) to the destination operand (first operand). This instruction can be
used to load a YMM register from a 256-bit memory location, to store the contents
of a YMM register into a 256-bit memory location, or to move data between two
YMM registers. When the source or destination operand is a memory operand, the
operand must be aligned on a 32-byte boundary or a general-protection exception
(**``#GP)``** will be generated. To move integer data to and from unaligned memory locations,
use the VMOVDQU instruction. Note: In VEX-encoded versions, VEX.vvvv is reserved
and must be 1111b otherwise instructions will **``#UD.``**



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 1.SSE2; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
