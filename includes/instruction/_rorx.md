## RORX  -  Rotate Right Logical Without Affecting Flags

> Operation

``` slim
IF (OperandSize = 32)
  y <- imm8 AND 1FH;
  DEST <- (SRC >> y) | (SRC << (32-y));
ELSEIF (OperandSize = 64 )
  y <- imm8 AND 3FH;
  DEST <- (SRC >> y) | (SRC << (64-y));
ENDIF

```

 Opcode/Instruction                  | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                         
 ---  | --- | --- | --- | ---
 VEX.LZ.F2.0F3A.W0 F0 /r ib RORX r32,| RMI  | V/V            | BMI2              | Rotate 32-bit r/m32 right imm8 times
 r/m32, imm8                         |      |                |                   | without affecting arithmetic flags. 
 VEX.LZ.F2.0F3A.W1 F0 /r ib RORX r64,| RMI  | V/N.E.         | BMI2              | Rotate 64-bit r/m64 right imm8 times
 r/m64, imm8                         |      |                |                   | without affecting arithmetic flags. 

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| Imm8     | NA       

### Description
Rotates the bits of second operand right by the count value specified in imm8
without affecting arithmetic flags. The RORX instruction does not read or write
the arithmetic flags. This instruction is not supported in real mode and virtual-8086
mode. The operand size is always 32 bits if not in 64-bit mode. In 64-bit mode
operand size 64 requires VEX.W1. VEX.W1 is ignored in non-64-bit modes. An attempt
to execute this instruction with VEX.L not equal to 0 will cause #UD.



### Flags Affected
None


### Intel C/C++ Compiler Intrinsic Equivalent
Auto-generated from high-level language.


### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Section 2.5.1, “Exception Conditions for VEX-Encoded GPR Instructions”,
Table 2-29; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.W = 1.
