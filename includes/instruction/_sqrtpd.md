## SQRTPD - Compute Square Roots of Packed Double-Precision Floating-Point Values

> Operation

``` slim
SQRTPD (128-bit Legacy SSE version)
DEST[63:0] <- SQRT(SRC[63:0])
DEST[127:64] <- SQRT(SRC[127:64])
DEST[VLMAX-1:128] (Unmodified)
VSQRTPD (VEX.128 encoded version)
DEST[63:0] <- SQRT(SRC[63:0])
DEST[127:64] <- SQRT(SRC[127:64])
DEST[VLMAX-1:128] <- 0
VSQRTPD (VEX.256 encoded version)
DEST[63:0] <- SQRT(SRC[63:0])
DEST[127:64] <- SQRT(SRC[127:64])
DEST[191:128] <- SQRT(SRC[191:128])
DEST[255:192] <- SQRT(SRC[255:192])

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 SQRTPD:| __m128d _mm_sqrt_pd (m128d a)      
 SQRTPD:| __m256d _mm256_sqrt_pd (__m256d a);

```

 Opcode*/Instruction                  | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                          
 ---  | --- | --- | --- | ---
 66 0F 51 /r SQRTPD xmm1, xmm2/m128   | RM   | V/V                   | SSE2              | Computes square roots of the packed  
                                      |      |                       |                   | doubleprecision floating-point values
                                      |      |                       |                   | in xmm2/m128 and stores the results  
                                      |      |                       |                   | in xmm1.                             
 VEX.128.66.0F.WIG 51 /r VSQRTPD xmm1,| RM   | V/V                   | AVX               | Computes Square Roots of the packed  
 xmm2/m128                            |      |                       |                   | doubleprecision floating-point values
                                      |      |                       |                   | in xmm2/m128 and stores the result in
                                      |      |                       |                   | xmm1.                                
 VEX.256.66.0F.WIG 51/r VSQRTPD ymm1, | RM   | V/V                   | AVX               | Computes Square Roots of the packed  
 ymm2/m256                            |      |                       |                   | doubleprecision floating-point values
                                      |      |                       |                   | in ymm2/m256 and stores the result in
                                      |      |                       |                   | ymm1.                                

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Performs a SIMD computation of the square roots of the two packed double-precision
floating-point values in the source operand (second operand) stores the packed
double-precision floating-point results in the destination operand. The source
operand can be an XMM register or a 128-bit memory location. The destination
operand is an XMM register. See Figure 11-3 in the Intel® 64 and IA-32 Architectures
Software Developer's Manual, Volume 1, for an illustration of a SIMD double-precision
floating-point operation.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
second source can be an XMM register or 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: the source operand second source operand or a 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
### the corresponding YMM register destination are zeroed. VEX.256 encoded version
The source operand is a YMM register or a 256-bit memory location. The destination
operand is a YMM register. Note: In VEX-encoded versions, VEX.vvvv is reserved
and must be 1111b otherwise instructions will **``#UD.``**



### SIMD Floating-Point Exceptions
Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 2; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
