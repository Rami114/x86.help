## CVTDQ2PS - Convert Packed Dword Integers to Packed Single-Precision FP Values

> Operation
``` slim

CVTDQ2PS (128-bit Legacy SSE version)
DEST[31:0] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[31:0])
DEST[63:32] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[63:32])
DEST[95:64] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[95:64])
DEST[127:96] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[127z:96)
DEST[VLMAX-1:128] (unmodified)
VCVTDQ2PS (VEX.128 encoded version)
DEST[31:0] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[31:0])
DEST[63:32] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[63:32])
DEST[95:64] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[95:64])
DEST[127:96] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[127z:96)
DEST[VLMAX-1:128] <- 0
VCVTDQ2PS (VEX.256 encoded version)
DEST[31:0] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[31:0])
DEST[63:32] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[63:32])
DEST[95:64] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[95:64])
DEST[127:96] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[127z:96)
DEST[159:128] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[159:128])
DEST[191:160] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[191:160])
DEST[223:192] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[223:192])
DEST[255:224] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[255:224)

```

 Opcode/Instruction                  | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 0F 5B /r CVTDQ2PS xmm1, xmm2/m128   | RM   | V/V           | SSE2              | Convert four packed signed doubleword 
                                     |      |               |                   | integers from xmm2/m128 to four packed
                                     |      |               |                   | single-precision floating-point values
                                     |      |               |                   | in xmm1.                              
 VEX.128.0F.WIG 5B /r VCVTDQ2PS xmm1,| RM   | V/V           | AVX               | Convert four packed signed doubleword 
 xmm2/m128                           |      |               |                   | integers from xmm2/mem to four packed 
                                     |      |               |                   | single-precision floating-point values
                                     |      |               |                   | in xmm1.                              
 VEX.256.0F.WIG 5B /r VCVTDQ2PS ymm1,| RM   | V/V           | AVX               | Convert eight packed signed doubleword
 ymm2/m256                           |      |               |                   | integers from ymm2/mem to eight packed
                                     |      |               |                   | single-precision floating-point values
                                     |      |               |                   | in ymm1.                              

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts four packed signed doubleword integers in the source operand (second
operand) to four packed singleprecision floating-point values in the destination
operand (first operand).

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The source operand is an
XMM register or 128- bit memory location. The destination operation is an XMM
register. The upper bits (VLMAX-1:128) of the corresponding XMM register destination
are unmodified. VEX.128 encoded version: The source operand is an XMM register
or 128- bit memory location. The destination operation is an XMM register. The
upper bits (VLMAX-1:128) of the corresponding YMM register destination are zeroed.
VEX.256 encoded version: The source operand is a YMM register or 256- bit memory
location. The destination operation is a YMM register. Note: In VEX-encoded
versions, VEX.vvvv is reserved and must be 1111b, otherwise instructions will
#UD.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 CVTDQ2PS: | __m128 _mm_cvtepi32_ps(__m128i a)      
 VCVTDQ2PS:| __m256 _mm256_cvtepi32_ps (__m256i src)

### SIMD Floating-Point Exceptions
Precision.


### Other Exceptions
See Exceptions Type 2; additionally

   | |  
---- | -----
 #UD| If VEX.vvvv != 1111B.
