## CVTTSS2SI - Convert with Truncation Scalar Single-Precision FP Value to Dword Integer

> Operation

``` slim
IF 64-Bit Mode and OperandSize = 64
  THEN
     DEST[63:0] <- Convert_Single_Precision_Floating_Point_To_
             Integer_Truncate(SRC[31:0]);
  ELSE
     DEST[31:0] <- Convert_Single_Precision_Floating_Point_To_
             Integer_Truncate(SRC[31:0]);
FI;

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
int _mm_cvttss_si32(__m128d a) __int64 _mm_cvttss_si64(__m128d a)


```

 Opcode/Instruction                      | Op/ En| 64/32-bit Mode| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 F3 0F 2C /r CVTTSS2SI r32, xmm/m32      | RM    | V/V           | SSE               | Convert one single-precision floating-point 
                                         |       |               |                   | value from xmm/m32 to one signed doubleword 
                                         |       |               |                   | integer in r32 using truncation.            
 F3 REX.W 0F 2C /r CVTTSS2SI r64, xmm/m32| RM    | V/N.E.        | SSE               | Convert one single-precision floating-point 
                                         |       |               |                   | value from xmm/m32 to one signed quadword   
                                         |       |               |                   | integer in r64 using truncation.            
 VEX.LIG.F3.0F.W0 2C /r VCVTTSS2SI r32,  | RM    | V/V           | AVX               | Convert one single-precision floating-point 
 xmm1/m32                                |       |               |                   | value from xmm1/m32 to one signed doubleword
                                         |       |               |                   | integer in r32 using truncation.            
 VEX.LIG.F3.0F.W1 2C /r VCVTTSS2SI r64,  | RM    | V/N.E.1       | AVX               | Convert one single-precision floating-point 
 xmm1/m32                                |       |               |                   | value from xmm1/m32 to one signed quadword  
                                         |       |               |                   | integer in r64 using truncation.            
<aside class="notification">
1. Encoding the VEX prefix with VEX.W=1 in non-64-bit mode is ignored.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts a single-precision floating-point value in the source operand (second
operand) to a signed doubleword integer (or signed quadword integer if operand
size is 64 bits) in the destination operand (first operand). The source operand
can be an XMM register or a 32-bit memory location. The destination operand
is a general-purpose register. When the source operand is an XMM register, the
single-precision floating-point value is contained in the low doubleword of
the register.

When a conversion is inexact, a truncated (round toward zero) result is returned.
If a converted result is larger than the maximum signed doubleword integer,
the floating-point invalid exception is raised. If this exception is masked,
the indefinite integer value (80000000H) is returned.

Legacy SSE instructions: In 64-bit mode, the instruction can access additional
registers (XMM8-XMM15, R8-R15) when used with a REX.R prefix. Use of the REX.W
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
