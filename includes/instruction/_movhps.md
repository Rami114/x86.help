## MOVHPS - Move High Packed Single-Precision Floating-Point Values

> Operation

``` slim
MOVHPS (128-bit Legacy SSE load)
DEST[63:0] (Unmodified)
DEST[127:64] <- SRC[63:0]
DEST[VLMAX-1:128] (Unmodified)
VMOVHPS (VEX.128 encoded load)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
DEST[VLMAX-1:128] <- 0
VMOVHPS (store)
DEST[63:0] <- SRC[127:64]

```

 Opcode/Instruction                    | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 0F 16 /r MOVHPS xmm, m64              | RM   | V/V           | SSE               | Move two packed single-precision floatingpoint 
                                       |      |               |                   | values from m64 to high quadword of            
                                       |      |               |                   | xmm.                                           
 0F 17 /r MOVHPS m64, xmm              | MR   | V/V           | SSE               | Move two packed single-precision floatingpoint 
                                       |      |               |                   | values from high quadword of xmm to            
                                       |      |               |                   | m64.                                           
 VEX.NDS.128.0F.WIG 16 /r VMOVHPS xmm2,| RVM  | V/V           | AVX               | Merge two packed single-precision floatingpoint
 xmm1, m64                             |      |               |                   | values from m64 and the low quadword           
                                       |      |               |                   | of xmm1.                                       
 VEX.128.0F.WIG 17/r VMOVHPS m64, xmm1 | MR   | V/V           | AVX               | Move two packed single-precision floatingpoint 
                                       |      |               |                   | values from high quadword of xmm1to            
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
values from the source 64-bit memory operand and stores them in the high 64-bits
of the destination XMM register. The lower 64bits of the XMM register are preserved.
The upper 128-bits of the corresponding YMM destination register are preserved.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). VEX.128 encoded load: Loads two single-precision floating-point
values from the source 64-bit memory operand (third operand) and stores it in
the upper 64-bits of the destination XMM register (first operand). The low 64-bits
from second XMM register (second operand) are stored in the lower 64-bits of
the destination. The upper 128-bits of the destination YMM register are zeroed.
128-bit store: Stores two packed single-precision floating-point values from
the high 64-bits of the XMM register source (second operand) to the 64-bit memory
location (first operand). Note: VMOVHPS (store) (VEX.NDS.128.0F 17 /r) is legal
and has the same behavior as the existing 0F 17 store. For VMOVHPS (store) (VEX.NDS.128.0F
17 /r) instruction version, VEX.vvvv is reserved and must be 1111b otherwise
instruction will #UD. If VMOVHPS is encoded with VEX.L= 1, an attempt to execute
the instruction encoded with VEX.L= 1 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVHPS:| __m128d _mm_loadh_pi ( __m128d a, __m64
        | \*p)                                    
 MOVHPS:| void _mm_storeh_pi (__m64 \*p, __m128d  
        | a)                                     

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L= 1.
