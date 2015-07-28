## CVTSI2SD - Convert Dword Integer to Scalar Double-Precision FP Value

> Operation
``` slim

CVTSI2SD
IF 64-Bit Mode And OperandSize = 64
THEN
  DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[63:0]);
ELSE
  DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[31:0]);
FI;
DEST[VLMAX-1:64] (Unmodified)
VCVTSI2SD
IF 64-Bit Mode And OperandSize = 64
THEN
  DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC2[63:0]);
ELSE
  DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC2[31:0]);
FI;
DEST[127:64] <- SRC1[127:64]
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                   | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                      
 ---  | --- | --- | --- | ---
 F2 0F 2A /r CVTSI2SD xmm, r/m32      | RM   | V/V           | SSE2              | Convert one signed doubleword integer            
                                      |      |               |                   | from r/m32 to one double-precision floating-point
                                      |      |               |                   | value in xmm.                                    
 F2 REX.W 0F 2A /r CVTSI2SD xmm, r/m64| RM   | V/N.E.        | SSE2              | Convert one signed quadword integer              
                                      |      |               |                   | from r/m64 to one double-precision floating-point
                                      |      |               |                   | value in xmm.                                    
 VEX.NDS.LIG.F2.0F.W0 2A /r VCVTSI2SD | RVM  | V/V           | AVX               | Convert one signed doubleword integer            
 xmm1, xmm2, r/m32                    |      |               |                   | from r/m32 to one double-precision floating-point
                                      |      |               |                   | value in xmm1.                                   
 VEX.NDS.LIG.F2.0F.W1 2A /r VCVTSI2SD | RVM  | V/N.E.1       | AVX               | Convert one signed quadword integer              
 xmm1, xmm2, r/m64                    |      |               |                   | from r/m64 to one double-precision floating-point
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
size is 64 bits) in the second source operand to a double-precision floating-point
value in the destination operand. The result is stored in the low quadword of
the destination operand, and the high quadword left unchanged. When conversion
is inexact, the value returned is rounded according to the rounding control
bits in the MXCSR register. Legacy SSE instructions: Use of the REX.W prefix
promotes the instruction to 64-bit operands. See the summary chart at the beginning
of this section for encoding data and limits. The second source operand can
be a general-purpose register or a 32/64-bit memory location. The first source
and destination operands are XMM registers. 128-bit Legacy SSE version: The
destination and first source operand are the same. Bits (VLMAX-1:64) of the
### corresponding YMM destination register remain unchanged. VEX.128 encoded version
Bits (127:64) of the XMM register destination are copied from corresponding
bits in the first source operand. Bits (VLMAX-1:128) of the destination YMM
register are zeroed.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 CVTSI2SD:| __m128d _mm_cvtsi32_sd(__m128d a, int    
          | b)                                       
 CVTSI2SD:| __m128d _mm_cvtsi64_sd(__m128d a, __int64
          | b)                                       

### SIMD Floating-Point Exceptions
Precision.


### Other Exceptions
See Exceptions Type 3.
