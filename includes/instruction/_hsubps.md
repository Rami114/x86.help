## HSUBPS - Packed Single-FP Horizontal Subtract

> Operation

``` slim
HSUBPS (128-bit Legacy SSE version)
DEST[31:0] <- SRC1[31:0] - SRC1[63:32]
DEST[63:32] <- SRC1[95:64] - SRC1[127:96]
DEST[95:64] <- SRC2[31:0] - SRC2[63:32]
DEST[127:96] <- SRC2[95:64] - SRC2[127:96]
DEST[VLMAX-1:128] (Unmodified)
VHSUBPS (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0] - SRC1[63:32]
DEST[63:32] <- SRC1[95:64] - SRC1[127:96]
DEST[95:64] <- SRC2[31:0] - SRC2[63:32]
DEST[127:96] <- SRC2[95:64] - SRC2[127:96]
DEST[VLMAX-1:128] <- 0
VHSUBPS (VEX.256 encoded version)
DEST[31:0] <- SRC1[31:0] - SRC1[63:32]
DEST[63:32] <- SRC1[95:64] - SRC1[127:96]
DEST[95:64] <- SRC2[31:0] - SRC2[63:32]
DEST[127:96] <- SRC2[95:64] - SRC2[127:96]
DEST[159:128] <- SRC1[159:128] - SRC1[191:160]
DEST[191:160] <- SRC1[223:192] - SRC1[255:224]
DEST[223:192] <- SRC2[159:128] - SRC2[191:160]
DEST[255:224] <- SRC2[223:192] - SRC2[255:224]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 HSUBPS: | __m128 _mm_hsub_ps(__m128 a, __m128    
         | b);                                    
 VHSUBPS:| __m256 _mm256_hsub_ps (__m256 a, __m256
         | b);                                    

```

 Opcode/Instruction                 | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 F2 0F 7D /r HSUBPS xmm1, xmm2/m128 | RM   | V/V           | SSE3              | Horizontal subtract packed single-precision
                                    |      |               |                   | floating-point values from xmm2/m128       
                                    |      |               |                   | to xmm1.                                   
 VEX.NDS.128.F2.0F.WIG 7D /r VHSUBPS| RVM  | V/V           | AVX               | Horizontal subtract packed single-precision
 xmm1, xmm2, xmm3/m128              |      |               |                   | floating-point values from xmm2 and        
                                    |      |               |                   | xmm3/mem.                                  
 VEX.NDS.256.F2.0F.WIG 7D /r VHSUBPS| RVM  | V/V           | AVX               | Horizontal subtract packed single-precision
 ymm1, ymm2, ymm3/m256              |      |               |                   | floating-point values from ymm2 and        
                                    |      |               |                   | ymm3/mem.                                  

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Subtracts the single-precision floating-point value in the second dword of the
destination operand from the first dword of the destination operand and stores
the result in the first dword of the destination operand.

Subtracts the single-precision floating-point value in the fourth dword of the
destination operand from the third dword of the destination operand and stores
the result in the second dword of the destination operand.

Subtracts the single-precision floating-point value in the second dword of the
source operand from the first dword of the source operand and stores the result
in the third dword of the destination operand.

Subtracts the single-precision floating-point value in the fourth dword of the
source operand from the third dword of the source operand and stores the result
in the fourth dword of the destination operand.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).

See Figure 3-21 for HSUBPS; see Figure 3-22 for VHSUBPS.

HSUBPS xmm1, xmm2/m128

xmm2/

   | |  
---- | -----
 [127:96]| [95:64]                                   | [63:32]                        | [31:0]m128                                
 [127:96]| [95:64]xmm2/m128 [31:0] - xmm2/m128[63:32]| [63:32]xmm1[95:64] xmm1[127:96]| xmm1 xmm2/m128 RESULT: [95:64] - xmm2/xmm1
         |                                           |                                | m128[127:96]                              
 [127:96]| [95:64]                                   | [63:32]                        | [31:0]                                    
OM15996

   | |  
---- | -----
 Figure 3-21.| HSUBPS - Packed Single-FP Horizontal Subtract
 X7          | X0                                         
 Y7          | Y0                                         
 Y6-Y7       | X0-X1 VHSUBPS operation                    
128-bit Legacy SSE version: The second source can be an XMM register or an 128-bit
memory location. The destination is not distinct from the first source XMM register
and the upper bits (VLMAX-1:128) of the corresponding YMM register destination
are unmodified. VEX.128 encoded version: the first source operand is an XMM
register or 128-bit memory location. The destination operand is an XMM register.
The upper bits (VLMAX-1:128) of the corresponding YMM register destination are
zeroed. VEX.256 encoded version: The first source operand is a YMM register.
The second source operand can be a YMM register or a 256-bit memory location.
The destination operand is a YMM register.



### Exceptions
When the source operand is a memory operand, the operand must be aligned on
a 16-byte boundary or a generalprotection exception (**``#GP)``** will be generated.


### Numeric Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 2.
