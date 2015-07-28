## MOVHPD - Move High Packed Double-Precision Floating-Point Value

> Operation

``` slim
MOVHPD (128-bit Legacy SSE load)
DEST[63:0] (Unmodified)
DEST[127:64] <- SRC[63:0]
DEST[VLMAX-1:128] (Unmodified)
VMOVHPD (VEX.128 encoded load)
DEST[63:0] <- SRC1[63:0]
DEST[127:64] <- SRC2[63:0]
DEST[VLMAX-1:128] <- 0
VMOVHPD (store)
DEST[63:0] <- SRC[127:64]

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 66 0F 16 /r MOVHPD xmm, m64            | RM   | V/V           | SSE2              | Move double-precision floating-point   
                                        |      |               |                   | value from m64 to high quadword of xmm.
 66 0F 17 /r MOVHPD m64, xmm            | MR   | V/V           | SSE2              | Move double-precision floating-point   
                                        |      |               |                   | value from high quadword of xmm to m64.
 VEX.NDS.128.66.0F.WIG 16 /r VMOVHPD    | RVM  | V/V           | AVX               | Merge double-precision floating-point  
 xmm2, xmm1, m64                        |      |               |                   | value from m64 and the low quadword    
                                        |      |               |                   | of xmm1.                               
 VEX128.66.0F.WIG 17/r VMOVHPD m64, xmm1| MR   | V/V           | AVX               | Move double-precision floating-point   
                                        |      |               |                   | values from high quadword of xmm1 to   
                                        |      |               |                   | m64.                                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 MR   | ModRM:r/m (w)   | ModRM:reg (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
This instruction cannot be used for register to register or memory to memory
moves. 128-bit Legacy SSE load: Moves a double-precision floating-point value
from the source 64-bit memory operand and stores it in the high 64bits of the
destination XMM register. The lower 64bits of the XMM register are preserved.
The upper 128-bits of the corresponding YMM destination register are preserved.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). VEX.128 encoded load: Loads a double-precision floating-point
value from the source 64-bit memory operand (third operand) and stores it in
the upper 64-bits of the destination XMM register (first operand). The low 64-bits
from second XMM register (second operand) are stored in the lower 64-bits of
the destination. The upper 128-bits of the destination YMM register are zeroed.
128-bit store: Stores a double-precision floating-point value from the high
64-bits of the XMM register source (second operand) to the 64-bit memory location
(first operand). Note: VMOVHPD (store) (VEX.128.66.0F 17 /r) is legal and has
the same behavior as the existing 66 0F 17 store. For VMOVHPD (store) (VEX.128.66.0F
17 /r) instruction version, VEX.vvvv is reserved and must be 1111b otherwise
instruction will #UD. If VMOVHPD is encoded with VEX.L= 1, an attempt to execute
the instruction encoded with VEX.L= 1 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVHPD:| __m128d _mm_loadh_pd ( __m128d a, double
        | \*p)                                     
 MOVHPD:| void _mm_storeh_pd (double \*p, __m128d  
        | a)                                      

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L= 1.
