## IRET/IRETD - Interrupt Return

> Operation

``` slim
IF PE = 0
  THEN
     GOTO REAL-ADDRESS-MODE;
  ELSE
     IF (IA32_EFER.LMA = 0)
          THEN (\* Protected mode \*)
             GOTO PROTECTED-MODE;
          ELSE (\* IA-32e mode \*)
             GOTO IA-32e-MODE;
     FI;
FI;
REAL-ADDRESS-MODE;
  IF OperandSize = 32
     THEN
       IF top 12 bytes of stack not within stack limits
          THEN #SS; FI;
       tempEIP <- 4 bytes at end of stack
       IF tempEIP[31:16] is not zero THEN #GP(0); FI;
       EIP <- Pop();
       CS <- Pop(); (\* 32-bit pop, high-order 16 bits discarded \*)
       tempEFLAGS <- Pop();
       EFLAGS <- (tempEFLAGS AND 257FD5H) OR (EFLAGS AND 1A0000H);
     ELSE (\* OperandSize = 16 \*)
       IF top 6 bytes of stack are not within stack limits
          THEN #SS; FI;
       EIP <- Pop(); (\* 16-bit pop; clear upper 16 bits \*)
       CS <- Pop(); (\* 16-bit pop \*)
       EFLAGS[15:0] <- Pop();
  FI;
  END;
```

 Opcode    | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                            
 ---  | --- | --- | --- | --- | ---
 CF        | IRET       | NP   | Valid      | Valid          | Interrupt return (16-bit operand size).
 CF        | IRETD      | NP   | Valid      | Valid          | Interrupt return (32-bit operand size).
 REX.W + CF| IRETQ      | NP   | Valid      | N.E.           | Interrupt return (64-bit operand size).

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Returns program control from an exception or interrupt handler to a program
or procedure that was interrupted by an exception, an external interrupt, or
a software-generated interrupt. These instructions are also used to perform
a return from a nested task. (A nested task is created when a CALL instruction
is used to initiate a task switch or when an interrupt or exception causes a
task switch to an interrupt or exception handler.) See the section titled “Task
Linking” in Chapter 7 of the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3A.

IRET and IRETD are mnemonics for the same opcode. The IRETD mnemonic (interrupt
return double) is intended for use when returning from an interrupt when using
the 32-bit operand size; however, most assemblers use the IRET mnemonic interchangeably
for both operand sizes.

In Real-Address Mode, the IRET instruction preforms a far return to the interrupted
program or procedure. During this operation, the processor pops the return instruction
pointer, return code segment selector, and EFLAGS image from the stack to the
EIP, CS, and EFLAGS registers, respectively, and then resumes execution of the
interrupted program or procedure.

In Protected Mode, the action of the IRET instruction depends on the settings
of the NT (nested task) and VM flags in the EFLAGS register and the VM flag
in the EFLAGS image stored on the current stack. Depending on the setting of
### these flags, the processor performs the following types of interrupt returns

 - Return from virtual-8086 mode.
 - Return to virtual-8086 mode.
 - Intra-privilege level return.
 - Inter-privilege level return.
 - Return from nested task (task switch).

If the NT flag (EFLAGS register) is cleared, the IRET instruction performs a
far return from the interrupt procedure, without a task switch. The code segment
being returned to must be equally or less privileged than the interrupt handler
routine (as indicated by the RPL field of the code segment selector popped from
the stack).

As with a real-address mode interrupt return, the IRET instruction pops the
return instruction pointer, return code segment selector, and EFLAGS image from
the stack to the EIP, CS, and EFLAGS registers, respectively, and then resumes
execution of the interrupted program or procedure. If the return is to another
privilege level, the IRET instruction also pops the stack pointer and SS from
the stack, before resuming program execution. If the return is to virtual-8086
mode, the processor also pops the data segment registers from the stack.

If the NT flag is set, the IRET instruction performs a task switch (return)
from a nested task (a task called with a CALL instruction, an interrupt, or
an exception) back to the calling or interrupted task. The updated state of
the task executing the IRET instruction is saved in its TSS. If the task is
re-entered later, the code that follows the IRET instruction is executed.

