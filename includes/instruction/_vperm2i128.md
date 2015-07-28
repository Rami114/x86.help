## VPERM2I128  -  Permute Integer Values

> Operation
``` slim

VPERM2I128
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
DEST[255:128] <- 0
FI

```

 Opcode/Instruction                        | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                          
 ---  | --- | --- | --- | ---
 VEX.NDS.256.66.0F3A.W0 46 /r ib VPERM2I128| RVMI | V/V            | AVX2              | Permute 128-bit integer data in ymm2 
 ymm1, ymm2, ymm3/m256, imm8               |      |                |                   | and ymm3/mem using controls from imm8
                                           |      |                |                   | and store result in ymm1.            

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVMI | ModRM:reg (w)| VEX.vvvv | ModRM:r/m (r)| Imm8     

### Description
Permute 128 bit integer data from the first source operand (second operand)
and second source operand (third operand) using bits in the 8-bit immediate
and store results in the destination operand (first operand). The first source
operand is a YMM register, the second source operand is a YMM register or a
256-bit memory location, and the destination operand is a YMM register.

   | |  
---- | -----
 SRC2| Y1                            | Y0                                    
 SRC1| X1                            | X0                                    
 DEST| X0, X1, Y0, or Y1 Figure 4-37.| X0, X1, Y0, or Y1 VPERM2I128 Operation
Imm8[1:0] select the source for the first destination 128-bit field, imm8[5:4]
select the source for the second destination field. If imm8[3] is set, the low
128-bit field is zeroed. If imm8[7] is set, the high 128-bit field is zeroed.
VEX.L must be 1, otherwise the instruction will #UD.



### Intel C/C++ Compiler Intrinsic Equivalent
VPERM2I128: __m256i _mm256_permute2x128_si256 (__m256i a, __m256i b, int control)


### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 #UD| If VEX.L = 0, If VEX.W = 1.
