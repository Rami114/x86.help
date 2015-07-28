## IMUL - Signed Multiply

> Operation
``` slim

IF (NumberOfOperands = 1)
  THEN IF (OperandSize = 8)
     THEN
       AX <- AL \* SRC (\* Signed multiplication \*)
       IF AL = AX
          THEN CF <- 0; OF <- 0;
          ELSE CF <- 1; OF <- 1; FI;
     ELSE IF OperandSize = 16
       THEN
          DX:AX <- AX \* SRC (\* Signed multiplication \*)
          IF sign_extend_to_32 (AX) = DX:AX
             THEN CF <- 0; OF <- 0;
             ELSE CF <- 1; OF <- 1; FI;
       ELSE IF OperandSize = 32
          THEN
             EDX:EAX <- EAX \* SRC (\* Signed multiplication \*)
             IF EAX = EDX:EAX
               THEN CF <- 0; OF <- 0;
               ELSE CF <- 1; OF <- 1; FI;
          ELSE (\* OperandSize = 64 \*)
             RDX:RAX <- RAX \* SRC (\* Signed multiplication \*)
             IF RAX = RDX:RAX
               THEN CF <- 0; OF <- 0;
               ELSE CF <- 1; OF <- 1; FI;
          FI;
     FI;
  ELSE IF (NumberOfOperands = 2)
     THEN
       temp <- DEST \* SRC (\* Signed multiplication; temp is double DEST size \*)
       DEST <- DEST \* SRC (\* Signed multiplication \*)
       IF temp != DEST
          THEN CF <- 1; OF <- 1;
          ELSE CF <- 0; OF <- 0; FI;
     ELSE (\* NumberOfOperands = 3 \*)
       DEST <- SRC1 \* SRC2 (\* Signed multiplication \*)
       temp <- SRC1 \* SRC2 (\* Signed multiplication; temp is double SRC1 size \*)
       IF temp != DEST
          THEN CF <- 1; OF <- 1;
          ELSE CF <- 0; OF <- 0; FI;
  FI;
FI;

```

 Opcode          | Instruction           | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                               
 ---  | --- | --- | --- | --- | ---
 F6 /5           | IMUL r/m8\*            | M    | Valid      | Valid          | AX← AL \* r/m byte.                        
 F7 /5           | IMUL r/m16            | M    | Valid      | Valid          | DX:AX ← AX \* r/m word.                    
 F7 /5           | IMUL r/m32            | M    | Valid      | Valid          | EDX:EAX ← EAX \* r/m32.                    
 REX.W + F7 /5   | IMUL r/m64            | M    | Valid      | N.E.           | RDX:RAX ← RAX \* r/m64.                    
 0F AF /r        | IMUL r16, r/m16       | RM   | Valid      | Valid          | word register ← word register \* r/m16.    
 0F AF /r        | IMUL r32, r/m32       | RM   | Valid      | Valid          | doubleword register ← doubleword register 
                 |                       |      |            |                | \*r/m32.                                   
 REX.W + 0F AF /r| IMUL r64, r/m64       | RM   | Valid      | N.E.           | Quadword register ← Quadword register     
                 |                       |      |            |                | \*r/m64.                                   
 6B /r ib        | IMUL r16, r/m16, imm8 | RMI  | Valid      | Valid          | word register ← r/m16 \* sign-extended     
                 |                       |      |            |                | immediate byte.                           
 6B /r ib        | IMUL r32, r/m32, imm8 | RMI  | Valid      | Valid          | doubleword register ← r/m32 \* signextended
                 |                       |      |            |                | immediate byte.                           
 REX.W + 6B /r ib| IMUL r64, r/m64, imm8 | RMI  | Valid      | N.E.           | Quadword register ← r/m64 \* sign-extended 
                 |                       |      |            |                | immediate byte.                           
 69 /r iw        | IMUL r16, r/m16, imm16| RMI  | Valid      | Valid          | word register ← r/m16 \* immediate word.   
 69 /r id        | IMUL r32, r/m32, imm32| RMI  | Valid      | Valid          | doubleword register ← r/m32 \* immediate   
                 |                       |      |            |                | doubleword.                               
 REX.W + 69 /r id| IMUL r64, r/m64, imm32| RMI  | Valid      | N.E.           | Quadword register ← r/m64 \* immediate     
                 |                       |      |            |                | doubleword.                               
<aside class="notification">
\* In 64-bit mode, r/m8 can not be encoded to access the following byte
registers if a REX prefix is used: AH, BH, CH, DH.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3 | Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (r, w)| NA           | NA        | NA       
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA        | NA       
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8/16/32| NA       

