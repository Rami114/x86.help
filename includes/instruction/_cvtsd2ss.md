## CVTSD2SS - Convert Scalar Double-Precision FP Value to Scalar Single-Precision FP Value

> Operation

``` slim
CVTSD2SS (128-bit Legacy SSE version)
DEST[31:0] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC[63:0]);
(\* DEST[VLMAX-1:32] Unmodified \*)
VCVTSD2SS (VEX.128 encoded version)
DEST[31:0] <- Convert_Double_Precision_To_Single_Precision_Floating_Point(SRC2[63:0]);
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                   | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 F2 0F 5A /r CVTSD2SS xmm1, xmm2/m64  | RM   | V/V           | SSE2              | Convert one double-precision floating-point
                                      |      |               |                   | value in xmm2/m64 to one single-precision  
                                      |      |               |                   | floating-point value in xmm1.              
 VEX.NDS.LIG.F2.0F.WIG 5A /r VCVTSD2SS| RVM  | V/V           | AVX               | Convert one double-precision floating-point
 xmm1,xmm2, xmm3/m64                  |      |               |                   | value in xmm3/m64 to one single-precision  
                                      |      |               |                   | floating-point value and merge with        
                                      |      |               |                   | high bits in xmm2.                         

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Converts a double-precision floating-point value in the source operand (second
operand) to a single-precision floating-point value in the destination operand
(first operand).

The source operand can be an XMM register or a 64-bit memory location. The destination
operand is an XMM register. When the source operand is an XMM register, the
double-precision floating-point value is contained in the low quadword of the
register. The result is stored in the low doubleword of the destination operand,
and the upper 3 doublewords are left unchanged. When the conversion is inexact,
the value returned is rounded according to the rounding control bits in the
MXCSR register.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The destination and first
source operand are the same. Bits (VLMAX-1:32) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: Bits (127:64) of the XMM
register destination are copied from corresponding bits in the first source
operand. Bits (VLMAX-1:128) of the destination YMM register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 CVTSD2SS:| __m128 _mm_cvtsd_ss(__m128 a, __m128d
          | b)                                   

### SIMD Floating-Point Exceptions
Overflow, Underflow, Invalid, Precision, Denormal.


### Other Exceptions
See Exceptions Type 3.
