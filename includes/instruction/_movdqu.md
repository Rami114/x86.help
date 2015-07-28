## MOVDQU - Move Unaligned Double Quadword

> Operation

``` slim
MOVDQU load and register copy (128-bit Legacy SSE version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] (Unmodified)
(V)MOVDQU 128-bit store-form versions
DEST[127:0] <- SRC[127:0]
VMOVDQU (VEX.128 encoded version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] <- 0
VMOVDQU (VEX.256 encoded version)
DEST[255:0] <- SRC[255:0]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 MOVDQU: | void _mm_storeu_si128 ( __m128i *p,   
         | __m128i a)                            
 MOVDQU: | __m128i _mm_loadu_si128 ( __m128i *p) 
 VMOVDQU:| __m256i _mm256_loadu_si256 (__m256i   
         | * p);                                 
 VMOVDQU:| _mm256_storeu_si256(_m256i *p, __m256i
         | a);                                   

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                         
 ---  | --- | --- | --- | ---
 F3 0F 6F /r MOVDQU xmm1, xmm2/m128        | RM   | V/V           | SSE2              | Move unaligned double quadword from 
                                           |      |               |                   | xmm2/m128 to xmm1.                  
 F3 0F 7F /r MOVDQU xmm2/m128, xmm1        | MR   | V/V           | SSE2              | Move unaligned double quadword from 
                                           |      |               |                   | xmm1 to xmm2/m128.                  
 VEX.128.F3.0F.WIG 6F /r VMOVDQU xmm1,     | RM   | V/V           | AVX               | Move unaligned packed integer values
 xmm2/m128                                 |      |               |                   | from xmm2/mem to xmm1.              
 VEX.128.F3.0F.WIG 7F /r VMOVDQU xmm2/m128,| MR   | V/V           | AVX               | Move unaligned packed integer values
 xmm1                                      |      |               |                   | from xmm1 to xmm2/mem.              
 VEX.256.F3.0F.WIG 6F /r VMOVDQU ymm1,     | RM   | V/V           | AVX               | Move unaligned packed integer values
 ymm2/m256                                 |      |               |                   | from ymm2/mem to ymm1.              
 VEX.256.F3.0F.WIG 7F /r VMOVDQU ymm2/m256,| MR   | V/V           | AVX               | Move unaligned packed integer values
 ymm1                                      |      |               |                   | from ymm1 to ymm2/mem.              

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
### 128-bit versions

Moves 128 bits of packed integer values from the source operand (second operand)
to the destination operand (first operand). This instruction can be used to
load an XMM register from a 128-bit memory location, to store the contents of
an XMM register into a 128-bit memory location, or to move data between two
XMM registers. When the source or destination operand is a memory operand, the
operand may be unaligned on a 16-byte boundary without causing a general-protection
exception (**``#GP)``** to be generated.1

To move a double quadword to or from memory locations that are known to be aligned
on 16-byte boundaries, use the MOVDQA instruction.

While executing in 16-bit addressing mode, a linear address for a 128-bit data
access that overlaps the end of a 16bit segment is not allowed and is defined
as reserved behavior. A specific processor implementation may or may not generate
a general-protection exception (**``#GP)``** in this situation, and the address that
spans the end of the segment may or may not wrap around to the beginning of
the segment.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: Bits (VLMAX-1:128) of the
corresponding YMM destination register remain unchanged. When the source or
destination operand is a memory operand, the operand may be unaligned to any
alignment without causing a general-protection exception (**``#GP)``** to be generated
VEX.128 encoded version: Bits (VLMAX-1:128) of the destination YMM register
are zeroed.

VEX.256 encoded version: Moves 256 bits of packed integer values from the source
operand (second operand) to the destination operand (first operand). This instruction
can be used to load a YMM register from a 256-bit memory

   | |  
---- | -----
 1.| If alignment checking is enabled (CR0.AM  
   | = 1, RFLAGS.AC = 1, and CPL = 3), an      
   | alignment-check exception (**``#AC)``** may       
   | or may not be generated (depending on     
   | processor implementation) when the operand
   | is not aligned on an 8-byte boundary.     
location, to store the contents of a YMM register into a 256-bit memory location,
or to move data between two YMM registers. Note: In VEX-encoded versions, VEX.vvvv
is reserved and must be 1111b otherwise instructions will **``#UD.``**



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
