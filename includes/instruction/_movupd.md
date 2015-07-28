## MOVUPD - Move Unaligned Packed Double-Precision Floating-Point Values

> Operation
``` slim

MOVUPD (128-bit load and register-copy form Legacy SSE version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] (Unmodified)
(V)MOVUPD (128-bit store form)
DEST[127:0] <- SRC[127:0]
VMOVUPD (VEX.128 encoded version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] <- 0
VMOVUPD (VEX.256 encoded version)
DEST[255:0] <- SRC[255:0]

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 66 0F 10 /r MOVUPD xmm1, xmm2/m128        | RM   | V/V           | SSE2              | Move packed double-precision floating-point
                                           |      |               |                   | values from xmm2/m128 to xmm1.             
 VEX.128.66.0F.WIG 10 /r VMOVUPD xmm1,     | RM   | V/V           | AVX               | Move unaligned packed double-precision     
 xmm2/m128                                 |      |               |                   | floating-point from xmm2/mem to xmm1.      
 VEX.256.66.0F.WIG 10 /r VMOVUPD ymm1,     | RM   | V/V           | AVX               | Move unaligned packed double-precision     
 ymm2/m256                                 |      |               |                   | floating-point from ymm2/mem to ymm1.      
 66 0F 11 /r MOVUPD xmm2/m128, xmm         | MR   | V/V           | SSE2              | Move packed double-precision floating-point
                                           |      |               |                   | values from xmm1 to xmm2/m128.             
 VEX.128.66.0F.WIG 11 /r VMOVUPD xmm2/m128,| MR   | V/V           | AVX               | Move unaligned packed double-precision     
 xmm1                                      |      |               |                   | floating-point from xmm1 to xmm2/mem.      
 VEX.256.66.0F.WIG 11 /r VMOVUPD ymm2/m256,| MR   | V/V           | AVX               | Move unaligned packed double-precision     
 ymm1                                      |      |               |                   | floating-point from ymm1 to ymm2/mem.      

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
### 128-bit versions

Moves a double quadword containing two packed double-precision floating-point
values from the source operand (second operand) to the destination operand (first
operand). This instruction can be used to load an XMM register from a 128-bit
memory location, store the contents of an XMM register into a 128-bit memory
location, or move data between two XMM registers.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: Bits (VLMAX-1:128) of the
corresponding YMM destination register remain unchanged.

When the source or destination operand is a memory operand, the operand may
be unaligned on a 16-byte boundary without causing a general-protection exception
(#GP) to be generated.1

To move double-precision floating-point values to and from memory locations
that are known to be aligned on 16byte boundaries, use the MOVAPD instruction.

While executing in 16-bit addressing mode, a linear address for a 128-bit data
access that overlaps the end of a 16bit segment is not allowed and is defined
as reserved behavior. A specific processor implementation may or may not generate
a general-protection exception (#GP) in this situation, and the address that
spans the end of the segment may or may not wrap around to the beginning of
the segment. VEX.128 encoded version: Bits (VLMAX-1:128) of the destination
YMM register are zeroed. VEX.256 encoded version: Moves 256 bits of packed double-precision
floating-point values from the source operand (second operand) to the destination
operand (first operand). This instruction can be used to load a YMM register
from a 256-bit memory location, to store the contents of a YMM register into
a 256-bit memory location, or to move data between two YMM registers.

   | |  
---- | -----
 1.| If alignment checking is enabled (CR0.AM  
   | = 1, RFLAGS.AC = 1, and CPL = 3), an      
   | alignment-check exception (#AC) may       
   | or may not be generated (depending on     
   | processor implementation) when the operand
   | is not aligned on an 8-byte boundary.     
<aside class="notification">
In VEX-encoded versions, VEX.vvvv is reserved and must be 1111b otherwise
instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVUPD: | __m128 _mm_loadu_pd(double \* p)       
 MOVUPD: | void _mm_storeu_pd(double \*p, __m128  
         | a)                                    
 VMOVUPD:| __m256d _mm256_loadu_pd (__m256d \* p);
 VMOVUPD:| _mm256_storeu_pd(_m256d \*p, __m256d   
         | a);                                   

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4 Note treatment of #AC varies; additionally

   | |  
---- | -----
 #UD| If VEX.vvvv != 1111B.
