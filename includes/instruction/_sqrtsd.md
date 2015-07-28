## SQRTSD - Compute Square Root of Scalar Double-Precision Floating-Point Value

> Operation

``` slim
SQRTSD (128-bit Legacy SSE version)
DEST[63:0] <- SQRT(SRC[63:0])
DEST[VLMAX-1:64] (Unmodified)
VSQRTSD (VEX.128 encoded version)
DEST[63:0] <- SQRT(SRC2[63:0])
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 SQRTSD:| __m128d _mm_sqrt_sd (m128d a, m128d
        | b)                                 

```

 Opcode*/Instruction                          | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                    
 ---  | --- | --- | --- | ---
 F2 0F 51 /r SQRTSD xmm1, xmm2/m64            | RM   | V/V                   | SSE2              | Computes square root of the low doubleprecision
                                              |      |                       |                   | floating-point value in xmm2/m64 and           
                                              |      |                       |                   | stores the results in xmm1.                    
 VEX.NDS.LIG.F2.0F.WIG 51/r VSQRTSD xmm1,xmm2,| RVM  | V/V                   | AVX               | Computes square root of the low doubleprecision
 xmm3/m64                                     |      |                       |                   | floating point value in xmm3/m64 and           
                                              |      |                       |                   | stores the results in xmm2. Also, upper        
                                              |      |                       |                   | double precision floating-point value          
                                              |      |                       |                   | (bits[127:64]) from xmm2 are copied            
                                              |      |                       |                   | to xmm1[127:64].                               

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Computes the square root of the low double-precision floating-point value in
the source operand (second operand) and stores the double-precision floating-point
result in the destination operand. The source operand can be an XMM register
or a 64-bit memory location. The destination operand is an XMM register. The
high quadword of the destination operand remains unchanged. See Figure 11-4
in the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume
1, for an illustration of a scalar double-precision floating-point operation.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
first source operand and the destination operand are the same. Bits (VLMAX1:64)
of the corresponding YMM destination register remain unchanged. VEX.128 encoded
version: Bits (VLMAX-1:128) of the destination YMM register are zeroed.



### SIMD Floating-Point Exceptions
Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3.
