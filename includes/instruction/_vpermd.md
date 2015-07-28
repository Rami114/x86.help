## VPERMD  -  Full Doublewords Element Permutation

> Operation
``` slim

VPERMD (VEX.256 encoded version)
DEST[31:0] <- (SRC2[255:0] >> (SRC1[2:0] \* 32))[31:0];
DEST[63:32] <- (SRC2[255:0] >> (SRC1[34:32] \* 32))[31:0];
DEST[95:64] <- (SRC2[255:0] >> (SRC1[66:64] \* 32))[31:0];
DEST[127:96] <- (SRC2[255:0] >> (SRC1[98:96] \* 32))[31:0];
DEST[159:128] <- (SRC2[255:0] >> (SRC1[130:128] \* 32))[31:0];
DEST[191:160] <- (SRC2[255:0] >> (SRC1[162:160] \* 32))[31:0];
DEST[223:192] <- (SRC2[255:0] >> (SRC1[194:192] \* 32))[31:0];
DEST[255:224] <- (SRC2[255:0] >> (SRC1[226:224] \* 32))[31:0];

```

 Opcode/Instruction                 | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 VEX.NDS.256.66.0F38.W0 36 /r VPERMD| RVM  | V/V            | AVX2              | Permute doublewords in ymm3/m256 using
 ymm1, ymm2, ymm3/m256              |      |                |                   | indexes in ymm2 and store the result  
                                    |      |                |                   | in ymm1.                              

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVM  | ModRM:reg (w)| VEX.vvvv | ModRM:r/m (r)| NA       

### Description
Use the index values in each dword element of the first source operand (the
second operand) to select a dword element in the second source operand (the
third operand), the resultant dword value from the second source operand is
copied to the destination operand (the first operand) in the corresponding position
of the index element. Note that this instruction permits a doubleword in the
source operand to be copied to more than one doubleword location in the destination
operand. An attempt to execute VPERMD encoded with VEX.L= 0 will cause an #UD
exception.



### Intel C/C++ Compiler Intrinsic Equivalent
VPERMD: __m256i _mm256_permutevar8x32_epi32(__m256i a, __m256i offsets);


### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 0 for VPERMD, If VEX.W =
    | 1.                                 
