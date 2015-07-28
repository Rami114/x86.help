## MOVAPS - Move Aligned Packed Single-Precision Floating-Point Values

> Operation

``` slim
MOVAPS (128-bit load- and register-copy- form Legacy SSE version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] (Unmodified)
(V)MOVAPS (128-bit store form)
DEST[127:0] <- SRC[127:0]
VMOVAPS (VEX.128 encoded version)
DEST[127:0] <- SRC[127:0]
DEST[VLMAX-1:128] <- 0
VMOVAPS (VEX.256 encoded version)
DEST[255:0] <- SRC[255:0]

```

 Opcode/Instruction                          | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F 28 /r MOVAPS xmm1, xmm2/m128             | RM   | V/V           | SSE               | Move packed single-precision floating-point
                                             |      |               |                   | values from xmm2/m128 to xmm1.             
 0F 29 /r MOVAPS xmm2/m128, xmm1             | MR   | V/V           | SSE               | Move packed single-precision floating-point
                                             |      |               |                   | values from xmm1 to xmm2/m128.             
 VEX.128.0F.WIG 28 /r VMOVAPS xmm1, xmm2/m128| RM   | V/V           | AVX               | Move aligned packed single-precision       
                                             |      |               |                   | floatingpoint values from xmm2/mem to      
                                             |      |               |                   | xmm1.                                      
 VEX.128.0F.WIG 29 /r VMOVAPS xmm2/m128,     | MR   | V/V           | AVX               | Move aligned packed single-precision       
 xmm1                                        |      |               |                   | floatingpoint values from xmm1 to xmm2/mem.
 VEX.256.0F.WIG 28 /r VMOVAPS ymm1, ymm2/m256| RM   | V/V           | AVX               | Move aligned packed single-precision       
                                             |      |               |                   | floatingpoint values from ymm2/mem to      
                                             |      |               |                   | ymm1.                                      
 VEX.256.0F.WIG 29 /r VMOVAPS ymm2/m256,     | MR   | V/V           | AVX               | Move aligned packed single-precision       
 ymm1                                        |      |               |                   | floatingpoint values from ymm1 to ymm2/mem.

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
Moves 4 or8 single-precision floating-point values from the source operand (second
operand) to the destination operand (first operand). This instruction can be
used to load an XMM or YMM register from an 128-bit or 256-bit memory location,
to store the contents of an XMM or YMM register into a 128-bit or 256-bit memory
location, or to move data between two XMM or two YMM registers. When the source
or destination operand is a memory operand, the operand must be aligned on a
16-byte (128-bit version) or 32-byte (VEX.256 encoded version) boundary or a
general-protection exception (#GP) will be generated.

To move single-precision floating-point values to and from unaligned memory
locations, use the (V)MOVUPS instruction.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). Note: In VEX-encoded versions, VEX.vvvv is reserved
and must be 1111b otherwise instructions will #UD. 128-bit versions: Moves 128
bits of packed single-precision floating-point values from the source operand
(second operand) to the destination operand (first operand). This instruction
can be used to load an XMM register from a 128-bit memory location, to store
the contents of an XMM register into a 128-bit memory location, or to move data
between two XMM registers. When the source or destination operand is a memory
operand, the operand must be aligned on a 16-byte boundary or a general-protection
exception (#GP) will be generated. To move single-precision floatingpoint values
to and from unaligned memory locations, use the VMOVUPS instruction. 128-bit
Legacy SSE version: Bits (VLMAX-1:128) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: Bits (VLMAX-1:128) of the
destination YMM register are zeroed. VEX.256 encoded version: Moves 256 bits
of packed single-precision floating-point values from the source operand (second
operand) to the destination operand (first operand). This instruction can be
used to load a YMM register from a 256-bit memory location, to store the contents
of a YMM register into a 256-bit memory location, or to move data between two
YMM registers.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVAPS: | __m128 _mm_load_ps (float const \* p);
 MOVAPS: | _mm_store_ps(float \* p, __m128 a);   
 VMOVAPS:| __m256 _mm256_load_ps (float const \* 
         | p);                                  
 VMOVAPS:| _mm256_store_ps(float \* p, __m256 a);

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 1.SSE; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
