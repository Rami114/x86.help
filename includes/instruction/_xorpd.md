## XORPD - Bitwise Logical XOR for Double-Precision Floating-Point Values

> Operation

``` slim
XORPD (128-bit Legacy SSE version)
DEST[63:0] <- DEST[63:0] BITWISE XOR SRC[63:0]
DEST[127:64] <- DEST[127:64] BITWISE XOR SRC[127:64]
DEST[VLMAX-1:128] (Unmodified)
VXORPD (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0] BITWISE XOR SRC2[63:0]
DEST[127:64] <- SRC1[127:64] BITWISE XOR SRC2[127:64]
DEST[VLMAX-1:128] <- 0
VXORPD (VEX.256 encoded version)
DEST[63:0] <- SRC1[63:0] BITWISE XOR SRC2[63:0]
DEST[127:64] <- SRC1[127:64] BITWISE XOR SRC2[127:64]
DEST[191:128] <- SRC1[191:128] BITWISE XOR SRC2[191:128]
DEST[255:192] <- SRC1[255:192] BITWISE XOR SRC2[255:192]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 XORPD: | __m128d _mm_xor_pd(__m128d a, __m128d    
        | b)                                       
 VXORPD:| __m256d _mm256_xor_pd (__m256d a, __m256d
        | b);                                      

```

 Opcode/Instruction                           | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 66 0F 57 /r XORPD xmm1, xmm2/m128            | RM   | V/V                   | SSE2              | Bitwise exclusive-OR of xmm2/m128 and   
                                              |      |                       |                   | xmm1.                                   
 VEX.NDS.128.66.0F.WIG 57 /r VXORPD xmm1,xmm2,| RVM  | V/V                   | AVX               | Return the bitwise logical XOR of packed
 xmm3/m128                                    |      |                       |                   | double-precision floating-point values  
                                              |      |                       |                   | in xmm2 and xmm3/mem.                   
 VEX.NDS.256.66.0F.WIG 57 /r VXORPD ymm1,     | RVM  | V/V                   | AVX               | Return the bitwise logical XOR of packed
 ymm2, ymm3/m256                              |      |                       |                   | double-precision floating-point values  
                                              |      |                       |                   | in ymm2 and ymm3/mem.                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs a bitwise logical exclusive-OR of the two packed double-precision floating-point
values from the source operand (second operand) and the destination operand
(first operand), and stores the result in the destination operand. The source
operand can be an XMM register or a 128-bit memory location. The destination
operand is an XMM register.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
second source can be an XMM register or an 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: the first source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
### the corresponding YMM register destination are zeroed. VEX.256 encoded version
The first source operand is a YMM register. The second source operand can be
a YMM register or a 256-bit memory location. The destination operand is a YMM
register.



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4.
