## CVTSI2SS - Convert Dword Integer to Scalar Single-Precision FP Value

> Operation
``` slim

CVTSI2SS (128-bit Legacy SSE version)
IF 64-Bit Mode And OperandSize = 64
THEN
  DEST[31:0] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[63:0]);
ELSE
  DEST[31:0] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[31:0]);
FI;
DEST[VLMAX-1:32] (Unmodified)
VCVTSI2SS (VEX.128 encoded version)
IF 64-Bit Mode And OperandSize = 64
THEN
  DEST[31:0] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[63:0]);
ELSE
  DEST[31:0] <- Convert_Integer_To_Single_Precision_Floating_Point(SRC[31:0]);
FI;
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                   | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                      
 ---  | --- | --- | --- | ---
 F3 0F 2A /r CVTSI2SS xmm, r/m32      | RM   | V/V           | SSE               | Convert one signed doubleword integer            
                                      |      |               |                   | from r/m32 to one single-precision floating-point
                                      |      |               |                   | value in xmm.                                    
 F3 REX.W 0F 2A /r CVTSI2SS xmm, r/m64| RM   | V/N.E.        | SSE               | Convert one signed quadword integer              
                                      |      |               |                   | from r/m64 to one single-precision floating-point
                                      |      |               |                   | value in xmm.                                    
 VEX.NDS.LIG.F3.0F.W0 2A /r VCVTSI2SS | RVM  | V/V           | AVX               | Convert one signed doubleword integer            
 xmm1, xmm2, r/m32                    |      |               |                   | from r/m32 to one single-precision floating-point
                                      |      |               |                   | value in xmm1.                                   
 VEX.NDS.LIG.F3.0F.W1 2A /r VCVTSI2SS | RVM  | V/N.E.1       | AVX               | Convert one signed quadword integer              
 xmm1, xmm2, r/m64                    |      |               |                   | from r/m64 to one single-precision floating-point
                                      |      |               |                   | value in xmm1.                                   
<aside class="notification">
1. Encoding the VEX prefix with VEX.W=1 in non-64-bit mode is ignored.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Converts a signed doubleword integer (or signed quadword integer if operand
size is 64 bits) in the source operand (second operand) to a single-precision
floating-point value in the destination operand (first operand). The source
operand can be a general-purpose register or a memory location. The destination
operand is an XMM register. The result is stored in the low doubleword of the
destination operand, and the upper three doublewords are left unchanged. When
a conversion is inexact, the value returned is rounded according to the rounding
control bits in the MXCSR register.

Legacy SSE instructions: In 64-bit mode, the instruction can access additional
registers (XMM8-XMM15, R8-R15) when used with a REX.R prefix. Use of the REX.W
prefix promotes the instruction to 64-bit operands. See the summary chart at
the beginning of this section for encoding data and limits.

128-bit Legacy SSE version: The destination and first source operand are the
same. Bits (VLMAX-1:32) of the corresponding YMM destination register remain
unchanged. VEX.128 encoded version: Bits (127:32) of the XMM register destination
are copied from corresponding bits in the first source operand. Bits (VLMAX-1:128)
of the destination YMM register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 CVTSI2SS:| __m128| _mm_cvtsi32_ss(__m128| a, int b)    
 CVTSI2SS:| __m128| _mm_cvtsi64_ss(__m128| a, __int64 b)

### SIMD Floating-Point Exceptions
Precision.


### Other Exceptions
See Exceptions Type 3.
