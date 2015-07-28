## VPERMPD  -  Permute Double-Precision Floating-Point Elements

> Operation

``` slim
VPERMPD (VEX.256 encoded version)
DEST[63:0] <- (SRC[255:0] >> (IMM8[1:0] \* 64))[63:0];
DEST[127:64] <- (SRC[255:0] >> (IMM8[3:2] \* 64))[63:0];
DEST[191:128] <- (SRC[255:0] >> (IMM8[5:4] \* 64))[63:0];
DEST[255:192] <- (SRC[255:0] >> (IMM8[7:6] \* 64))[63:0];

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
VPERMPD: __m256d _mm256_permute4x64_pd(__m256d a, int control) ;


```

 Opcode/Instruction                 | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 VEX.256.66.0F3A.W1 01 /r ib VPERMPD| RMI  | V/V            | AVX2              | Permute double-precision floating-point
 ymm1, ymm2/m256, imm8              |      |                |                   | elements in ymm2/m256 using indexes    
                                    |      |                |                   | in imm8 and store the result in ymm1.  

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| Imm8     | NA       

### Description
Use two-bit index values in the immediate byte to select a double-precision
floating-point element in the source operand; the resultant data from the source
operand is copied to the corresponding element of the destination operand in
the order of the index field. Note that this instruction permits a qword in
the source operand to be copied to multiple location in the destination operand.
An attempt to execute VPERMPD encoded with VEX.L= 0 will cause an **``#UD``** exception.



### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 0.
