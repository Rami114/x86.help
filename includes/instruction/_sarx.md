## SARX/SHLX/SHRX  -  Shift Without Affecting Flags

> Operation
``` slim

TEMP <- SRC1;
IF VEX.W1 and CS.L = 1
THEN
  countMASK <-3FH;
ELSE
  countMASK <-1FH;
FI
COUNT <- (SRC2 AND countMASK)
DEST[OperandSize -1] = TEMP[OperandSize -1];
DO WHILE (COUNT != 0)
  IF instruction is SHLX
     THEN
       DEST[] <- DEST \*2;
     ELSE IF instruction is SHRX
       THEN
          DEST[] <- DEST /2; //unsigned divide
     ELSE
          DEST[] <- DEST /2; // signed divide, round toward negative infinity
  FI;
  COUNT <- COUNT - 1;
OD

```

 Opcode/Instruction                     | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 VEX.NDS1.LZ.F3.0F38.W0 F7 /r SARX r32a,| RMV  | V/V            | BMI2              | Shift r/m32 arithmetically right with 
 r/m32, r32b                            |      |                |                   | count specified in r32b.              
 VEX.NDS1.LZ.66.0F38.W0 F7 /r SHLX r32a,| RMV  | V/V            | BMI2              | Shift r/m32 logically left with count 
 r/m32, r32b                            |      |                |                   | specified in r32b.                    
 VEX.NDS1.LZ.F2.0F38.W0 F7 /r SHRX r32a,| RMV  | V/V            | BMI2              | Shift r/m32 logically right with count
 r/m32, r32b                            |      |                |                   | specified in r32b.                    
 VEX.NDS1.LZ.F3.0F38.W1 F7 /r SARX r64a,| RMV  | V/N.E.         | BMI2              | Shift r/m64 arithmetically right with 
 r/m64, r64b                            |      |                |                   | count specified in r64b.              
 VEX.NDS1.LZ.66.0F38.W1 F7 /r SHLX r64a,| RMV  | V/N.E.         | BMI2              | Shift r/m64 logically left with count 
 r/m64, r64b                            |      |                |                   | specified in r64b.                    
 VEX.NDS1.LZ.F2.0F38.W1 F7 /r SHRX r64a,| RMV  | V/N.E.         | BMI2              | Shift r/m64 logically right with count
 r/m64, r64b                            |      |                |                   | specified in r64b.                    
<aside class="notification">
1. ModRM:r/m is used to encode the first source operand (second operand)
and VEX.vvvv encodes the second source operand (third operand).
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3   | Operand 4
 ---  | --- | --- | --- | ---
 RMV  | ModRM:reg (w)| ModRM:r/m (r)| VEX.vvvv (r)| NA       

### Description
Shifts the bits of the first source operand (the second operand) to the left
or right by a COUNT value specified in the second source operand (the third
operand). The result is written to the destination operand (the first operand).
The shift arithmetic right (SARX) and shift logical right (SHRX) instructions
shift the bits of the destination operand to the right (toward less significant
bit locations), SARX keeps and propagates the most significant bit (sign bit)
while shifting. The logical shift left (SHLX) shifts the bits of the destination
operand to the left (toward more significant bit locations). This instruction
is not supported in real mode and virtual-8086 mode. The operand size is always
32 bits if not in 64-bit mode. In 64-bit mode operand size 64 requires VEX.W1.
VEX.W1 is ignored in non-64-bit modes. An attempt to execute this instruction
with VEX.L not equal to 0 will cause #UD. If the value specified in the first
source operand exceeds OperandSize -1, the COUNT value is masked. SARX,SHRX,
and SHLX instructions do not update flags.



### Flags Affected
None.


### Intel C/C++ Compiler Intrinsic Equivalent
Auto-generated from high-level language.


### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Section 2.5.1, “Exception Conditions for VEX-Encoded GPR Instructions”,
Table 2-29; additionally

   | |  
---- | -----
 #UD| If VEX.W = 1.
