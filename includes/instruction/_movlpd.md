## MOVLPD - Move Low Packed Double-Precision Floating-Point Value

> Operation

``` slim
MOVLPD (128-bit Legacy SSE load)
DEST[63:0] <- SRC[63:0]
DEST[VLMAX-1:64] (Unmodified)
VMOVLPD (VEX.128 encoded load)
DEST[63:0] <- SRC2[63:0]
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0
VMOVLPD (store)
DEST[63:0] <- SRC[63:0]

```

 Opcode/Instruction                 | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 66 0F 12 /r MOVLPD xmm, m64        | RM   | V/V           | SSE2              | Move double-precision floating-point    
                                    |      |               |                   | value from m64 to low quadword of xmm   
                                    |      |               |                   | register.                               
 66 0F 13 /r MOVLPD m64, xmm        | MR   | V/V           | SSE2              | Move double-precision floating-point    
                                    |      |               |                   | nvalue from low quadword of xmm register
                                    |      |               |                   | to m64.                                 
 VEX.NDS.128.66.0F.WIG 12 /r VMOVLPD| RVM  | V/V           | AVX               | Merge double-precision floating-point   
 xmm2, xmm1, m64                    |      |               |                   | value from m64 and the high quadword    
                                    |      |               |                   | of xmm1.                                
 VEX.128.66.0F.WIG 13/r VMOVLPD m64,| MR   | V/V           | AVX               | Move double-precision floating-point    
 xmm1                               |      |               |                   | values from low quadword of xmm1 to     
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
from the source 64-bit memory operand and stores it in the low 64bits of the
destination XMM register. The upper 64bits of the XMM register are preserved.
The upper 128-bits of the corresponding YMM destination register are preserved.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). VEX.128 encoded load: Loads a double-precision floating-point
value from the source 64-bit memory operand (third operand), merges it with
the upper 64-bits of the first source XMM register (second operand), and stores
it in the low 128-bits of the destination XMM register (first operand). The
upper 128-bits of the destination YMM register are zeroed. 128-bit store: Stores
a double-precision floating-point value from the low 64-bits of the XMM register
### source (second operand) to the 64-bit memory location (first operand). Note
VMOVLPD (store) (VEX.128.66.0F 13 /r) is legal and has the same behavior as
the existing 66 0F 13 store. For VMOVLPD (store) (VEX.128.66.0F 13 /r) instruction
version, VEX.vvvv is reserved and must be 1111b otherwise instruction will #UD.
If VMOVLPD is encoded with VEX.L= 1, an attempt to execute the instruction encoded
with VEX.L= 1 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVLPD:| __m128d _mm_loadl_pd ( __m128d a, double
        | \*p)                                     
 MOVLPD:| void _mm_storel_pd (double \*p, __m128d  
        | a)                                      

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L= 1. If VEX.vvvv != 1111B.
