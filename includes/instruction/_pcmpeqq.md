## PCMPEQQ  -  Compare Packed Qword Data for Equal

> Operation

``` slim
IF (DEST[63:0] = SRC[63:0])
  THEN DEST[63:0] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[63:0] <- 0; FI;
IF (DEST[127:64] = SRC[127:64])
  THEN DEST[127:64] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[127:64] <- 0; FI;
VPCMPEQQ (VEX.128 encoded version)
DEST[127:0] <-COMPARE_QWORDS_EQUAL(SRC1,SRC2)
DEST[VLMAX-1:128] <- 0
VPCMPEQQ (VEX.256 encoded version)
DEST[127:0] <-COMPARE_QWORDS_EQUAL(SRC1[127:0],SRC2[127:0])
DEST[255:128] <-COMPARE_QWORDS_EQUAL(SRC1[255:128],SRC2[255:128])

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 66 0F 38 29 /r PCMPEQQ xmm1, xmm2/m128| RM   | V/V                   | SSE4_1            | Compare packed qwords in xmm2/m128 and
                                       |      |                       |                   | xmm1 for equality.                    
 VEX.NDS.128.66.0F38.WIG 29 /r VPCMPEQQ| RVM  | V/V                   | AVX               | Compare packed quadwords in xmm3/m128 
 xmm1, xmm2, xmm3/m128                 |      |                       |                   | and xmm2 for equality.                
 VEX.NDS.256.66.0F38.WIG 29 /r VPCMPEQQ| RVM  | V/V                   | AVX2              | Compare packed quadwords in ymm3/m256 
 ymm1, ymm2, ymm3 /m256                |      |                       |                   | and ymm2 for equality.                

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Performs an SIMD compare for equality of the packed quadwords in the destination
operand (first operand) and the

   | |  
---- | -----
 source operand (second operand). nation       | If a pair of data elements is equal, 
 is set to all 1s; otherwise, it is set        | the corresponding data element in the
 to 0s. 128-bit Legacy SSE version: The        | desti-                               
 second source operand can be an XMM           |                                      
 register or a 128-bit memory location.        |                                      
 The first source and destination operands     |                                      
 are XMM registers. Bits (VLMAX-1:128)         |                                      
 of the corresponding YMM destination          |                                      
 register remain unchanged. VEX.128 encoded    |                                      
 version: The second source operand can        |                                      
 be an XMM register or a 128-bit memory        |                                      
 location. The first source and destination    |                                      
 operands are XMM registers. Bits (VLMAX-1:128)|                                      
 ---  | ---
 of the corresponding YMM register are         |                                      
 zeroed. VEX.256 encoded version: The          |                                      
 first source operand is a YMM register.       |                                      
 The second source operand is a YMM register   |                                      
 or a 256-bit memory location. The destination |                                      
 operand is a YMM register. Note: VEX.L        |                                      
 ---  | ---
 must be 0, otherwise the instruction          |                                      
 will **``#UD.``**                                     |                                      


### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PCMPEQQ:| __m128i _mm_cmpeq_epi64(__m128i a, __m128i
            | b);                                       
 VPCMPEQQ:  | __m256i _mm256_cmpeq_epi64( __m256i       
            | a, __m256i b);                            

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