If the NT flag is set and the processor is in IA-32e mode, the IRET instruction
causes a general protection exception.

In 64-bit mode, the instruction's default operation size is 32 bits. Use of
the REX.W prefix promotes operation to 64 bits (IRETQ). See the summary chart
at the beginning of this section for encoding data and limits.

See “Changes to Instruction Behavior in VMX Non-Root Operation” in Chapter 25
of the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume
3C, for more information about the behavior of this instruction in VMX non-root
operation.



### PROTECTED-MODE
  IF VM = 1 (\* Virtual-8086 mode: PE = 1, VM = 1 \*)
     THEN
       GOTO RETURN-FROM-VIRTUAL-8086-MODE; (\* PE = 1, VM = 1 \*)
  FI;
  IF NT = 1
     THEN
       GOTO TASK-RETURN; (\* PE = 1, VM = 0, NT = 1 \*)
  FI;
  IF OperandSize = 32
     THEN
       IF top 12 bytes of stack not within stack limits
          THEN #SS(0); FI;
       tempEIP <- Pop();
       tempCS <- Pop();
       tempEFLAGS <- Pop();
     ELSE (\* OperandSize = 16 \*)
       IF top 6 bytes of stack are not within stack limits
          THEN #SS(0); FI;
       tempEIP <- Pop();
       tempCS <- Pop();
       tempEFLAGS <- Pop();
       tempEIP <- tempEIP AND FFFFH;
       tempEFLAGS <- tempEFLAGS AND FFFFH;
  FI;
  IF tempEFLAGS(VM) = 1 and CPL = 0
     THEN
       GOTO RETURN-TO-VIRTUAL-8086-MODE;
     ELSE
       GOTO PROTECTED-MODE-RETURN;
  FI;
### IA-32e-MODE
  IF NT = 1
     THEN #GP(0);
  ELSE IF OperandSize = 32
     THEN
       IF top 12 bytes of stack not within stack limits
          THEN #SS(0); FI;
       tempEIP <- Pop();
       tempCS <- Pop();
       tempEFLAGS <- Pop();
     ELSE IF OperandSize = 16
       THEN
          IF top 6 bytes of stack are not within stack limits
             THEN #SS(0); FI;
          tempEIP <- Pop();
          tempCS <- Pop();
          tempEFLAGS <- Pop();
          tempEIP <- tempEIP AND FFFFH;
          tempEFLAGS <- tempEFLAGS AND FFFFH;
       FI;
     ELSE (\* OperandSize = 64 \*)
       THEN
             tempRIP <- Pop();
             tempCS <- Pop();
             tempEFLAGS <- Pop();
             tempRSP <- Pop();
             tempSS <- Pop();
  FI;
  GOTO IA-32e-MODE-RETURN;
### RETURN-FROM-VIRTUAL-8086-MODE
(\* Processor is in virtual-8086 mode when IRET is executed and stays in virtual-8086 mode \*)
  IF IOPL = 3 (\* Virtual mode: PE = 1, VM = 1, IOPL = 3 \*)
     THEN IF OperandSize = 32
       THEN
          IF top 12 bytes of stack not within stack limits
             THEN #SS(0); FI;
          IF instruction pointer not within code segment limits
             THEN #GP(0); FI;
          EIP <- Pop();
          CS <- Pop(); (\* 32-bit pop, high-order 16 bits discarded \*)
          EFLAGS <- Pop();
          (\* VM, IOPL,VIP and VIF EFLAG bits not modified by pop \*)
       ELSE (\* OperandSize = 16 \*)
          IF top 6 bytes of stack are not within stack limits
             THEN #SS(0); FI;
          IF instruction pointer not within code segment limits
             THEN #GP(0); FI;
          EIP <- Pop();
          EIP <- EIP AND 0000FFFFH;
          CS <- Pop(); (\* 16-bit pop \*)
          EFLAGS[15:0] <- Pop(); (\* IOPL in EFLAGS not modified by pop \*)
       FI;
     ELSE
       #GP(0); (\* Trap to virtual-8086 monitor: PE = 1, VM = 1, IOPL < 3 \*)
  FI;
