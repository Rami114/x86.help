## MUL - Unsigned Multiply

> Operation

``` slim
IF (Byte operation)
  THEN
     AX <- AL \* SRC;
  ELSE (\* Word or doubleword operation \*)
     IF OperandSize = 16
       THEN
          DX:AX <- AX \* SRC;
       ELSE IF OperandSize = 32
          THEN EDX:EAX <- EAX \* SRC; FI;
       ELSE (\* OperandSize = 64 \*)
          RDX:RAX <- RAX \* SRC;
     FI;
FI;

```

 Opcode       | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                               
 ---  | --- | --- | --- | --- | ---
 F6 /4        | MUL r/m8   | M    | Valid      | Valid          | Unsigned multiply (AX ← AL \* r/m8).       
 REX + F6 /4  | MUL r/m8\*  | M    | Valid      | N.E.           | Unsigned multiply (AX ← AL \* r/m8).       
 F7 /4        | MUL r/m16  | M    | Valid      | Valid          | Unsigned multiply (DX:AX ← AX \* r/m16).   
 F7 /4        | MUL r/m32  | M    | Valid      | Valid          | Unsigned multiply (EDX:EAX ← EAX \* r/m32).
 REX.W + F7 /4| MUL r/m64  | M    | Valid      | N.E.           | Unsigned multiply (RDX:RAX ← RAX \* r/m64).
<aside class="notification">
\* In 64-bit mode, r/m8 can not be encoded to access the following byte
registers if a REX prefix is used: AH, BH, CH, DH.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (r)| NA       | NA       | NA       

### Description
Performs an unsigned multiplication of the first operand (destination operand)
and the second operand (source operand) and stores the result in the destination
operand. The destination operand is an implied operand located in register AL,
AX or EAX (depending on the size of the operand); the source operand is located
in a generalpurpose register or a memory location. The action of this instruction
and the location of the result depends on the opcode and the operand size as
shown in Table 3-66.

The result is stored in register AX, register pair DX:AX, or register pair EDX:EAX
(depending on the operand size), with the high-order bits of the product contained
in register AH, DX, or EDX, respectively. If the high-order bits of the product
are 0, the CF and OF flags are cleared; otherwise, the flags are set.

In 64-bit mode, the instruction's default operation size is 32 bits. Use of
the REX.R prefix permits access to additional registers (R8-R15). Use of the
REX.W prefix promotes operation to 64 bits.

See the summary chart at the beginning of this section for encoding data and
limits.


### Table 3-66. MUL Results
 Operand Size| Source 1 AL AX EAX RAX| Source 2 r/m8 r/m16 r/m32 r/m64| Destination AX DX:AX EDX:EAX RDX:RAX
 ---  | --- | --- | ---


### Flags Affected
The OF and CF flags are set to 0 if the upper half of the result is 0; otherwise,
they are set to 1. The SF, ZF, AF, and PF flags are undefined.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory operand effective address
                | is outside the CS, DS, ES, FS, or GS 
                | segment limit. If the DS, ES, FS, or 
                | GS register contains a NULL segment  
                | selector.                            
 **``#SS(0)``**         | If a memory operand effective address
                | is outside the SS segment limit.     
 **``#PF(fault-code)``**| If a page fault occurs.              
 **``#AC(0)``**         | If alignment checking is enabled and 
                | an unaligned memory reference is made
                | while the current privilege level is 
                | 3.                                   
 **``#UD``**            | If the LOCK prefix is used.          

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#GP``**| If a memory operand effective address
    | is outside the CS, DS, ES, FS, or GS 
    | segment limit.                       
 **``#SS``**| If a memory operand effective address
    | is outside the SS segment limit.     
 **``#UD``**| If the LOCK prefix is used.          

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory operand effective address 
                | is outside the CS, DS, ES, FS, or GS  
                | segment limit.                        
 **``#SS(0)``**         | If a memory operand effective address 
                | is outside the SS segment limit.      
 **``#PF(fault-code)``**| If a page fault occurs.               
 **``#AC(0)``**         | If alignment checking is enabled and  
                | an unaligned memory reference is made.
 **``#UD``**            | If the LOCK prefix is used.           

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If a memory address referencing the        
                | SS segment is in a non-canonical form.     
 **``#GP(0)``**         | If the memory address is in a non-canonical
                | form.                                      
 **``#PF(fault-code)``**| If a page fault occurs.                    
 **``#AC(0)``**         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3.                                         
