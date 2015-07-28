## STI - Set Interrupt Flag

> Operation
``` slim

IF PE = 0
  THEN
     IF <- 1; (\* Set Interrupt Flag \*)
  ELSE
     IF VM = 0
       THEN
          IF IOPL ≥ CPL
             THEN
               IF <- 1;
          ELSE
             IF (IOPL < CPL) and (CPL = 3) and (VIP = 0)
               THEN
                  VIF <- 1;
               ELSE
                  #GP(0);
             FI;
          FI;
       ELSE
          IF IOPL = 3
             THEN
               IF <- 1;
          ELSE
             IF ((IOPL < 3) and (VIP = 0) and (VME = 1))
               THEN
                  VIF <- 1;
             ELSE
               #GP(0); (\* Trap to virtual-8086 monitor \*)
             FI;)
          FI;
     FI;
FI;

```

 Opcode\*| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                           
 ---  | --- | --- | --- | --- | ---
 FB     | STI        | NP   | Valid      | Valid          | Set interrupt flag; external, maskable
        |            |      |            |                | interrupts enabled at the end of the  
        |            |      |            |                | next instruction.                     

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
If protected-mode virtual interrupts are not enabled, STI sets the interrupt
flag (IF) in the EFLAGS register. After the IF flag is set, the processor begins
responding to external, maskable interrupts after the next instruction is executed.
The delayed effect of this instruction is provided to allow interrupts to be
enabled just before returning from a procedure (or subroutine). For instance,
if an STI instruction is followed by an RET instruction, the RET instruction
is allowed to execute before external interrupts are recognized1. If the STI
instruction is followed by a CLI instruction (which clears the IF flag), the
effect of the STI instruction is negated.

The IF flag and the STI and CLI instructions do not prohibit the generation
of exceptions and NMI interrupts. NMI interrupts (and SMIs) may be blocked for
one macroinstruction following an STI.

When protected-mode virtual interrupts are enabled, CPL is 3, and IOPL is less
than 3; STI sets the VIF flag in the EFLAGS register, leaving IF unaffected.

Table 4-15 indicates the action of the STI instruction depending on the processor's
mode of operation and the CPL/IOPL settings of the running program or procedure.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.


### Table 4-15. Decision Table for STI Results
   | |  
---- | -----
 PE| VM| IOPL | CPL| PVI| VIP| VME| STI Result
 0 | X | X    | X  | X  | X  | X  | IF = 1    
 1 | 0 | ≥ CPL| X  | X  | X  | X  | IF = 1    
 1 | 0 | < CPL| 3  | 1  | 0  | X  | VIF = 1   
 1 | 0 | < CPL| < 3| X  | X  | X  | GP Fault  
 1 | 0 | < CPL| X  | 0  | X  | X  | GP Fault  
 1 | 0 | < CPL| X  | X  | 1  | X  | GP Fault  
 1 | 1 | 3    | X  | X  | X  | X  | IF = 1    
 1 | 1 | < 3  | X  | X  | 0  | 1  | VIF = 1   
 1 | 1 | < 3  | X  | X  | 1  | X  | GP Fault  
 1 | 1 | < 3  | X  | X  | X  | 0  | GP Fault  
<aside class="notification">
X = This setting has no impact.
</aside>

   | |  
---- | -----
 1.| The STI instruction delays recognition      
   | of interrupts only if it is executed        
   | with EFLAGS.IF = 0. In a sequence of        
   | STI instructions, only the first instruction
   | in the sequence is guaranteed to delay      
   | interrupts. In the following instruction    
   | sequence, interrupts may be recognized      
   | before RET executes: STI STI RET            


### Flags Affected
The IF flag is set to 1; or the VIF flag is set to 1.


### Protected Mode Exceptions
   | |  
---- | -----
 #GP(0)| If the CPL is greater (has less privilege)
       | than the IOPL of the current program      
       | or procedure.                             
 #UD   | If the LOCK prefix is used.               

### Real-Address Mode Exceptions
   | |  
---- | -----
 #UD| If the LOCK prefix is used.

### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
