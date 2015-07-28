## VPERM2F128  -  Permute Floating-Point Values

> Operation

``` slim
VPERM2F128
CASE IMM8[1:0] of
0: DEST[127:0] <- SRC1[127:0]
1: DEST[127:0] <- SRC1[255:128]
2: DEST[127:0] <- SRC2[127:0]
3: DEST[127:0] <- SRC2[255:128]
ESAC
CASE IMM8[5:4] of
0: DEST[255:128] <- SRC1[127:0]
1: DEST[255:128] <- SRC1[255:128]
2: DEST[255:128] <- SRC2[127:0]
3: DEST[255:128] <- SRC2[255:128]
ESAC
IF (imm8[3])
DEST[127:0] <- 0
FI
IF (imm8[7])
DEST[VLMAX-1:128] <- 0
FI

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 VPERM2F128:| __m256 _mm256_permute2f128_ps (__m256     
            | a, __m256 b, int control)                 
 VPERM2F128:| __m256d _mm256_permute2f128_pd (__m256d   
            | a, __m256d b, int control)                
 VPERM2F128:| __m256i _mm256_permute2f128_si256 (__m256i
            | a, __m256i b, int control)                

```

 Opcode/Instruction                        | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                          
 ---  | --- | --- | --- | ---
 VEX.NDS.256.66.0F3A.W0 06 /r ib VPERM2F128| RVMI | V/V                   | AVX               | Permute 128-bit floating-point fields
 ymm1, ymm2, ymm3/m256, imm8               |      |                       |                   | in ymm2 and ymm3/mem using controls  
                                           |      |                       |                   | from imm8 and store result in ymm1.  

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVMI | ModRM:reg (w)| VEX.vvvv (r)| ModRM:r/m (r)| imm8     

### Description
Permute 128 bit floating-point-containing fields from the first source operand
(second operand) and second source operand (third operand) using bits in the
8-bit immediate and store results in the destination operand (first operand).
The first source operand is a YMM register, the second source operand is a YMM
register or a 256-bit memory location, and the destination operand is a YMM
register.

   | |  
---- | -----
 SRC2| Y1                            | Y0                                    
 SRC1| X1                            | X0                                    
 DEST| X0, X1, Y0, or Y1 Figure 4-42.| X0, X1, Y0, or Y1 VPERM2F128 Operation
Imm8[1:0] select the source for the first destination 128-bit field, imm8[5:4]
select the source for the second destination field. If imm8[3] is set, the low
128-bit field is zeroed. If imm8[7] is set, the high 128-bit field is zeroed.
VEX.L must be 1, otherwise the instruction will **``#UD.``**



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 0 If VEX.W = 1.
