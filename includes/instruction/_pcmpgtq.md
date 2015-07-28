## PCMPGTQ  -  Compare Packed Data for Greater Than

> Operation
``` slim

IF (DEST[63-0] > SRC[63-0])
  THEN DEST[63-0] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[63-0] <- 0; FI
IF (DEST[127-64] > SRC[127-64])
  THEN DEST[127-64] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[127-64] <- 0; FI
VPCMPGTQ (VEX.128 encoded version)
DEST[127:0] <-COMPARE_QWORDS_GREATER(SRC1,SRC2)
DEST[VLMAX-1:128] <- 0
VPCMPGTQ (VEX.256 encoded version)
DEST[127:0] <-COMPARE_QWORDS_GREATER(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_QWORDS_GREATER(SRC1[255:128],SRC2[255:128])

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 66 0F 38 37 /r PCMPGTQ xmm1,xmm2/m128 | RM   | V/V                   | SSE4_2            | Compare packed signed qwords in xmm2/m128
                                       |      |                       |                   | and xmm1 for greater than.               
 VEX.NDS.128.66.0F38.WIG 37 /r VPCMPGTQ| RVM  | V/V                   | AVX               | Compare packed signed qwords in xmm2     
 xmm1, xmm2, xmm3/m128                 |      |                       |                   | and xmm3/m128 for greater than.          
 VEX.NDS.256.66.0F38.WIG 37 /r VPCMPGTQ| RVM  | V/V                   | AVX2              | Compare packed signed qwords in ymm2     
 ymm1, ymm2, ymm3/m256                 |      |                       |                   | and ymm3/m256 for greater than.          

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs an SIMD signed compare for the packed quadwords in the destination
operand (first operand) and the source operand (second operand). If the data
element in the first (destination) operand is greater than the corresponding
element in the second (source) operand, the corresponding data element in the
destination is set to all 1s; otherwise, it is set to 0s.

128-bit Legacy SSE version: The second source operand can be an XMM register
or a 128-bit memory location. The first source operand and destination operand
are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: The second source operand can be
an XMM register or a 128-bit memory location. The first source operand and destination
operand are XMM registers. Bits (VLMAX-1:128) of the corresponding YMM register
are zeroed. VEX.256 encoded version: The first source operand is a YMM register.
The second source operand is a YMM register or a 256-bit memory location. The
destination operand is a YMM register.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PCMPGTQ:| __m128i _mm_cmpgt_epi64(__m128i a, __m128i
            | b)                                        
 VPCMPGTQ:  | __m256i _mm256_cmpgt_epi64( __m256i       
            | a, __m256i b);                            

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
