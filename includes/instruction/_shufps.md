## SHUFPS - Shuffle Packed Single-Precision Floating-Point Values

> Operation
``` slim

CASE (SELECT[1:0]) OF
```

 Opcode\*/Instruction                | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                   
 ---  | --- | --- | --- | ---
 0F C6 /r ib SHUFPS xmm1, xmm2/m128,| RMI  | V/V                   | SSE               | Shuffle packed single-precision floating-point
 imm8                               |      |                       |                   | values selected by imm8 from xmm1 and         
                                    |      |                       |                   | xmm1/m128 to xmm1.                            
 VEX.NDS.128.0F.WIG C6 /r ib VSHUFPS| RVMI | V/V                   | AVX               | Shuffle Packed single-precision floating-point
 xmm1, xmm2, xmm3/m128, imm8        |      |                       |                   | values selected by imm8 from xmm2 and         
                                    |      |                       |                   | xmm3/mem.                                     
 VEX.NDS.256.0F.WIG C6 /r ib VSHUFPS| RVMI | V/V                   | AVX               | Shuffle Packed single-precision floating-point
 ymm1, ymm2, ymm3/m256, imm8        |      |                       |                   | values selected by imm8 from ymm2 and         
                                    |      |                       |                   | ymm3/mem.                                     

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
Moves two of the four packed single-precision floating-point values from the
destination operand (first operand) into the low quadword of the destination
operand; moves two of the four packed single-precision floating-point values
from the source operand (second operand) into to the high quadword of the destination
operand (see Figure 4-22). The select operand (third operand) determines which
values are moved to the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
source can be an XMM register or an 128-bit memory location. The destination
is not distinct from the first source XMM register and the upper bits (VLMAX-1:128)
of the corresponding YMM register destination are unmodified. VEX.128 encoded
version: the first source operand is an XMM register or 128-bit memory location.
The destination operand is an XMM register. The upper bits (VLMAX-1:128) of
the corresponding YMM register destination are zeroed. determines which values
are moved to the destination operand.

VEX.256 encoded version: The first source operand is a YMM register. The second
source operand can be a YMM register or a 256-bit memory location. The destination
operand is a YMM register.

   | |  
---- | -----
 DEST| X3                    | X2                                | X1       | X0       
 SRC | Y3                    | Y2                                | Y1       | Y0       
 DEST| Y3 ... Y0 Figure 4-22.| Y3 ... Y0 SHUFPS Shuffle Operation| X3 ... X0| X3 ... X0
The source operand can be an XMM register or a 128-bit memory location. The
### destination operand is an XMM register. The select operand is an 8-bit immediate
bits 0 and 1 select the value to be moved from the destination operand to the
low doubleword of the result, bits 2 and 3 select the value to be moved from
the destination operand to the second doubleword of the result, bits 4 and 5
select the value to be moved from the source operand to the third doubleword
of the result, and bits 6 and 7 select the value to be moved from the source
operand to the high doubleword of the result.



###   0
###   1
###   2
###   3
ESAC;
CASE (SELECT[3:2]) OF
###   0
###   1
###   2
###   3
ESAC;
CASE (SELECT[5:4]) OF
###   0
###   1
###   2
###   3
ESAC;
CASE (SELECT[7:6]) OF
###   0
###   1
###   2
###   3
ESAC;
SHUFPS (128-bit Legacy SSE version)
DEST[31:0] <- Select4(SRC1[127:0], imm8[1:0]);
DEST[63:32] <- Select4(SRC1[127:0], imm8[3:2]);
DEST[95:64] <- Select4(SRC2[127:0], imm8[5:4]);
DEST[127:96] <- Select4(SRC2[127:0], imm8[7:6]);
DEST[VLMAX-1:128] (Unmodified)
VSHUFPS (VEX.128 encoded version)
DEST[31:0] <- Select4(SRC1[127:0], imm8[1:0]);
DEST[63:32] <- Select4(SRC1[127:0], imm8[3:2]);
DEST[95:64] <- Select4(SRC2[127:0], imm8[5:4]);
DEST[127:96] <- Select4(SRC2[127:0], imm8[7:6]);
DEST[VLMAX-1:128] <- 0
VSHUFPS (VEX.256 encoded version)
DEST[31:0] <- Select4(SRC1[127:0], imm8[1:0]);
DEST[63:32] <- Select4(SRC1[127:0], imm8[3:2]);
DEST[95:64] <- Select4(SRC2[127:0], imm8[5:4]);
DEST[127:96] <- Select4(SRC2[127:0], imm8[7:6]);
DEST[159:128] <- Select4(SRC1[255:128], imm8[1:0]);
DEST[191:160] <- Select4(SRC1[255:128], imm8[3:2]);
DEST[223:192] <- Select4(SRC2[255:128], imm8[5:4]);
DEST[255:224] <- Select4(SRC2[255:128], imm8[7:6]);

### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 SHUFPS: | __m128 _mm_shuffle_ps(__m128 a, __m128
         | b, unsigned int imm8)                 
 VSHUFPS:| __m256 _mm256_shuffle_ps (__m256 a,   
         | __m256 b, const int select);          

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4.
