## CVTSS2SI - Convert Scalar Single-Precision FP Value to Dword Integer

> Operation

``` slim
IF 64-bit Mode and OperandSize = 64
  THEN
     DEST[64:0] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[31:0]);
  ELSE
     DEST[31:0] <- Convert_Single_Precision_Floating_Point_To_Integer(SRC[31:0]);
FI;

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 F3 0F 2D /r CVTSS2SI r32, xmm/m32      | RM   | V/V           | SSE               | Convert one single-precision floating-point 
                                        |      |               |                   | value from xmm/m32 to one signed doubleword 
                                        |      |               |                   | integer in r32.                             
 F3 REX.W 0F 2D /r CVTSS2SI r64, xmm/m32| RM   | V/N.E.        | SSE               | Convert one single-precision floating-point 
                                        |      |               |                   | value from xmm/m32 to one signed quadword   
                                        |      |               |                   | integer in r64.                             
 VEX.LIG.F3.0F.W0 2D /r VCVTSS2SI r32,  | RM   | V/V           | AVX               | Convert one single-precision floating-point 
 xmm1/m32                               |      |               |                   | value from xmm1/m32 to one signed doubleword
                                        |      |               |                   | integer in r32.                             
 VEX.LIG.F3.0F.W1 2D /r VCVTSS2SI r64,  | RM   | V/N.E.1       | AVX               | Convert one single-precision floating-point 
 xmm1/m32                               |      |               |                   | value from xmm1/m32 to one signed quadword  
                                        |      |               |                   | integer in r64.                             
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
can be an XMM register or a memory location. The destination operand is a general-purpose
register. When the source operand is an XMM register, the single-precision floating-point
value is contained in the low doubleword of the register.

When a conversion is inexact, the value returned is rounded according to the
rounding control bits in the MXCSR register. If a converted result is larger
than the maximum signed doubleword integer, the floating-point invalid exception
is raised, and if this exception is masked, the indefinite integer value (80000000H)
is returned.

In 64-bit mode, the instruction can access additional registers (XMM8-XMM15,
R8-R15) when used with a REX.R prefix. Use of the REX.W prefix promotes the
instruction to 64-bit operands. See the summary chart at the beginning of this
section for encoding data and limits. Legacy SSE instructions: In 64-bit mode,
Use of the REX.W prefix promotes the instruction to 64-bit operands. See the
summary chart at the beginning of this section for encoding data and limits.
<aside class="notification">
In VEX-encoded versions, VEX.vvvv is reserved and must be 1111b, otherwise
instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
int _mm_cvtss_si32(__m128d a) __int64 _mm_cvtss_si64(__m128d a)


### SIMD Floating-Point Exceptions
Invalid, Precision.


### Other Exceptions
See Exceptions Type 3; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
