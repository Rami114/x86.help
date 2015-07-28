## POPF/POPFD/POPFQ - Pop Stack into EFLAGS Register

> Operation

``` slim
IF VM = 0 (\* Not in Virtual-8086 Mode \*)
  THEN IF CPL = 0
     THEN
       IF OperandSize = 32;
          THEN
             EFLAGS <- Pop(); (\* 32-bit pop \*)
             (\* All non-reserved flags except RF, VIP, VIF, and VM can be modified;
             VIP and VIF are cleared; RF, VM, and all reserved bits are unaffected. \*)
          ELSE IF (Operandsize = 64)
1.
             RFLAGS = Pop(); (\* 64-bit pop \*)
             (\* All non-reserved flags except RF, VIP, VIF, and VM can be modified; VIP
             and VIF are cleared; RF, VM, and all reserved bits are unaffected.\*)
          ELSE (\* OperandSize = 16 \*)
             EFLAGS[15:0] <- Pop(); (\* 16-bit pop \*)
             (\* All non-reserved flags can be modified. \*)
       FI;
     ELSE (\* CPL > 0 \*)
       IF OperandSize = 32
          THEN
             IF CPL > IOPL
               THEN
                  EFLAGS <- Pop(); (\* 32-bit pop \*)
                  (\* All non-reserved bits except IF, IOPL, RF, VIP, and
                  VIF can be modified; IF, IOPL, RF, VM, and all reserved
                  bits are unaffected; VIP and VIF are cleared. \*)
               ELSE
                  EFLAGS <- Pop(); (\* 32-bit pop \*)
                  (\* All non-reserved bits except IOPL, RF, VIP, and VIF can be
                  modified; IOPL, RF, VM, and all reserved bits are
                  unaffected; VIP and VIF are cleared. \*)
             FI;
          ELSE IF (Operandsize = 64)
             IF CPL > IOPL
               THEN
                  RFLAGS <- Pop(); (\* 64-bit pop \*)
                  (\* All non-reserved bits except IF, IOPL, RF, VIP, and
                  VIF can be modified; IF, IOPL, RF, VM, and all reserved
                  bits are unaffected; VIP and VIF are cleared. \*)
               ELSE
                  RFLAGS <- Pop(); (\* 64-bit pop \*)
                  (\* All non-reserved bits except IOPL, RF, VIP, and VIF can be
                  modified; IOPL, RF, VM, and all reserved bits are
                  unaffected; VIP and VIF are cleared. \*)
             FI;
          ELSE (\* OperandSize = 16 \*)
             EFLAGS[15:0] <- Pop(); (\* 16-bit pop \*)
             (\* All non-reserved bits except IOPL can be modified; IOPL and all
             reserved bits are unaffected. \*)
       FI;
     FI;
  ELSE
     IF IOPL = 3
       THEN IF OperandSize = 32
          THEN
             EFLAGS <- Pop();
             (\* All non-reserved bits except VM, RF, IOPL, VIP, and VIF can be
             modified; VM, RF, IOPL, VIP, VIF, and all reserved bits are unaffected. \*)
          ELSE
             EFLAGS[15:0] <- Pop(); FI;
             (\* All non-reserved bits except IOPL can be modified;
             IOPL and all reserved bits are unaffected. \*)
     ELSE (\* IOPL < 3 \*)
       #GP(0);
     FI;
  FI;
FI;

```

 Opcode| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                          
 ---  | --- | --- | --- | --- | ---
 9D    | POPF       | NP   | Valid      | Valid          | Pop top of stack into lower 16 bits  
       |            |      |            |                | of EFLAGS.                           
 9D    | POPFD      | NP   | N.E.       | Valid          | Pop top of stack into EFLAGS.        
 9D    | POPFQ      | NP   | Valid      | N.E.           | Pop top of stack and zero-extend into
       |            |      |            |                | RFLAGS.                              

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Pops a doubleword (POPFD) from the top of the stack (if the current operand-size
attribute is 32) and stores the value in the EFLAGS register, or pops a word
from the top of the stack (if the operand-size attribute is 16) and stores it
in the lower 16 bits of the EFLAGS register (that is, the FLAGS register). These
instructions reverse the operation of the PUSHF/PUSHFD instructions.

The POPF (pop flags) and POPFD (pop flags double) mnemonics reference the same
opcode. The POPF instruction is intended for use when the operand-size attribute
is 16; the POPFD instruction is intended for use when the operand-size attribute
is 32. Some assemblers may force the operand size to 16 for POPF and to 32 for
POPFD. Others may treat the mnemonics as synonyms (POPF/POPFD) and use the setting
of the operand-size attribute to determine the size of values to pop from the
stack.

The effect of POPF/POPFD on the EFLAGS register changes, depending on the mode
of operation. When the processor is operating in protected mode at privilege
level 0 (or in real-address mode, the equivalent to privilege level 0), all
non-reserved flags in the EFLAGS register except RF1, VIP, VIF, and VM may be
modified. VIP, VIF and VM remain unaffected.

When operating in protected mode with a privilege level greater than 0, but
less than or equal to IOPL, all flags can be modified except the IOPL field
and VIP, VIF, and VM. Here, the IOPL flags are unaffected, the VIP and VIF flags
are cleared, and the VM flag is unaffected. The interrupt flag (IF) is altered
only when executing at a level at least as privileged as the IOPL. If a POPF/POPFD
instruction is executed with insufficient privilege, an exception does not occur
but privileged bits do not change.

When operating in virtual-8086 mode, the IOPL must be equal to 3 to use POPF/POPFD
instructions; VM, RF, IOPL, VIP, and VIF are unaffected. If the IOPL is less
than 3, POPF/POPFD causes a general-protection exception (#GP).

In 64-bit mode, use REX.W to pop the top of stack to RFLAGS. The mnemonic assigned
is POPFQ (note that the 32bit operand is not encodable). POPFQ pops 64 bits
from the stack, loads the lower 32 bits into RFLAGS, and zero extends the upper
bits of RFLAGS.

See Chapter 3 of the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 1, for more information about the EFLAGS registers.



### Flags Affected
All flags may be affected; see the Operation section for details.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If the top of stack is not within the  
                | stack segment.                         
 **``#PF(fault-code)``**| If a page fault occurs.                
 **``#AC(0)``**         | If an unaligned memory reference is    
                | made while the current privilege level 
                | is 3 and alignment checking is enabled.
 **``#UD``**            | If the LOCK prefix is used.            

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#SS``**| If the top of stack is not within the
    | stack segment.                       
 **``#UD``**| If the LOCK prefix is used.          

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If the I/O privilege level is less than        
                | 3. If an attempt is made to execute            
                | the POPF/POPFD instruction with an operand-size
                | override prefix.                               
 **``#SS(0)``**         | If the top of stack is not within the          
                | stack segment.                                 
 **``#PF(fault-code)``**| If a page fault occurs.                        
 **``#AC(0)``**         | If an unaligned memory reference is            
                | made while alignment checking is enabled.      
 **``#UD``**            | If the LOCK prefix is used.                    

### Compatibility Mode Exceptions
Same as for protected mode exceptions.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If the memory address is in a non-canonical
                | form.                                      
 **``#SS(0)``**         | If the stack address is in a non-canonical 
                | form.                                      
 **``#PF(fault-code)``**| If a page fault occurs.                    
 **``#AC(0)``**         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3.                                         
 **``#UD``**            | If the LOCK prefix is used.                
