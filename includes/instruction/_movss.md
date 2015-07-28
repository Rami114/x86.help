## MOVSS - Move Scalar Single-Precision Floating-Point Values

> Operation

``` slim
MOVSS (Legacy SSE version when the source and destination operands are both XMM registers)
DEST[31:0] <- SRC[31:0]
DEST[VLMAX-1:32] (Unmodified)
MOVSS/VMOVSS (when the source operand is an XMM register and the destination is memory)
DEST[31:0] <- SRC[31:0]
MOVSS (Legacy SSE version when the source operand is memory and the destination is an XMM register)
DEST[31:0] <- SRC[31:0]
DEST[127:32] <- 0
DEST[VLMAX-1:128] (Unmodified)
VMOVSS (VEX.NDS.128.F3.0F 11 /r where the destination is an XMM register)
DEST[31:0] <- SRC2[31:0]
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0
VMOVSS (VEX.NDS.128.F3.0F 10 /r where the source and destination are XMM registers)
DEST[31:0] <- SRC2[31:0]
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0
VMOVSS (VEX.NDS.128.F3.0F 10 /r when the source operand is memory and the destination is an XMM register)
DEST[31:0] <- SRC[31:0]
DEST[VLMAX-1:32] <- 0

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 F3 0F 10 /r MOVSS xmm1, xmm2/m32        | RM   | V/V           | SSE               | Move scalar single-precision floating-point 
                                         |      |               |                   | value from xmm2/m32 to xmm1 register.       
 VEX.NDS.LIG.F3.0F.WIG 10 /r VMOVSS xmm1,| RVM  | V/V           | AVX               | Merge scalar single-precision floating-point
 xmm2, xmm3                              |      |               |                   | value from xmm2 and xmm3 to xmm1 register.  
 VEX.LIG.F3.0F.WIG 10 /r VMOVSS xmm1,    | XM   | V/V           | AVX               | Load scalar single-precision floating-point 
 m32                                     |      |               |                   | value from m32 to xmm1 register.            
 F3 0F 11 /r MOVSS xmm2/m32, xmm         | MR   | V/V           | SSE               | Move scalar single-precision floating-point 
                                         |      |               |                   | value from xmm1 register to xmm2/m32.       
 VEX.NDS.LIG.F3.0F.WIG 11 /r VMOVSS xmm1,| MVR  | V/V           | AVX               | Move scalar single-precision floating-point 
 xmm2, xmm3                              |      |               |                   | value from xmm2 and xmm3 to xmm1 register.  
 VEX.LIG.F3.0F.WIG 11 /r VMOVSS m32,     | MR   | V/V           | AVX               | Move scalar single-precision floating-point 
 xmm1                                    |      |               |                   | value from xmm1 register to m32.            

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA           | NA       
 XM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 MVR  | ModRM:r/m (w)| VEX.vvvv (r) | ModRM:reg (r)| NA       

### Description
Moves a scalar single-precision floating-point value from the source operand
(second operand) to the destination operand (first operand). The source and
destination operands can be XMM registers or 32-bit memory locations. This instruction
can be used to move a single-precision floating-point value to and from the
low doubleword of an XMM register and a 32-bit memory location, or to move a
single-precision floating-point value between the low doublewords of two XMM
registers. The instruction cannot be used to transfer data between memory locations.
For non-VEX encoded syntax and when the source and destination operands are
XMM registers, the high doublewords of the destination operand remains unchanged.
When the source operand is a memory location and destination operand is an XMM
registers, the high doublewords of the destination operand is cleared to all
0s.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). VEX encoded instruction syntax supports two source operands
and a destination operand if ModR/M.mod field is 11B. VEX.vvvv is used to encode
the first source operand (the second operand). The low 128 bits of the destination
operand stores the result of merging the low dword of the second source operand
with three dwords in bits 127:32 of the first source operand. The upper bits
of the destination operand are cleared. Note: For the “VMOVSS m32, xmm1” (memory
store form) instruction version, VEX.vvvv is reserved and must be 1111b otherwise
instruction will #UD. Note: For the “VMOVSS xmm1, m32” (memory load form) instruction
version, VEX.vvvv is reserved and must be 1111b otherwise instruction will #UD.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVSS:| __m128 _mm_load_ss(float \* p)      
 MOVSS:| void _mm_store_ss(float \* p, __m128
       | a)                                 
 MOVSS:| __m128 _mm_move_ss(__m128 a, __m128
       | b)                                 

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
