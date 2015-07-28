## VINSERTF128  -  Insert Packed Floating-Point Values

> Operation
``` slim

TEMP[255:0] <- SRC1[255:0]
CASE (imm8[0]) OF
  0: TEMP[127:0] <- SRC2[127:0]
  1: TEMP[255:128] <- SRC2[127:0]
ESAC
DEST <-TEMP

```

 Opcode/Instruction                         | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 VEX.NDS.256.66.0F3A.W0 18 /r ib VINSERTF128| RVM  | V/V           | AVX               | Insert a single precision floating-point 
 ymm1, ymm2, xmm3/m128, imm8                |      |               |                   | value selected by imm8 from xmm3/m128    
                                            |      |               |                   | into ymm2 at the specified destination   
                                            |      |               |                   | element specified by imm8 and zero out   
                                            |      |               |                   | destination elements in ymm1 as indicated
                                            |      |               |                   | in imm8.                                 

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVM  | ModRM:reg (w)| VEX.vvvv (r)| ModRM:r/m (r)| NA       

### Description
Performs an insertion of 128-bits of packed floating-point values from the second
source operand (third operand) into an the destination operand (first operand)
at an 128-bit offset from imm8[0]. The remaining portions of the destination
are written by the corresponding fields of the first source operand (second
operand). The second source operand can be either an XMM register or a 128-bit
memory location. The high 7 bits of the immediate are ignored.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 INSERTF128:| __m256 _mm256_insertf128_ps (__m256     
            | a, __m128 b, int offset);               
 INSERTF128:| __m256d _mm256_insertf128_pd (__m256d   
            | a, __m128d b, int offset);              
 INSERTF128:| __m256i _mm256_insertf128_si256 (__m256i
            | a, __m128i b, int offset);              

### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 #UD| If VEX.W = 1.
