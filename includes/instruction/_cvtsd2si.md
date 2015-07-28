## CVTSD2SI - Convert Scalar Double-Precision FP Value to Integer

> Operation

``` slim
IF 64-Bit Mode and OperandSize = 64
  THEN
     DEST[63:0] <- Convert_Double_Precision_Floating_Point_To_Integer64(SRC[63:0]);
  ELSE
     DEST[31:0] <- Convert_Double_Precision_Floating_Point_To_Integer32(SRC[63:0]);
FI;

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
int _mm_cvtsd_si32(__m128d a) __int64 _mm_cvtsd_si64(__m128d a)


```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 F2 0F 2D /r CVTSD2SI r32, xmm/m64      | RM   | V/V           | SSE2              | Convert one double-precision floating-point 
                                        |      |               |                   | value from xmm/m64 to one signed doubleword 
                                        |      |               |                   | integer r32.                                
 F2 REX.W 0F 2D /r CVTSD2SI r64, xmm/m64| RM   | V/N.E.        | SSE2              | Convert one double-precision floating-point 
                                        |      |               |                   | value from xmm/m64 to one signed quadword   
                                        |      |               |                   | integer sign-extended into r64.             
 VEX.LIG.F2.0F.W0 2D /r VCVTSD2SI r32,  | RM   | V/V           | AVX               | Convert one double precision floating-point 
 xmm1/m64                               |      |               |                   | value from xmm1/m64 to one signed doubleword
                                        |      |               |                   | integer r32.                                
 VEX.LIG.F2.0F.W1 2D /r VCVTSD2SI r64,  | RM   | V/N.E.1       | AVX               | Convert one double precision floating-point 
 xmm1/m64                               |      |               |                   | value from xmm1/m64 to one signed quadword  
                                        |      |               |                   | integer sign-extended into r64.             
<aside class="notification">
1. Encoding the VEX prefix with VEX.W=1 in non-64-bit mode is ignored.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts a double-precision floating-point value in the source operand (second
operand) to a signed doubleword integer in the destination operand (first operand).
The source operand can be an XMM register or a 64-bit memory location. The destination
operand is a general-purpose register. When the source operand is an XMM register,
the double-precision floating-point value is contained in the low quadword of
the register.

When a conversion is inexact, the value returned is rounded according to the
rounding control bits in the MXCSR register. If a converted result is larger
than the maximum signed doubleword integer, the floating-point invalid exception
is raised, and if this exception is masked, the indefinite integer value (80000000H)
is returned.

In 64-bit mode, the instruction can access additional registers (XMM8-XMM15,
R8-R15) when used with a REX.R prefix. Use of the REX.W prefix promotes the
instruction to 64-bit operation. See the summary chart at the beginning of this
section for encoding data and limits. Legacy SSE instructions: Use of the REX.W
prefix promotes the instruction to 64-bit operation. See the summary chart at
the beginning of this section for encoding data and limits. Note: In VEX-encoded
versions, VEX.vvvv is reserved and must be 1111b, otherwise instructions will
**``#UD.``**



### SIMD Floating-Point Exceptions
Invalid, Precision.


### Other Exceptions
See Exceptions Type 3; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
