## CVTPS2DQ - Convert Packed Single-Precision FP Values to Packed Dword Integers

> Operation

``` slim
CVTPS2DQ (128-bit Legacy SSE version)
DEST[31:0] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[31:0])
DEST[63:32] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[63:32])
DEST[95:64] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[95:64])
DEST[127:96] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[127:96])
DEST[VLMAX-1:128] (unmodified)
VCVTPS2DQ (VEX.128 encoded version)
DEST[31:0] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[31:0])
DEST[63:32] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[63:32])
DEST[95:64] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[95:64])
DEST[127:96] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[127:96])
DEST[VLMAX-1:128] <- 0
VCVTPS2DQ (VEX.256 encoded version)
DEST[31:0] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[31:0])
DEST[63:32] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[63:32])
DEST[95:64] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[95:64])
DEST[127:96] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[127:96)
DEST[159:128] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[159:128])
DEST[191:160] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[191:160])
DEST[223:192] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[223:192])
DEST[255:224] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[255:224])

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 CVTPS2DQ: | __m128i _mm_cvtps_epi32(__m128 a)  
 VCVTPS2DQ:| __ m256i _mm256_cvtps_epi32 (__m256
           | a)                                 

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 66 0F 5B /r CVTPS2DQ xmm1, xmm2/m128   | RM   | V/V           | SSE2              | Convert four packed single-precision     
                                        |      |               |                   | floatingpoint values from xmm2/m128      
                                        |      |               |                   | to four packed signed doubleword integers
                                        |      |               |                   | in xmm1.                                 
 VEX.128.66.0F.WIG 5B /r VCVTPS2DQ xmm1,| RM   | V/V           | AVX               | Convert four packed single precision     
 xmm2/m128                              |      |               |                   | floatingpoint values from xmm2/mem to    
                                        |      |               |                   | four packed signed doubleword values     
                                        |      |               |                   | in xmm1.                                 
 VEX.256.66.0F.WIG 5B /r VCVTPS2DQ ymm1,| RM   | V/V           | AVX               | Convert eight packed single precision    
 ymm2/m256                              |      |               |                   | floatingpoint values from ymm2/mem to    
                                        |      |               |                   | eight packed signed doubleword values    
                                        |      |               |                   | in ymm1.                                 

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts four or eight packed single-precision floating-point values in the
source operand to four or eight signed doubleword integers in the destination
operand.

When a conversion is inexact, the value returned is rounded according to the
rounding control bits in the MXCSR register. If a converted result is larger
than the maximum signed doubleword integer, the floating-point invalid exception
is raised, and if this exception is masked, the indefinite integer value (80000000H)
is returned.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The source operand is an
XMM register or 128- bit memory location. The destination operation is an XMM
register. The upper bits (VLMAX-1:128) of the corresponding YMM register destination
are unmodified. VEX.128 encoded version: The source operand is an XMM register
or 128- bit memory location. The destination operation is a YMM register. The
upper bits (VLMAX-1:128) of the corresponding YMM register destination are zeroed.
VEX.256 encoded version: The source operand is a YMM register or 256- bit memory
location. The destination operation is a YMM register. Note: In VEX-encoded
versions, VEX.vvvv is reserved and must be 1111b otherwise instructions will
**``#UD.``**



### SIMD Floating-Point Exceptions
Invalid, Precision.


### Other Exceptions
See Exceptions Type 2; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