END;
### RETURN-TO-VIRTUAL-8086-MODE
  (\* Interrupted procedure was in virtual-8086 mode: PE = 1, CPL=0, VM = 1 in flag image \*)
  IF top 24 bytes of stack are not within stack segment limits
     THEN #SS(0); FI;
  IF instruction pointer not within code segment limits
     THEN #GP(0); FI;
  CS <- tempCS;
  EIP <- tempEIP & FFFFH;
  EFLAGS <- tempEFLAGS;
  TempESP <- Pop();
  TempSS <- Pop();
  ES <- Pop(); (\* Pop 2 words; throw away high-order word \*)
  DS <- Pop(); (\* Pop 2 words; throw away high-order word \*)
  FS <- Pop(); (\* Pop 2 words; throw away high-order word \*)
  GS <- Pop(); (\* Pop 2 words; throw away high-order word \*)
  SS:ESP <- TempSS:TempESP;
  CPL <- 3;
  (\* Resume execution in Virtual-8086 mode \*)
END;
TASK-RETURN: (\* PE = 1, VM = 0, NT = 1 \*)
  Read segment selector in link field of current TSS;
  IF local/global bit is set to local
  or index not within GDT limits
     THEN #TS (TSS selector); FI;
  Access TSS for task specified in link field of current TSS;
  IF TSS descriptor type is not TSS or if the TSS is marked not busy
     THEN #TS (TSS selector); FI;
  IF TSS not present
     THEN #NP(TSS selector); FI;
  SWITCH-TASKS (without nesting) to TSS specified in link field of current TSS;
  Mark the task just abandoned as NOT BUSY;
  IF EIP is not within code segment limit
     THEN #GP(0); FI;
END;
PROTECTED-MODE-RETURN: (\* PE = 1 \*)
  IF return code segment selector is NULL
     THEN GP(0); FI;
  IF return code segment selector addresses descriptor beyond descriptor table limit
     THEN GP(selector); FI;
  Read segment descriptor pointed to by the return code segment selector;
  IF return code segment descriptor is not a code segment
     THEN #GP(selector); FI;
  IF return code segment selector RPL < CPL
     THEN #GP(selector); FI;
  IF return code segment descriptor is conforming
  and return code segment DPL > return code segment selector RPL
     THEN #GP(selector); FI;
  IF return code segment descriptor is not present
     THEN #NP(selector); FI;
  IF return code segment selector RPL > CPL
     THEN GOTO RETURN-OUTER-PRIVILEGE-LEVEL;
     ELSE GOTO RETURN-TO-SAME-PRIVILEGE-LEVEL; FI;
END;
RETURN-TO-SAME-PRIVILEGE-LEVEL: (\* PE = 1, RPL = CPL \*)
  IF new mode != 64-Bit Mode
     THEN
       IF tempEIP is not within code segment limits
          THEN #GP(0); FI;
       EIP <- tempEIP;
     ELSE (\* new mode = 64-bit mode \*)
       IF tempRIP is non-canonical
             THEN #GP(0); FI;
       RIP <- tempRIP;
  FI;
  CS <- tempCS; (\* Segment descriptor information also loaded \*)
  EFLAGS (CF, PF, AF, ZF, SF, TF, DF, OF, NT) <- tempEFLAGS;
  IF OperandSize = 32 or OperandSize = 64
     THEN EFLAGS(RF, AC, ID) <- tempEFLAGS; FI;
  IF CPL ≤ IOPL
     THEN EFLAGS(IF) <- tempEFLAGS; FI;
  IF CPL = 0
     THEN (\* VM = 0 in flags image \*)
     EFLAGS(IOPL) <- tempEFLAGS;
     IF OperandSize = 32 or OperandSize = 64
       THEN EFLAGS(VIF, VIP) <- tempEFLAGS; FI;
  FI;
