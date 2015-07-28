## CVTSS2SD - Convert Scalar Single-Precision FP Value to Scalar Double-Precision FP Value

> Operation
``` slim

CVTSS2SD (128-bit Legacy SSE version)
DEST[63:0] <- Convert_Single_Precision_To_Double_Precision_Floating_Point(SRC[31:0]);
DEST[VLMAX-1:64] (Unmodified)
VCVTSS2SD (VEX.128 encoded version)
DEST[63:0] <- Convert_Single_Precision_To_Double_Precision_Floating_Point(SRC2[31:0])
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                   | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 F3 0F 5A /r CVTSS2SD xmm1, xmm2/m32  | RM   | V/V           | SSE2              | Convert one single-precision floating-point
                                      |      |               |                   | value in xmm2/m32 to one double-precision  
                                      |      |               |                   | floating-point value in xmm1.              
 VEX.NDS.LIG.F3.0F.WIG 5A /r VCVTSS2SD| RVM  | V/V           | AVX               | Convert one single-precision floating-point
 xmm1, xmm2, xmm3/m32                 |      |               |                   | value in xmm3/m32 to one double-precision  
                                      |      |               |                   | floating-point value and merge with        
                                      |      |               |                   | high bits of xmm2.                         

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Converts a single-precision floating-point value in the source operand (second
operand) to a double-precision floating-point value in the destination operand
(first operand). The source operand can be an XMM register or a 32bit memory
location. The destination operand is an XMM register. When the source operand
is an XMM register, the single-precision floating-point value is contained in
the low doubleword of the register. The result is stored in the low quadword
of the destination operand, and the high quadword is left unchanged.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). 128-bit Legacy SSE version: The destination and first
source operand are the same. Bits (VLMAX-1:64) of the corresponding YMM destination
register remain unchanged. VEX.128 encoded version: Bits (127:64) of the XMM
register destination are copied from corresponding bits in the first source
operand. Bits (VLMAX-1:128) of the destination YMM register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 CVTSS2SD:| __m128d _mm_cvtss_sd(__m128d a, __m128
          | b)                                    

### SIMD Floating-Point Exceptions
Invalid, Denormal.


### Other Exceptions
See Exceptions Type 3.
