## INSERTPS  -  Insert Packed Single Precision Floating-Point Value

> Operation
``` slim

INSERTPS (128-bit Legacy SSE version)
IF (SRC = REG) THEN COUNT_S <- imm8[7:6]
  ELSE COUNT_S <- 0
COUNT_D <- imm8[5:4]
ZMASK <- imm8[3:0]
CASE (COUNT_S) OF
  0: TMP <- SRC[31:0]
  1: TMP <- SRC[63:32]
  2: TMP <- SRC[95:64]
  3: TMP <- SRC[127:96]
ESAC;
CASE (COUNT_D) OF
  0: TMP2[31:0] <- TMP
     TMP2[127:32] <- DEST[127:32]
  1: TMP2[63:32] <- TMP
     TMP2[31:0] <- DEST[31:0]
     TMP2[127:64] <- DEST[127:64]
  2: TMP2[95:64] <- TMP
     TMP2[63:0] <- DEST[63:0]
     TMP2[127:96] <- DEST[127:96]
  3: TMP2[127:96] <- TMP
     TMP2[95:0] <- DEST[95:0]
ESAC;
IF (ZMASK[0] = 1) THEN DEST[31:0] <- 00000000H
  ELSE DEST[31:0] <- TMP2[31:0]
IF (ZMASK[1] = 1) THEN DEST[63:32] <- 00000000H
  ELSE DEST[63:32] <- TMP2[63:32]
IF (ZMASK[2] = 1) THEN DEST[95:64] <- 00000000H
  ELSE DEST[95:64] <- TMP2[95:64]
IF (ZMASK[3] = 1) THEN DEST[127:96] <- 00000000H
  ELSE DEST[127:96] <- TMP2[127:96]
DEST[VLMAX-1:128] (Unmodified)
VINSERTPS (VEX.128 encoded version)
IF (SRC = REG) THEN COUNT_S <- imm8[7:6]
  ELSE COUNT_S <- 0
COUNT_D <- imm8[5:4]
ZMASK <- imm8[3:0]
CASE (COUNT_S) OF
  0: TMP <- SRC2[31:0]
  1: TMP <- SRC2[63:32]
  2: TMP <- SRC2[95:64]
  3: TMP <- SRC2[127:96]
ESAC;
CASE (COUNT_D) OF
  0: TMP2[31:0] <- TMP
     TMP2[127:32] <- SRC1[127:32]
  1: TMP2[63:32] <- TMP
     TMP2[31:0] <- SRC1[31:0]
     TMP2[127:64] <- SRC1[127:64]
  2: TMP2[95:64] <- TMP
     TMP2[63:0] <- SRC1[63:0]
     TMP2[127:96] <- SRC1[127:96]
  3: TMP2[127:96] <- TMP
     TMP2[95:0] <- SRC1[95:0]
ESAC;
IF (ZMASK[0] = 1) THEN DEST[31:0] <- 00000000H
  ELSE DEST[31:0] <- TMP2[31:0]
IF (ZMASK[1] = 1) THEN DEST[63:32] <- 00000000H
  ELSE DEST[63:32] <- TMP2[63:32]
IF (ZMASK[2] = 1) THEN DEST[95:64] <- 00000000H
  ELSE DEST[95:64] <- TMP2[95:64]
IF (ZMASK[3] = 1) THEN DEST[127:96] <- 00000000H
  ELSE DEST[127:96] <- TMP2[127:96]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                        | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 66 0F 3A 21 /r ib INSERTPS xmm1, xmm2/m32,| RMI  | V/V           | SSE4_1            | Insert a single precision floating-point 
 imm8                                      |      |               |                   | value selected by imm8 from xmm2/m32     
                                           |      |               |                   | into xmm1 at the specified destination   
                                           |      |               |                   | element specified by imm8 and zero out   
                                           |      |               |                   | destination elements in xmm1 as indicated
                                           |      |               |                   | in imm8.                                 
 VEX.NDS.128.66.0F3A.WIG 21 /r ib VINSERTPS| RVMI | V/V           | AVX               | Insert a single precision floating point 
 xmm1, xmm2, xmm3/m32, imm8                |      |               |                   | value selected by imm8 from xmm3/m32     
                                           |      |               |                   | and merge into xmm2 at the specified     
                                           |      |               |                   | destination element specified by imm8    
                                           |      |               |                   | and zero out destination elements in     
                                           |      |               |                   | xmm1 as indicated in imm8.               

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
(register source form) Select a single precision floating-point element from
second source as indicated by Count_S bits of the immediate operand and insert
it into the first source at the location indicated by the Count_D bits of the
immediate operand. Store in the destination and zero out destination elements
based on the ZMask bits of the immediate operand.

(memory source form) Load a floating-point element from a 32-bit memory location
and insert it into the first source at the location indicated by the Count_D
bits of the immediate operand. Store in the destination and zero out destination
elements based on the ZMask bits of the immediate operand.

128-bit Legacy SSE version: The first source register is an XMM register. The
second source operand is either an XMM register or a 32-bit memory location.
The destination is not distinct from the first source XMM register and the upper
bits (VLMAX-1:128) of the corresponding YMM register destination are unmodified.
VEX.128 encoded version. The destination and first source register is an XMM
register. The second source operand is either an XMM register or a 32-bit memory
location. The upper bits (VLMAX-1:128) of the corresponding YMM register destination
are zeroed. If VINSERTPS is encoded with VEX.L= 1, an attempt to execute the
instruction encoded with VEX.L= 1 will cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 INSERTPS:| __m128 _mm_insert_ps(__m128 dst, __m128
          | src, const int ndx);                   

### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 5.
