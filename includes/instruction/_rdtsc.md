## RDTSC - Read Time-Stamp Counter

> Operation
``` slim

IF (CR4.TSD = 0) or (CPL = 0) or (CR0.PE = 0)
  THEN EDX:EAX <- TimeStampCounter;
  ELSE (\* CR4.TSD = 1 and (CPL = 1, 2, or 3) and CR0.PE = 1 \*)
     #GP(0);
FI;

```

 Opcode\*| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                          
 ---  | --- | --- | --- | --- | ---
 0F 31  | RDTSC      | NP   | Valid      | Valid          | Read time-stamp counter into EDX:EAX.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Loads the current value of the processor's time-stamp counter (a 64-bit MSR)
into the EDX:EAX registers. The EDX register is loaded with the high-order 32
bits of the MSR and the EAX register is loaded with the low-order 32 bits. (On
processors that support the Intel 64 architecture, the high-order 32 bits of
each of RAX and RDX are cleared.)

The processor monotonically increments the time-stamp counter MSR every clock
cycle and resets it to 0 whenever the processor is reset. See “Time Stamp Counter”
in Chapter 17 of the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3B, for specific details of the time stamp counter behavior.

When in protected or virtual 8086 mode, the time stamp disable (TSD) flag in
register CR4 restricts the use of the RDTSC instruction as follows. When the
TSD flag is clear, the RDTSC instruction can be executed at any privilege level;
when the flag is set, the instruction can only be executed at privilege level
0. (When in real-address mode, the RDTSC instruction is always enabled.)

The time-stamp counter can also be read with the RDMSR instruction, when executing
at privilege level 0.

The RDTSC instruction is not a serializing instruction. It does not necessarily
wait until all previous instructions have been executed before reading the counter.
Similarly, subsequent instructions may begin execution before the read operation
is performed. If software requires RDTSC to be executed only after all previous
instructions have completed locally, it can either use RDTSCP (if the processor
supports that instruction) or execute the sequence LFENCE;RDTSC.

This instruction was introduced by the Pentium processor.

See “Changes to Instruction Behavior in VMX Non-Root Operation” in Chapter 25
of the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume
3C, for more information about the behavior of this instruction in VMX non-root
operation.



### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
 #GP(0)| If the TSD flag in register CR4 is set
       | and the CPL is greater than 0.        
 #UD   | If the LOCK prefix is used.           

### Real-Address Mode Exceptions
   | |  
---- | -----
 #UD| If the LOCK prefix is used.

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 #GP(0)| If the TSD flag in register CR4 is set.
 #UD   | If the LOCK prefix is used.            

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
