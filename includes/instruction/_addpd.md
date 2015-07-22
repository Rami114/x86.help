## ADDPD - Add Packed Double-Precision Floating-Point Values 
> Operation 

``` slim
// ADDPD (128-bit Legacy SSE version)
DEST[63:0] <- DEST[63:0] + SRC[63:0];
DEST[127:64] <- DEST[127:64] ++ SRC[127:64];
DEST[VLMAX-1:128] (Unmodified)

// VADDPD (VEX.128 encoded version)
DEST[63:0] <-SRC1[63:0] + SRC2[63:0]
DEST[127:64] <- SRC1[127:64] + SRC2[127:64]
DEST[VLMAX-1:128] <- 0

// VADDPD (VEX.256 encoded version)
DEST[63:0] <- SRC1[63:0] + SRC2[63:0]
DEST[127:64] <- SRC1[127:64] + SRC2[127:64]
DEST[191:128] <- SRC1[191:128] + SRC2[191:128]
DEST[255:192] <- SRC1[255:192] + SRC2[255:192]
```

> Intel C/C++ Compiler Intrinsic Equivalent

``` c
// ADDPD        
__m128d _mm_add_pd (__m128d a, __m128d b)

// VADDPD     
__m256d _mm256_add_pd (__m256d a, __m256d b)
```

Opcode / Instruction | Op/En | 64/32bit Mode Support | CPUID Feature Flag | Description
-------------------- | ----- | ----------- | --------------- | -----------
66 0F 58 /r ADDPD xmm1, xmm2/m128 | RM  |    V/V   |            SSE2       |     Add packed double-precision floating-point values from xmm2/m128 to xmm1.
VEX.NDS.128.66.0F.WIG 58 /r VADDPD xmm1,xmm2, xmm3/m128 | RVM |   V/V    |           AVX         |    Add packed double-precision floating-point values from xmm3/mem to xmm2 and stores result in xmm1.
VEX.NDS.256.66.0F.WIG 58 /r VADDPD ymm1, ymm2, ymm3/m256 | RVM |  V/V   |            AVX         |    Add packed double-precision floating-point values from ymm3/mem to ymm2 and stores result in ymm1.

### Instruction Operand Encoding
Op/En  | Operand 1  | Operand 2  | Operand 3  | Operand 4
------ | ---------- | ---------- | ---------- | ---------
RM          |         ModRM:reg (r, w)       |                    ModRM:r/m (r )       |                          NA               |                                NA
RVM          |          ModRM:reg (w)       |                       VEX.vvvv (r )       |                       ModRM:r/m (r )      |                               NA

### Description
Performs a SIMD add of the two packed double-precision floating-point values from the source operand (second 
operand) and the destination operand (first operand), and stores the packed double-precision floating-point results 
in the destination operand. 

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction to access additional registers 
(XMM8-XMM15).

128-bit Legacy SSE version: The second source can be an XMM register or an 128-bit memory location. The desti-
nation is not distinct from the first source XMM register and the upper bits (VLMAX-1:128) of the corresponding 
YMM register destination are unmodified. See Chapter 11 in the Intel® 64 and IA-32 Architectures Software Devel-
oper’s Manual, Volume 1, for an overview of SIMD double-precision floating-point operation.

VEX.128 encoded version: the first source operand is an XMM register or 128-bit memory location. The destination 
operand is an XMM register. The upper bits (VLMAX-1:128) of the corresponding YMM register destination are 
zeroed.

VEX.256 encoded version: The first source operand is a YMM register. The second source operand can be a YMM 
register or a 256-bit memory location. The destination operand is a YMM register. 

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.

### Other Exceptions
See Exceptions Type 2.
