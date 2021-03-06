## CVTPD2PS - Convert Packed Double-Precision FP Values to Packed Single-Precision FP Values

> Operation

``` slim
CVTPD2PS (128-bit Legacy SSE version)
DEST[31:0] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[63:0])
DEST[63:32] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[127:64])
DEST[127:64] <- 0
DEST[VLMAX-1:128] (unmodified)
VCVTPD2PS (VEX.128 encoded version)
DEST[31:0] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[63:0])
DEST[63:32] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[127:64])
DEST[VLMAX-1:64] <- 0
VCVTPD2PS (VEX.256 encoded version)
DEST[31:0] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[63:0])
DEST[63:32] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[127:64])
DEST[95:64] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[191:128])
DEST[127:96] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[255:192)
DEST[255:128]<- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 CVTPD2PS:| __m128 _mm_cvtpd_ps(__m128d a)    
 CVTPD2PS:| __m256 _mm256_cvtpd_ps (__m256d a)

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 66 0F 5A /r CVTPD2PS xmm1, xmm2/m128   | RM   | V/V           | SSE2              | Convert two packed double-precision       
                                        |      |               |                   | floatingpoint values in xmm2/m128 to      
                                        |      |               |                   | two packed single-precision floating-point
                                        |      |               |                   | values in xmm1.                           
 VEX.128.66.0F.WIG 5A /r VCVTPD2PS xmm1,| RM   | V/V           | AVX               | Convert two packed double-precision       
 xmm2/m128                              |      |               |                   | floatingpoint values in xmm2/mem to       
                                        |      |               |                   | two singleprecision floating-point values 
                                        |      |               |                   | in xmm1.                                  
 VEX.256.66.0F.WIG 5A /r VCVTPD2PS xmm1,| RM   | V/V           | AVX               | Convert four packed double-precision      
 ymm2/m256                              |      |               |                   | floatingpoint values in ymm2/mem to       
                                        |      |               |                   | four singleprecision floating-point       
                                        |      |               |                   | values in xmm1.                           

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts two packed double-precision floating-point values in the source operand
(second operand) to two packed single-precision floating-point values in the
destination operand (first operand). When a conversion is inexact, the value
returned is rounded according to the rounding control bits in the MXCSR register.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The source operand is an
XMM register or 128- bit memory location. The destination operation is an XMM
register. Bits[127:64] of the destination XMM register are zeroed. However,
the upper bits (VLMAX-1:128) of the corresponding YMM register destination are
unmodified. VEX.128 encoded version: The source operand is an XMM register or
128- bit memory location. The destination operation is a YMM register. The upper
bits (VLMAX-1:64) of the corresponding YMM register destination are zeroed.
VEX.256 encoded version: The source operand is a YMM register or 256- bit memory
location. The destination operation is an XMM register. The upper bits (255:128)
of the corresponding YMM register destination are zeroed. Note: In VEX-encoded
versions, VEX.vvvv is reserved and must be 1111b otherwise instructions will
**``#UD.``**

   | |  
---- | -----
 SRC DEST| X3 Figure 3-12.| X2 0 VCVTPD2PS (VEX.256 encoded version)| X1 X2| X0 X0


### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 2; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
