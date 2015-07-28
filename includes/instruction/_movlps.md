## MOVLPS - Move Low Packed Single-Precision Floating-Point Values

> Operation
``` slim

MOVLPS (128-bit Legacy SSE load)
DEST[63:0] <- SRC[63:0]
DEST[VLMAX-1:64] (Unmodified)
VMOVLPS (VEX.128 encoded load)
DEST[63:0] <- SRC2[63:0]
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0
VMOVLPS (store)
DEST[63:0] <- SRC[63:0]

```

 Opcode/Instruction                    | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 0F 12 /r MOVLPS xmm, m64              | RM   | V/V           | SSE               | Move two packed single-precision floatingpoint 
                                       |      |               |                   | values from m64 to low quadword of xmm.        
 0F 13 /r MOVLPS m64, xmm              | MR   | V/V           | SSE               | Move two packed single-precision floatingpoint 
                                       |      |               |                   | values from low quadword of xmm to m64.        
 VEX.NDS.128.0F.WIG 12 /r VMOVLPS xmm2,| RVM  | V/V           | AVX               | Merge two packed single-precision floatingpoint
 xmm1, m64                             |      |               |                   | values from m64 and the high quadword          
                                       |      |               |                   | of xmm1.                                       
 VEX.128.0F.WIG 13/r VMOVLPS m64, xmm1 | MR   | V/V           | AVX               | Move two packed single-precision floatingpoint 
                                       |      |               |                   | values from low quadword of xmm1 to            
                                       |      |               |                   | m64.                                           

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 MR   | ModRM:r/m (w)   | ModRM:reg (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
This instruction cannot be used for register to register or memory to memory
moves. 128-bit Legacy SSE load: Moves two packed single-precision floating-point
values from the source 64-bit memory operand and stores them in the low 64-bits
of the destination XMM register. The upper 64bits of the XMM register are preserved.
The upper 128-bits of the corresponding YMM destination register are preserved.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). VEX.128 encoded load: Loads two packed single-precision
floating-point values from the source 64-bit memory operand (third operand),
merges them with the upper 64-bits of the first source XMM register (second
operand), and stores them in the low 128-bits of the destination XMM register
(first operand). The upper 128-bits of the destination YMM register are zeroed.
128-bit store: Loads two packed single-precision floating-point values from
the low 64-bits of the XMM register source (second operand) to the 64-bit memory
location (first operand). Note: VMOVLPS (store) (VEX.128.0F 13 /r) is legal
and has the same behavior as the existing 0F 13 store. For VMOVLPS (store) (VEX.128.0F
13 /r) instruction version, VEX.vvvv is reserved and must be 1111b otherwise
instruction will #UD.

If VMOVLPS is encoded with VEX.L= 1, an attempt to execute the instruction encoded
with VEX.L= 1 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVLPS:| __m128 _mm_loadl_pi ( __m128 a, __m64
        | \*p)                                  
 MOVLPS:| void _mm_storel_pi (__m64 \*p, __m128 
        | a)                                   

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 #UD| If VEX.L= 1. If VEX.vvvv != 1111B.
