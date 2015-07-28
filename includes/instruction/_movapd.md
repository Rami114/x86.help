## MOVAPD - Move Aligned Packed Double-Precision Floating-Point Values

> Operation

``` slim
MOVAPD (128-bit load- and register-copy- form Legacy SSE version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] (Unmodified)
(V)MOVAPD (128-bit store-form version)
DEST[127:0] <- SRC[127:0]
VMOVAPD (VEX.128 encoded version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] <- 0
VMOVAPD (VEX.256 encoded version)
DEST[255:0] <- SRC[255:0]

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 66 0F 28 /r MOVAPD xmm1, xmm2/m128        | RM   | V/V           | SSE2              | Move packed double-precision floating-point
                                           |      |               |                   | values from xmm2/m128 to xmm1.             
 66 0F 29 /r MOVAPD xmm2/m128, xmm1        | MR   | V/V           | SSE2              | Move packed double-precision floating-point
                                           |      |               |                   | values from xmm1 to xmm2/m128.             
 VEX.128.66.0F.WIG 28 /r VMOVAPD xmm1,     | RM   | V/V           | AVX               | Move aligned packed double-precision       
 xmm2/m128                                 |      |               |                   | floatingpoint values from xmm2/mem to      
                                           |      |               |                   | xmm1.                                      
 VEX.128.66.0F.WIG 29 /r VMOVAPD xmm2/m128,| MR   | V/V           | AVX               | Move aligned packed double-precision       
 xmm1                                      |      |               |                   | floatingpoint values from xmm1 to xmm2/mem.
 VEX.256.66.0F.WIG 28 /r VMOVAPD ymm1,     | RM   | V/V           | AVX               | Move aligned packed double-precision       
 ymm2/m256                                 |      |               |                   | floatingpoint values from ymm2/mem to      
                                           |      |               |                   | ymm1.                                      
 VEX.256.66.0F.WIG 29 /r VMOVAPD ymm2/m256,| MR   | V/V           | AVX               | Move aligned packed double-precision       
 ymm1                                      |      |               |                   | floatingpoint values from ymm1 to ymm2/mem.

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
Moves 2 or 4 double-precision floating-point values from the source operand
(second operand) to the destination operand (first operand). This instruction
can be used to load an XMM or YMM register from an 128-bit or 256-bit memory
location, to store the contents of an XMM or YMM register into a 128-bit or
256-bit memory location, or to move data between two XMM or two YMM registers.
When the source or destination operand is a memory operand, the operand must
be aligned on a 16-byte (128-bit version) or 32-byte (VEX.256 encoded version)
boundary or a general-protection exception (#GP) will be generated.

To move double-precision floating-point values to and from unaligned memory
locations, use the (V)MOVUPD instruction.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit versions: Moves 128 bits of packed double-precision
floating-point values from the source operand (second operand) to the destination
operand (first operand). This instruction can be used to load an XMM register
from a 128-bit memory location, to store the contents of an XMM register into
a 128-bit memory location, or to move data between two XMM registers. When the
source or destination operand is a memory operand, the operand must be aligned
on a 16-byte boundary or a general-protection exception (#GP) will be generated.
To move single-precision floating-point values to and from unaligned memory
locations, use the VMOVUPD instruction. 128-bit Legacy SSE version: Bits (VLMAX-1:128)
of the corresponding YMM destination register remain unchanged. VEX.128 encoded
version: Bits (VLMAX-1:128) of the destination YMM register destination are
zeroed. VEX.256 encoded version: Moves 256 bits of packed double-precision floating-point
values from the source operand (second operand) to the destination operand (first
operand). This instruction can be used to load a YMM register from a 256-bit
memory location, to store the contents of a YMM register into a 256-bit memory
location, or to move data between two YMM registers. When the source or destination
operand is a memory operand, the operand must be aligned on a 32-byte boundary
or a general-protection exception (#GP) will be generated. To move single-precision
floating-point values to and from unaligned memory locations, use the VMOVUPD
instruction.

<aside class="notification">
In VEX-encoded versions, VEX.vvvv is reserved and must be 1111b otherwise
instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVAPD: | __m128d _mm_load_pd (double const \* 
         | p);                                 
 MOVAPD: | _mm_store_pd(double \* p, __m128d a);
 VMOVAPD:| __m256d _mm256_load_pd (double const
         | \* p);                               
 VMOVAPD:| _mm256_store_pd(double \* p, __m256d 
         | a);                                 

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 1.SSE2; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