### Description
Performs a signed multiplication of two operands. This instruction has three
forms, depending on the number of operands.

 - One-operand form  -  This form is identical to that used by the MUL instruction.
Here, the source operand (in a general-purpose register or memory location)
is multiplied by the value in the AL, AX, EAX, or RAX register (depending on
the operand size) and the product is stored in the AX, DX:AX, EDX:EAX, or RDX:RAX
registers, respectively.
 - Two-operand form  -  With this form the destination operand (the first operand)
is multiplied by the source operand (second operand). The destination operand
is a general-purpose register and the source operand is an immediate value,
a general-purpose register, or a memory location. The product is then stored
in the destination operand location.
 - Three-operand form  -  This form requires a destination operand (the first operand)
and two source operands (the second and the third operands). Here, the first
source operand (which can be a general-purpose register or a memory location)
is multiplied by the second source operand (an immediate value). The product
is then stored in the destination operand (a general-purpose register).

When an immediate value is used as an operand, it is sign-extended to the length
of the destination operand format.

The CF and OF flags are set when significant bit (including the sign bit) are
carried into the upper half of the result. The CF and OF flags are cleared when
the result (including the sign bit) fits exactly in the lower half of the result.

The three forms of the IMUL instruction are similar in that the length of the
product is calculated to twice the length of the operands. With the one-operand
form, the product is stored exactly in the destination. With the two- and three-
operand forms, however, the result is truncated to the length of the destination
before it is stored in the destination register. Because of this truncation,
the CF or OF flag should be tested to ensure that no significant bits are lost.

The two- and three-operand forms may also be used with unsigned operands because
the lower half of the product is the same regardless if the operands are signed
or unsigned. The CF and OF flags, however, cannot be used to determine if the
upper half of the result is non-zero.

In 64-bit mode, the instruction's default operation size is 32 bits. Use of
the REX.R prefix permits access to additional registers (R8-R15). Use of the
REX.W prefix promotes operation to 64 bits. Use of REX.W modifies the three
forms of the instruction as follows.

 - One-operand form  - The source operand (in a 64-bit general-purpose register or
memory location) is multiplied by the value in the RAX register and the product
is stored in the RDX:RAX registers.
 - Two-operand form  -  The source operand is promoted to 64 bits if it is a register
or a memory location. The destination operand is promoted to 64 bits.
 - Three-operand form  -  The first source operand (either a register or a memory
location) and destination operand are promoted to 64 bits. If the source operand
is an immediate, it is sign extended to 64 bits.



### Flags Affected
For the one operand form of the instruction, the CF and OF flags are set when
significant bits are carried into the upper half of the result and cleared when
the result fits exactly in the lower half of the result. For the two- and three-operand
forms of the instruction, the CF and OF flags are set when the result must be
truncated to fit in the destination operand size and cleared when the result
fits exactly in the destination operand size. The SF, ZF, AF, and PF flags are
undefined.


### Protected Mode Exceptions
   | |  
---- | -----
 #GP(0)         | If a memory operand effective address
                | is outside the CS, DS, ES, FS, or GS 
                | segment limit. If the DS, ES, FS, or 
                | GS register is used to access memory 
                | and it contains a NULL NULL segment  
                | selector.                            
 #SS(0)         | If a memory operand effective address
                | is outside the SS segment limit.     
 #PF(fault-code)| If a page fault occurs.              
 #AC(0)         | If alignment checking is enabled and 
                | an unaligned memory reference is made
                | while the current privilege level is 
                | 3.                                   
 #UD            | If the LOCK prefix is used.          

### Real-Address Mode Exceptions
   | |  
---- | -----
 #GP| If a memory operand effective address
    | is outside the CS, DS, ES, FS, or GS 
    | segment limit.                       
 #SS| If a memory operand effective address
    | is outside the SS segment limit.     
 #UD| If the LOCK prefix is used.          

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 #GP(0)         | If a memory operand effective address 
                | is outside the CS, DS, ES, FS, or GS  
                | segment limit.                        
 #SS(0)         | If a memory operand effective address 
                | is outside the SS segment limit.      
 #PF(fault-code)| If a page fault occurs.               
 #AC(0)         | If alignment checking is enabled and  
                | an unaligned memory reference is made.
 #UD            | If the LOCK prefix is used.           

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 #SS(0)         | If a memory address referencing the        
                | SS segment is in a non-canonical form.     
 #GP(0)         | If the memory address is in a non-canonical
                | form.                                      
 #PF(fault-code)| If a page fault occurs.                    
 #AC(0)         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3.                                         
 #UD            | If the LOCK prefix is used.                
