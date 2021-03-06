## CVTPI2PD - Convert Packed Dword Integers to Packed Double-Precision FP Values

> Operation

``` slim
DEST[63:0] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[31:0]);
DEST[127:64] <- Convert_Integer_To_Double_Precision_Floating_Point(SRC[63:32]);

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 CVTPI2PD:| __m128d _mm_cvtpi32_pd(__m64 a)

```

 Opcode/Instruction               | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                           
 ---  | --- | --- | --- | ---
 66 0F 2A /r CVTPI2PD xmm, mm/m64*| RM   | Valid      | Valid          | Convert two packed signed doubleword  
                                  |      |            |                | integers from mm/mem64 to two packed  
                                  |      |            |                | double-precision floating-point values
                                  |      |            |                | in xmm.                               
<aside class="notification">
*Operation is different for different operand sets; see the Description
section.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts two packed signed doubleword integers in the source operand (second
operand) to two packed doubleprecision floating-point values in the destination
operand (first operand).

The source operand can be an MMX technology register or a 64-bit memory location.
The destination operand is an XMM register. In addition, depending on the operand
### configuration

 - For operands xmm, mm: the instruction causes a transition from x87 FPU to MMX
technology operation (that is, the x87 FPU top-of-stack pointer is set to 0
and the x87 FPU tag word is set to all 0s [valid]). If this instruction is executed
while an x87 FPU floating-point exception is pending, the exception is handled
before the CVTPI2PD instruction is executed.
 - For operands xmm, m64: the instruction does not cause a transition to MMX technology
and does not take x87 FPU exceptions.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Table 22-6, “Exception Conditions for Legacy SIMD/MMX Instructions with
XMM and without FP Exception,” in the Intel® 64 and IA-32 Architectures Software
Developer's Manual, Volume 3B.
