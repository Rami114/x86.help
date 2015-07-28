## CVTTPD2DQ - Convert with Truncation Packed Double-Precision FP Values to Packed Dword Integers

> Operation

``` slim
CVTTPD2DQ (128-bit Legacy SSE version)
DEST[31:0] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[63:0])
DEST[63:32] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[127:64])
DEST[127:64] <- 0
DEST[VLMAX-1:128] (unmodified)
VCVTTPD2DQ (VEX.128 encoded version)
DEST[31:0] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[63:0])
DEST[63:32] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[127:64])
DEST[VLMAX-1:64] <- 0
VCVTTPD2DQ (VEX.256 encoded version)
DEST[31:0] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[63:0])
DEST[63:32] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[127:64])
DEST[95:64] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[191:128])
DEST[127:96] <- Convert_Double_Precision_Floating_Point_To_Integer_Truncate(SRC[255:192)
DEST[255:128]<- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 CVTTPD2DQ: | __m128i _mm_cvttpd_epi32(__m128d a) 
 VCVTTPD2DQ:| __m128i _mm256_cvttpd_epi32 (__m256d
            | src)                                

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 66 0F E6 /r CVTTPD2DQ xmm1, xmm2/m128   | RM   | V/V           | SSE2              | Convert two packed double-precision     
                                         |      |               |                   | floatingpoint values from xmm2/m128     
                                         |      |               |                   | to two packed signed doubleword integers
                                         |      |               |                   | in xmm1 using truncation.               
 VEX.128.66.0F.WIG E6 /r VCVTTPD2DQ xmm1,| RM   | V/V           | AVX               | Convert two packed double-precision     
 xmm2/m128                               |      |               |                   | floatingpoint values in xmm2/mem to     
                                         |      |               |                   | two signed doubleword integers in xmm1  
                                         |      |               |                   | using truncation.                       
 VEX.256.66.0F.WIG E6 /r VCVTTPD2DQ xmm1,| RM   | V/V           | AVX               | Convert four packed double-precision    
 ymm2/m256                               |      |               |                   | floatingpoint values in ymm2/mem to     
                                         |      |               |                   | four signed doubleword integers in xmm1 
                                         |      |               |                   | using truncation.                       

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts two or four packed double-precision floating-point values in the source
operand (second operand) to two or four packed signed doubleword integers in
the destination operand (first operand). When a conversion is inexact, a truncated
(round toward zero) value is returned.If a converted result is larger than the
maximum signed doubleword integer, the floating-point invalid exception is raised,
and if this exception is masked, the indefinite integer value (80000000H) is
returned.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The source operand is an
XMM register or 128- bit memory location. The destination operation is an XMM
register. The upper bits (VLMAX-1:128) of the corresponding YMM register destination
are unmodified. VEX.128 encoded version: The source operand is an XMM register
or 128- bit memory location. The destination operation is a YMM register. The
upper bits (VLMAX-1:128) of the corresponding YMM register destination are zeroed.
VEX.256 encoded version: The source operand is a YMM register or 256- bit memory
location. The destination operation is an XMM register. The upper bits (255:128)
of the corresponding YMM register destination are zeroed. Note: In VEX-encoded
versions, VEX.vvvv is reserved and must be 1111b, otherwise instructions will
**``#UD.``**

   | |  
---- | -----
 SRC DEST| X3 Figure 3-14.| X2 0 VCVTTPD2DQ (VEX.256 encoded version)| X1 X2| X0 X0


### SIMD Floating-Point Exceptions
Invalid, Precision.


### Other Exceptions
See Exceptions Type 2; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
