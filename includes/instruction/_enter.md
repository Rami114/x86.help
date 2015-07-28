## ENTER - Make Stack Frame for Procedure Parameters

> Operation

``` slim
NestingLevel <- NestingLevel MOD 32
IF 64-Bit Mode (StackSize = 64)
  THEN
     Push(RBP);
     FrameTemp <- RSP;
  ELSE IF StackSize = 32
     THEN
       Push(EBP);
       FrameTemp <- ESP; FI;
  ELSE (\* StackSize = 16 \*)
       Push(BP);
       FrameTemp <- SP;
FI;
IF NestingLevel = 0
  THEN GOTO CONTINUE;
FI;
IF (NestingLevel > 1)
  THEN FOR i <- 1 to (NestingLevel - 1)
     DO
       IF 64-Bit Mode (StackSize = 64)
          THEN
             RBP <- RBP - 8;
             Push([RBP]); (\* Quadword push \*)
          ELSE IF OperandSize = 32
             THEN
               IF StackSize = 32
                  EBP <- EBP - 4;
                  Push([EBP]); (\* Doubleword push \*)
               ELSE (\* StackSize = 16 \*)
                  BP <- BP - 4;
                  Push([BP]); (\* Doubleword push \*)
               FI;
             FI;
          ELSE (\* OperandSize = 16 \*)
             IF StackSize = 32
               THEN
                  EBP <- EBP - 2;
                  Push([EBP]); (\* Word push \*)
               ELSE (\* StackSize = 16 \*)
                  BP <- BP - 2;
                  Push([BP]); (\* Word push \*)
             FI;
          FI;
  OD;
FI;
IF 64-Bit Mode (StackSize = 64)
  THEN
     Push(FrameTemp); (\* Quadword push \*)
  ELSE IF OperandSize = 32
     THEN
       Push(FrameTemp); FI; (\* Doubleword push \*)
  ELSE (\* OperandSize = 16 \*)
       Push(FrameTemp); (\* Word push \*)
FI;
```

 Opcode  | Instruction      | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                                 
 ---  | --- | --- | --- | --- | ---
 C8 iw 00| ENTER imm16, 0   | II   | Valid      | Valid          | Create a stack frame for a procedure.       
 C8 iw 01| ENTER imm16,1    | II   | Valid      | Valid          | Create a nested stack frame for a procedure.
 C8 iw ib| ENTER imm16, imm8| II   | Valid      | Valid          | Create a nested stack frame for a procedure.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 II   | iw       | imm8     | NA       | NA       

### Description
Creates a stack frame for a procedure. The first operand (size operand) specifies
the size of the stack frame (that is, the number of bytes of dynamic storage
allocated on the stack for the procedure). The second operand (nesting level
operand) gives the lexical nesting level (0 to 31) of the procedure. The nesting
level determines the number of stack frame pointers that are copied into the
“display area” of the new stack frame from the preceding frame. Both of these
operands are immediate values.

The stack-size attribute determines whether the BP (16 bits), EBP (32 bits),
or RBP (64 bits) register specifies the current frame pointer and whether SP
(16 bits), ESP (32 bits), or RSP (64 bits) specifies the stack pointer. In 64bit
mode, stack-size attribute is always 64-bits.

The ENTER and companion LEAVE instructions are provided to support block structured
languages. The ENTER instruction (when used) is typically the first instruction
in a procedure and is used to set up a new stack frame for a procedure. The
LEAVE instruction is then used at the end of the procedure (just before the
RET instruction) to release the stack frame.

If the nesting level is 0, the processor pushes the frame pointer from the BP/EBP/RBP
register onto the stack, copies the current stack pointer from the SP/ESP/RSP
register into the BP/EBP/RBP register, and loads the SP/ESP/RSP register with
the current stack-pointer value minus the value in the size operand. For nesting
levels of 1 or greater, the processor pushes additional frame pointers on the
stack before adjusting the stack pointer. These additional frame pointers provide
the called procedure with access points to other nested frames on the stack.
See “Procedure Calls for Block-Structured Languages” in Chapter 6 of the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1, for more information
about the actions of the ENTER instruction.

The ENTER instruction causes a page fault whenever a write using the final value
of the stack pointer (within the current stack segment) would do so.

In 64-bit mode, default operation size is 64 bits; 32-bit operation size cannot
be encoded.



### CONTINUE
IF 64-Bit Mode (StackSize = 64)
  THEN
       RBP <- FrameTemp;
       RSP <- RSP − Size;
  ELSE IF StackSize = 32
     THEN
       EBP <- FrameTemp;
       ESP <- ESP − Size; FI;
  ELSE (* StackSize = 16 *)
       BP <- FrameTemp;
       SP <- SP − Size;
FI;
END;

### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If the new value of the SP or ESP register
                | is outside the stack segment limit.       
 **``#PF(fault-code)``**| If a page fault occurs or if a write      
                | using the final value of the stack pointer
                | (within the current stack segment) would  
                | cause a page fault.                       
 **``#UD``**            | If the LOCK prefix is used.               

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#SS``**| If the new value of the SP or ESP register
    | is outside the stack segment limit.       
 **``#UD``**| If the LOCK prefix is used.               

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If the new value of the SP or ESP register
                | is outside the stack segment limit.       
 **``#PF(fault-code)``**| If a page fault occurs or if a write      
                | using the final value of the stack pointer
                | (within the current stack segment) would  
                | cause a page fault.                       
 **``#UD``**            | If the LOCK prefix is used.               

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If the stack address is in a non-canonical
                | form.                                     
 **``#PF(fault-code)``**| If a page fault occurs or if a write      
                | using the final value of the stack pointer
                | (within the current stack segment) would  
                | cause a page fault.                       
 **``#UD``**            | If the LOCK prefix is used.               
