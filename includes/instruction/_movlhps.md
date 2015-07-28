## MOVLHPS - Move Packed Single-Precision Floating-Point Values Low to High

> Operation

``` slim
MOVLHPS (128-bit two-argument form)
DEST[63:0] (Unmodified)
DEST[127:64] <- SRC[63:0]
DEST[VLMAX-1:128] (Unmodified)
VMOVLHPS (128-bit three-argument form)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 MOVHLPS:| __m128 _mm_movelh_ps(__m128 a, __m128
         | b)                                   

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 0F 16 /r MOVLHPS xmm1, xmm2            | RM   | V/V           | SSE               | Move two packed single-precision floatingpoint 
                                        |      |               |                   | values from low quadword of xmm2 to            
                                        |      |               |                   | high quadword of xmm1.                         
 VEX.NDS.128.0F.WIG 16 /r VMOVLHPS xmm1,| RVM  | V/V           | AVX               | Merge two packed single-precision floatingpoint
 xmm2, xmm3                             |      |               |                   | values from low quadword of xmm3 and           
                                        |      |               |                   | low quadword of xmm2.                          

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
This instruction cannot be used for memory to register moves. 128-bit two-argument
form: Moves two packed single-precision floating-point values from the low quadword
of the second XMM argument (second operand) to the high quadword of the first
XMM register (first argument). The low quadword of the destination operand is
left unchanged. The upper 128 bits of the corresponding YMM destination register
are unmodified. 128-bit three-argument form Moves two packed single-precision
floating-point values from the low quadword of the third XMM argument (third
operand) to the high quadword of the destination (first operand). Copies the
low quadword from the second XMM argument (second operand) to the low quadword
of the destination (first operand). The upper 128-bits of the destination YMM
register are zeroed. If VMOVLHPS is encoded with VEX.L= 1, an attempt to execute
the instruction encoded with VEX.L= 1 will cause an **``#UD``** exception.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 7; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L= 1.
