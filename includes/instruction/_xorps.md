## XORPS - Bitwise Logical XOR for Single-Precision Floating-Point Values

> Operation

``` slim
XORPS (128-bit Legacy SSE version)
DEST[31:0] <- SRC1[31:0] BITWISE XOR SRC2[31:0]
DEST[63:32] <- SRC1[63:32] BITWISE XOR SRC2[63:32]
DEST[95:64] <- SRC1[95:64] BITWISE XOR SRC2[95:64]
DEST[127:96] <- SRC1[127:96] BITWISE XOR SRC2[127:96]
DEST[VLMAX-1:128] (Unmodified)
VXORPS (VEX.128 encoded version)
DEST[31:0] <- SRC1[31:0] BITWISE XOR SRC2[31:0]
DEST[63:32] <- SRC1[63:32] BITWISE XOR SRC2[63:32]
DEST[95:64] <- SRC1[95:64] BITWISE XOR SRC2[95:64]
DEST[127:96] <- SRC1[127:96] BITWISE XOR SRC2[127:96]
DEST[VLMAX-1:128] <- 0
VXORPS (VEX.256 encoded version)
DEST[31:0] <- SRC1[31:0] BITWISE XOR SRC2[31:0]
DEST[63:32] <- SRC1[63:32] BITWISE XOR SRC2[63:32]
DEST[95:64] <- SRC1[95:64] BITWISE XOR SRC2[95:64]
DEST[127:96] <- SRC1[127:96] BITWISE XOR SRC2[127:96]
DEST[159:128] <- SRC1[159:128] BITWISE XOR SRC2[159:128]
DEST[191:160]<- SRC1[191:160] BITWISE XOR SRC2[191:160]
DEST[223:192] <- SRC1[223:192] BITWISE XOR SRC2[223:192]
DEST[255:224] <- SRC1[255:224] BITWISE XOR SRC2[255:224].

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 XORPS: | __m128 _mm_xor_ps(__m128 a, __m128 b) 
 VXORPS:| __m256 _mm256_xor_ps (__m256 a, __m256
        | b);                                   

```

 Opcode/Instruction                        | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F 57 /r XORPS xmm1, xmm2/m128            | RM   | V/V                   | SSE               | Bitwise exclusive-OR of xmm2/m128 and   
                                           |      |                       |                   | xmm1.                                   
 VEX.NDS.128.0F.WIG 57 /r VXORPS xmm1,xmm2,| RVM  | V/V                   | AVX               | Return the bitwise logical XOR of packed
 xmm3/m128                                 |      |                       |                   | singleprecision floating-point values   
                                           |      |                       |                   | in xmm2 and xmm3/mem.                   
 VEX.NDS.256.0F.WIG 57 /r VXORPS ymm1,     | RVM  | V/V                   | AVX               | Return the bitwise logical XOR of packed
 ymm2, ymm3/m256                           |      |                       |                   | singleprecision floating-point values   
                                           |      |                       |                   | in ymm2 and ymm3/mem.                   

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs a bitwise logical exclusive-OR of the four packed single-precision
floating-point values from the source operand (second operand) and the destination
operand (first operand), and stores the result in the destination operand. The
source operand can be an XMM register or a 128-bit memory location. The destination
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