END;
### RETURN-TO-OUTER-PRIVILEGE-LEVEL
  IF OperandSize = 32
     THEN
       IF top 8 bytes on stack are not within limits
          THEN #SS(0); FI;
     ELSE (\* OperandSize = 16 \*)
       IF top 4 bytes on stack are not within limits
          THEN #SS(0); FI;
  FI;
  Read return segment selector;
  IF stack segment selector is NULL
     THEN #GP(0); FI;
  IF return stack segment selector index is not within its descriptor table limits
     THEN #GP(SSselector); FI;
  Read segment descriptor pointed to by return segment selector;
  IF stack segment selector RPL != RPL of the return code segment selector
  or the stack segment descriptor does not indicate a a writable data segment;
  or the stack segment DPL != RPL of the return code segment selector
     THEN #GP(SS selector); FI;
  IF stack segment is not present
     THEN #SS(SS selector); FI;
  IF new mode != 64-Bit Mode
     THEN
       IF tempEIP is not within code segment limits
          THEN #GP(0); FI;
       EIP <- tempEIP;
     ELSE (\* new mode = 64-bit mode \*)
       IF tempRIP is non-canonical
             THEN #GP(0); FI;
       RIP <- tempRIP;
  FI;
  CS <- tempCS;
  EFLAGS (CF, PF, AF, ZF, SF, TF, DF, OF, NT) <- tempEFLAGS;
  IF OperandSize = 32
     THEN EFLAGS(RF, AC, ID) <- tempEFLAGS; FI;
  IF CPL ≤ IOPL
     THEN EFLAGS(IF) <- tempEFLAGS; FI;
  IF CPL = 0
     THEN
       EFLAGS(IOPL) <- tempEFLAGS;
       IF OperandSize = 32
          THEN EFLAGS(VM, VIF, VIP) <- tempEFLAGS; FI;
       IF OperandSize = 64
          THEN EFLAGS(VIF, VIP) <- tempEFLAGS; FI;
  FI;
  CPL <- RPL of the return code segment selector;
  FOR each of segment register (ES, FS, GS, and DS)
     DO
       IF segment register points to data or non-conforming code segment
       and CPL > segment descriptor DPL (\* Stored in hidden part of segment register \*)
          THEN (\* Segment register invalid \*)
             SegmentSelector <- 0; (\* NULL segment selector \*)
       FI;
     OD;
END;
IA-32e-MODE-RETURN: (\* IA32_EFER.LMA = 1, PE = 1 \*)
  IF ( (return code segment selector is NULL) or (return RIP is non-canonical) or
       (SS selector is NULL going back to compatibility mode) or
       (SS selector is NULL going back to CPL3 64-bit mode) or
       (RPL <> CPL going back to non-CPL3 64-bit mode for a NULL SS selector) )
     THEN GP(0); FI;
  IF return code segment selector addresses descriptor beyond descriptor table limit
     THEN GP(selector); FI;
  Read segment descriptor pointed to by the return code segment selector;
  IF return code segment descriptor is not a code segment
     THEN #GP(selector); FI;
  IF return code segment selector RPL < CPL
     THEN #GP(selector); FI;
  IF return code segment descriptor is conforming
  and return code segment DPL > return code segment selector RPL
     THEN #GP(selector); FI;
  IF return code segment descriptor is not present
     THEN #NP(selector); FI;
  IF return code segment selector RPL > CPL
     THEN GOTO RETURN-OUTER-PRIVILEGE-LEVEL;
     ELSE GOTO RETURN-TO-SAME-PRIVILEGE-LEVEL; FI;
END;

