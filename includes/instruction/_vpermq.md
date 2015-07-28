## VPERMQ  -  Qwords Element Permutation

> Operation

``` slim
VPERMQ (VEX.256 encoded version)
DEST[63:0] <- (SRC[255:0] >> (IMM8[1:0] \* 64))[63:0];
DEST[127:64] <- (SRC[255:0] >> (IMM8[3:2] \* 64))[63:0];
DEST[191:128] <- (SRC[255:0] >> (IMM8[5:4] \* 64))[63:0];
DEST[255:192] <- (SRC[255:0] >> (IMM8[7:6] \* 64))[63:0];

```

 Opcode/Instruction                      | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 VEX.256.66.0F3A.W1 00 /r ib VPERMQ ymm1,| RMI  | V/V            | AVX2              | Permute qwords in ymm2/m256 using indexes
 ymm2/m256, imm8                         |      |                |                   | in imm8 and store the result in ymm1.    

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| Imm8     | NA       

### Description
Use two-bit index values in the immediate byte to select a qword element in
the source operand, the resultant qword value from the source operand is copied
to the corresponding element of the destination operand in the order of the
index field. Note that this instruction permits a qword in the source operand
to be copied to multiple locations in the destination operand. An attempt to
execute VPERMQ encoded with VEX.L= 0 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
VPERMQ: __m256i _mm256_permute4x64_epi64(__m256i a, int control)


### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 0.
