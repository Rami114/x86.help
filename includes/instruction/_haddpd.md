## HADDPD - Packed Double-FP Horizontal Add

> Operation

``` slim
HADDPD (128-bit Legacy SSE version)
DEST[63:0] <- SRC1[127:64] + SRC1[63:0]
DEST[127:64] <- SRC2[127:64] + SRC2[63:0]
DEST[VLMAX-1:128] (Unmodified)
VHADDPD (VEX.128 encoded version)
DEST[63:0] <- SRC1[127:64] + SRC1[63:0]
DEST[127:64] <- SRC2[127:64] + SRC2[63:0]
DEST[VLMAX-1:128] <- 0
VHADDPD (VEX.256 encoded version)
DEST[63:0] <- SRC1[127:64] + SRC1[63:0]
DEST[127:64] <- SRC2[127:64] + SRC2[63:0]
DEST[191:128] <- SRC1[255:192] + SRC1[191:128]
DEST[255:192] <- SRC2[255:192] + SRC2[191:128]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 VHADDPD:| __m256d _mm256_hadd_pd (__m256d a, __m256d
         | b);                                       
 HADDPD: | __m128d _mm_hadd_pd (__m128d a, __m128d   
         | b);                                       

```

 Opcode/Instruction                 | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 66 0F 7C /r HADDPD xmm1, xmm2/m128 | RM   | V/V           | SSE3              | Horizontal add packed double-precision
                                    |      |               |                   | floating-point values from xmm2/m128  
                                    |      |               |                   | to xmm1.                              
 VEX.NDS.128.66.0F.WIG 7C /r VHADDPD| RVM  | V/V           | AVX               | Horizontal add packed double-precision
 xmm1,xmm2, xmm3/m128               |      |               |                   | floating-point values from xmm2 and   
                                    |      |               |                   | xmm3/mem.                             
 VEX.NDS.256.66.0F.WIG 7C /r VHADDPD| RVM  | V/V           | AVX               | Horizontal add packed double-precision
 ymm1, ymm2, ymm3/m256              |      |               |                   | floating-point values from ymm2 and   
                                    |      |               |                   | ymm3/mem.                             

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Adds the double-precision floating-point values in the high and low quadwords
of the destination operand and stores the result in the low quadword of the
destination operand.

Adds the double-precision floating-point values in the high and low quadwords
of the source operand and stores the result in the high quadword of the destination
operand.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).

See Figure 3-15 for HADDPD; see Figure 3-16 for VHADDPD.

HADDPD xmm1, xmm2/m128

xmm2

   | |  
---- | -----
 [127:64]| [63:0]/m128                               
 [127:64]| xmm1 Result: xmm1[63:0] + xmm1[127:64]xmm1
 [127:64]| [63:0]                                    
OM15993

   | |  
---- | -----
 Figure 3-15.| HADDPD - Packed Double-FP Horizontal Add
 X3          | X0                                    
 Y3          | Y0                                    
 Y2 + Y3     | X0 + X1 VHADDPD operation             
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