### Flags Affected
All the flags and fields in the EFLAGS register are potentially modified, depending
on the mode of operation of the processor. If performing a return from a nested
task to a previous task, the EFLAGS register will be modified according to the
EFLAGS image stored in the previous task's TSS.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If the return code or stack segment               
                | selector is NULL. If the return instruction       
                | pointer is not within the return code             
                | segment limit.                                    
 **``#GP(selector)``**  | If a segment selector index is outside            
                | its descriptor table limits. If the               
                | return code segment selector RPL is               
                | less than the CPL. If the DPL of a conforming-code
                | segment is greater than the return code           
                | segment selector RPL. If the DPL for              
                | a nonconforming-code segment is not               
                | equal to the RPL of the code segment              
                | selector. If the stack segment descriptor         
                | DPL is not equal to the RPL of the return         
                | code segment selector. If the stack               
                | segment is not a writable data segment.           
                | If the stack segment selector RPL is              
                | not equal to the RPL of the return code           
                | segment selector. If the segment descriptor       
                | for a code segment does not indicate              
                | it is a code segment. If the segment              
                | selector for a TSS has its local/global           
                | bit set for local. If a TSS segment               
                | descriptor specifies that the TSS is              
                | not busy. If a TSS segment descriptor             
                | specifies that the TSS is not available.          
 **``#SS(0)``**         | If the top bytes of stack are not within          
                | stack limits.                                     
 **``#NP(selector)``**  | If the return code or stack segment               
                | is not present.                                   
 **``#PF(fault-code)``**| If a page fault occurs.                           
 **``#AC(0)``**         | If an unaligned memory reference occurs           
                | when the CPL is 3 and alignment checking          
                | is enabled.                                       
 **``#UD``**            | If the LOCK prefix is used.                       

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#GP``**| If the return instruction pointer is     
    | not within the return code segment limit.
 **``#SS``**| If the top bytes of stack are not within 
    | stack limits.                            

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**| If the return instruction pointer is     
       | not within the return code segment limit.
IF IOPL not equal to 3.

   | |  
---- | -----
 **``#PF(fault-code)``**| If a page fault occurs.                 
 **``#SS(0)``**         | If the top bytes of stack are not within
                | stack limits.                           
 **``#AC(0)``**         | If an unaligned memory reference occurs 
                | and alignment checking is enabled.      
 **``#UD``**            | If the LOCK prefix is used.             

### Compatibility Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``** Other exceptions same as in Protected| If EFLAGS.NT[bit 14] = 1.
 Mode.                                       |                          

### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If EFLAGS.NT[bit 14] = 1. If the return    
                | code segment selector is NULL. If the      
                | stack segment selector is NULL going       
                | back to compatibility mode. If the stack   
                | segment selector is NULL going back        
                | to CPL3 64-bit mode. If a NULL stack       
                | segment selector RPL is not equal to       
                | CPL going back to non-CPL3 64-bit mode.    
                | If the return instruction pointer is       
                | not within the return code segment limit.  
                | If the return instruction pointer is       
                | non-canonical.                             
 **``#GP(Selector)``**  | If a segment selector index is outside     
                | its descriptor table limits. If a segment  
                | descriptor memory address is non-canonical.
                | If the segment descriptor for a code       
                | segment does not indicate it is a code     
                | segment. If the proposed new code segment  
                | descriptor has both the D-bit and L-bit    
                | set. If the DPL for a nonconforming-code   
                | segment is not equal to the RPL of the     
                | code segment selector. If CPL is greater   
                | than the RPL of the code segment selector. 
                | If the DPL of a conforming-code segment    
                | is greater than the return code segment    
                | selector RPL. If the stack segment is      
                | not a writable data segment. If the        
                | stack segment descriptor DPL is not        
                | equal to the RPL of the return code        
                | segment selector. If the stack segment     
                | selector RPL is not equal to the RPL       
                | of the return code segment selector.       
 **``#SS(0)``**         | If an attempt to pop a value off the       
                | stack violates the SS limit. If an attempt 
                | to pop a value off the stack causes        
                | a non-canonical address to be referenced.  
 **``#NP(selector)``**  | If the return code or stack segment        
                | is not present.                            
 **``#PF(fault-code)``**| If a page fault occurs.                    
 **``#AC(0)``**         | If an unaligned memory reference occurs    
                | when the CPL is 3 and alignment checking   
                | is enabled.                                
 **``#UD``**            | If the LOCK prefix is used.                
