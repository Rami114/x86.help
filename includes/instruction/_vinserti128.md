## VINSERTI128  -  Insert Packed Integer Values

> Operation

``` slim
VINSERTI128
TEMP[255:0] <- SRC1[255:0]
CASE (imm8[0]) OF
  0: TEMP[127:0] <-SRC2[127:0]
  1: TEMP[255:128] <- SRC2[127:0]
ESAC
DEST <-TEMP

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
VINSERTI128: __m256i _mm256_inserti128_si256 (__m256i a, __m128i b, int offset);


```

 Opcode/Instruction                         | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 VEX.NDS.256.66.0F3A.W0 38 /r ib VINSERTI128| RVMI | V/V            | AVX2              | Insert 128-bits of integer data from  
 ymm1, ymm2, xmm3/m128, imm8                |      |                |                   | xmm3/mem and the remaining values from
                                            |      |                |                   | ymm2 into ymm1.                       

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVMI | ModRM:reg (w)| VEX.vvvv | ModRM:r/m (r)| Imm8     

### Description
Performs an insertion of 128-bits of packed integer data from the second source
operand (third operand) into an the destination operand (first operand) at a
128-bit offset from imm8[0]. The remaining portions of the destination are written
by the corresponding fields of the first source operand (second operand). The
second source operand can be either an XMM register or a 128-bit memory location.
The high 7 bits of the immediate are ignored. VEX.L must be 1; an attempt to
execute this instruction with VEX.L=0 will cause **``#UD.``**



### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 0, If VEX.W = 1.
