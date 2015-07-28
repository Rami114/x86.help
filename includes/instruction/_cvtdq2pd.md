## CVTDQ2PD - Convert Packed Dword Integers to Packed Double-Precision FP Values

> Operation

``` slim
CVTDQ2PD (128-bit Legacy SSE version)
DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[31:0])
DEST[127:64] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[63:32])
DEST[VLMAX-1:128] (unmodified)
VCVTDQ2PD (VEX.128 encoded version)
DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[31:0])
DEST[127:64] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[63:32])
DEST[VLMAX-1:128] <- 0
VCVTDQ2PD (VEX.256 encoded version)
DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[31:0])
DEST[127:64] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[63:32])
DEST[191:128] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[95:64])
DEST[255:192] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[127:96)

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 CVTDQ2PD: | __m128d _mm_cvtepi32_pd(__m128i a) 
 VCVTDQ2PD:| __m256d _mm256_cvtepi32_pd (__m128i
           | src)                               

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 F3 0F E6 CVTDQ2PD xmm1, xmm2/m64       | RM   | V/V           | SSE2              | Convert two packed signed doubleword  
                                        |      |               |                   | integers from xmm2/m128 to two packed 
                                        |      |               |                   | double-precision floating-point values
                                        |      |               |                   | in xmm1.                              
 VEX.128.F3.0F.WIG E6 /r VCVTDQ2PD xmm1,| RM   | V/V           | AVX               | Convert two packed signed doubleword  
 xmm2/m64                               |      |               |                   | integers from xmm2/mem to two packed  
                                        |      |               |                   | double-precision floating-point values
                                        |      |               |                   | in xmm1.                              
 VEX.256.F3.0F.WIG E6 /r VCVTDQ2PD ymm1,| RM   | V/V           | AVX               | Convert four packed signed doubleword 
 xmm2/m128                              |      |               |                   | integers from xmm2/mem to four packed 
                                        |      |               |                   | double-precision floating-point values
                                        |      |               |                   | in ymm1.                              

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts two packed signed doubleword integers in the source operand (second
operand) to two packed doubleprecision floating-point values in the destination
operand (first operand).

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The source operand is an
XMM register or 64- bit memory location. The destination operation is an XMM
register. The upper bits (VLMAX-1:128) of the corresponding XMM register destination
are unmodified. VEX.128 encoded version: The source operand is an XMM register
or 64- bit memory location. The destination operation is an XMM register. The
upper bits (VLMAX-1:128) of the corresponding YMM register destination are zeroed.
VEX.256 encoded version: The source operand is a YMM register or 128- bit memory
location. The destination operation is a YMM register. Note: In VEX-encoded
versions, VEX.vvvv is reserved and must be 1111b, otherwise instructions will
**``#UD.``**

   | |  
---- | -----
 SRC X3| X3 X2| X2 X1| X1| X0 X0
DEST

   | |  
---- | -----
 Figure 3-10.| CVTDQ2PD (VEX.256 encoded version)


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
