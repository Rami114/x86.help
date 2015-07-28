## VEXTRACTI128  -  Extract packed Integer Values

> Operation
``` slim

VEXTRACTI128 (memory destination form)
CASE (imm8[0]) OF
  0: DEST[127:0] <- SRC1[127:0]
  1: DEST[127:0] <- SRC1[255:128]
ESAC.
VEXTRACTI128 (register destination form)
CASE (imm8[0]) OF
  0: DEST[127:0] <- SRC1[127:0]
  1: DEST[127:0] <- SRC1[255:128]
ESAC.
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                          
 ---  | --- | --- | --- | ---
 VEX.256.66.0F3A.W0 39 /r ib VEXTRACTI128| RMI  | V/V           | AVX2              | Extract 128 bits of integer data from
 xmm1/m128, ymm2, imm8                   |      |               |                   | ymm2 and store results in xmm1/mem.  

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:r/m (w)| ModRM:reg (r)| Imm8     | NA       

### Description
Extracts 128-bits of packed integer values from the source operand (second operand)
at a 128-bit offset from imm8[0] into the destination operand (first operand).
The destination may be either an XMM register or a 128-bit memory location.
VEX.vvvv is reserved and must be 1111b otherwise instructions will #UD. The
high 7 bits of the immediate are ignored. An attempt to execute VEXTRACTI128
encoded with VEX.L= 0 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 VEXTRACTI128:| __m128i _mm256_extracti128_si256(__m256i
              | a, int offset);                         

### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 #UD| IF VEX.L = 0, If VEX.W = 1.
