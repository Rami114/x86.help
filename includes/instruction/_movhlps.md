## MOVHLPS -  Move Packed Single-Precision Floating-Point Values High to Low

> Operation
``` slim

MOVHLPS (128-bit two-argument form)
DEST[63:0] <- SRC[127:64]
DEST[VLMAX-1:64] (Unmodified)
VMOVHLPS (128-bit three-argument form)
DEST[63:0] <- SRC2[127:64]
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 0F 12 /r MOVHLPS xmm1, xmm2            | RM   | V/V           | SSE               | Move two packed single-precision floatingpoint 
                                        |      |               |                   | values from high quadword of xmm2 to           
                                        |      |               |                   | low quadword of xmm1.                          
 VEX.NDS.128.0F.WIG 12 /r VMOVHLPS xmm1,| RVM  | V/V           | AVX               | Merge two packed single-precision floatingpoint
 xmm2, xmm3                             |      |               |                   | values from high quadword of xmm3 and          
                                        |      |               |                   | low quadword of xmm2.                          

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
This instruction cannot be used for memory to register moves. 128-bit two-argument
form: Moves two packed single-precision floating-point values from the high
quadword of the second XMM argument (second operand) to the low quadword of
the first XMM register (first argument). The high quadword of the destination
operand is left unchanged. Bits (VLMAX-1:64) of the corresponding YMM destination
register are unmodified. 128-bit three-argument form Moves two packed single-precision
floating-point values from the high quadword of the third XMM argument (third
operand) to the low quadword of the destination (first operand). Copies the
high quadword from the second XMM argument (second operand) to the high quadword
of the destination (first operand). Bits (VLMAX-1:128) of the destination YMM
register are zeroed.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). If VMOVHLPS is encoded with VEX.L= 1, an attempt to
execute the instruction encoded with VEX.L= 1 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVHLPS:| __m128 _mm_movehl_ps(__m128 a, __m128
         | b)                                   

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 7; additionally

   | |  
---- | -----
 #UD| If VEX.L= 1.
