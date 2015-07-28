## MOVDQ2Q - Move Quadword from XMM to MMX Technology Register

> Operation
``` slim

DEST <- SRC[63:0];

```

 Opcode     | Instruction    | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                                
 ---  | --- | --- | --- | --- | ---
 F2 0F D6 /r| MOVDQ2Q mm, xmm| RM   | Valid      | Valid          | Move low quadword from xmm to mmx register.

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Moves the low quadword from the source operand (second operand) to the destination
operand (first operand). The source operand is an XMM register and the destination
operand is an MMX technology register.

This instruction causes a transition from x87 FPU to MMX technology operation
(that is, the x87 FPU top-of-stack pointer is set to 0 and the x87 FPU tag word
is set to all 0s [valid]). If this instruction is executed while an x87 FPU
floating-point exception is pending, the exception is handled before the MOVDQ2Q
instruction is executed.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVDQ2Q:| __m64 _mm_movepi64_pi64 ( __m128i a)

### SIMD Floating-Point Exceptions
None.


### Protected Mode Exceptions
   | |  
---- | -----
 #NM| If CR0.TS[bit 3] = 1.                   
 #UD| If CR0.EM[bit 2] = 1. If CR4.OSFXSR[bit 
    | 9] = 0. If CPUID.01H:EDX.SSE2[bit 26]   
    | = 0. If the LOCK prefix is used.        
 #MF| If there is a pending x87 FPU exception.

### Real-Address Mode Exceptions
Same exceptions as in protected mode.


### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
