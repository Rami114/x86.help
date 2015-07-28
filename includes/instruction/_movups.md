## MOVUPS - Move Unaligned Packed Single-Precision Floating-Point Values

> Operation
``` slim

MOVUPS (128-bit load and register-copy form Legacy SSE version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] (Unmodified)
(V)MOVUPS (128-bit store form)
DEST[127:0] <- SRC[127:0]
VMOVUPS (VEX.128 encoded load-form)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] <- 0
VMOVUPS (VEX.256 encoded version)
DEST[255:0] <- SRC[255:0]

```

 Opcode/Instruction                          | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F 10 /r MOVUPS xmm1, xmm2/m128             | RM   | V/V           | SSE               | Move packed single-precision floating-point
                                             |      |               |                   | values from xmm2/m128 to xmm1.             
 VEX.128.0F.WIG 10 /r VMOVUPS xmm1, xmm2/m128| RM   | V/V           | AVX               | Move unaligned packed single-precision     
                                             |      |               |                   | floating-point from xmm2/mem to xmm1.      
 VEX.256.0F.WIG 10 /r VMOVUPS ymm1, ymm2/m256| RM   | V/V           | AVX               | Move unaligned packed single-precision     
                                             |      |               |                   | floating-point from ymm2/mem to ymm1.      
 0F 11 /r MOVUPS xmm2/m128, xmm1             | MR   | V/V           | SSE               | Move packed single-precision floating-point
                                             |      |               |                   | values from xmm1 to xmm2/m128.             
 VEX.128.0F.WIG 11 /r VMOVUPS xmm2/m128,     | MR   | V/V           | AVX               | Move unaligned packed single-precision     
 xmm1                                        |      |               |                   | floating-point from xmm1 to xmm2/mem.      
 VEX.256.0F.WIG 11 /r VMOVUPS ymm2/m256,     | MR   | V/V           | AVX               | Move unaligned packed single-precision     
 ymm1                                        |      |               |                   | floating-point from ymm1 to ymm2/mem.      

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
128-bit versions: Moves a double quadword containing four packed single-precision
floating-point values from the source operand (second operand) to the destination
operand (first operand). This instruction can be used to load an XMM register
from a 128-bit memory location, store the contents of an XMM register into a
128-bit memory location, or move data between two XMM registers.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: Bits (VLMAX-1:128) of the
corresponding YMM destination register remain unchanged.

When the source or destination operand is a memory operand, the operand may
be unaligned on a 16-byte boundary without causing a general-protection exception
(#GP) to be generated.1

To move packed single-precision floating-point values to and from memory locations
that are known to be aligned on 16-byte boundaries, use the MOVAPS instruction.

While executing in 16-bit addressing mode, a linear address for a 128-bit data
access that overlaps the end of a 16bit segment is not allowed and is defined
as reserved behavior. A specific processor implementation may or may not generate
a general-protection exception (#GP) in this situation, and the address that
spans the end of the segment may or may not wrap around to the beginning of
the segment. VEX.128 encoded version: Bits (VLMAX-1:128) of the destination
YMM register are zeroed.

VEX.256 encoded version: Moves 256 bits of packed single-precision floating-point
values from the source operand (second operand) to the destination operand (first
operand). This instruction can be used to load a YMM register from a 256-bit
memory location, to store the contents of a YMM register into a 256-bit memory
location, or to move data between two YMM registers. Note: In VEX-encoded versions,
VEX.vvvv is reserved and must be 1111b otherwise instructions will #UD.

   | |  
---- | -----
 1.| If alignment checking is enabled (CR0.AM  
   | = 1, RFLAGS.AC = 1, and CPL = 3), an      
   | alignment-check exception (#AC) may       
   | or may not be generated (depending on     
   | processor implementation) when the operand
   | is not aligned on an 8-byte boundary.     


### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVUPS: | __m128 _mm_loadu_ps(double \* p)      
 MOVUPS: | void _mm_storeu_ps(double \*p, __m128 
         | a)                                   
 VMOVUPS:| __m256 _mm256_loadu_ps (__m256 \* p); 
 VMOVUPS:| _mm256_storeu_ps(_m256 \*p, __m256 a);

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4 Note treatment of #AC varies; additionally

   | |  
---- | -----
 #UD| If VEX.vvvv != 1111B.
