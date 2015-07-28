## RDTSCP - Read Time-Stamp Counter and Processor ID

> Operation

``` slim
IF (CR4.TSD = 0) or (CPL = 0) or (CR0.PE = 0)
  THEN
     EDX:EAX <- TimeStampCounter;
     ECX <- IA32_TSC_AUX[31:0];
  ELSE (\* CR4.TSD = 1 and (CPL = 1, 2, or 3) and CR0.PE = 1 \*)
     #GP(0);
FI;

```

 Opcode\* | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                              
 ---  | --- | --- | --- | --- | ---
 0F 01 F9| RDTSCP     | NP   | Valid      | Valid          | Read 64-bit time-stamp counter and 32-bit
         |            |      |            |                | IA32_TSC_AUX value into EDX:EAX and      
         |            |      |            |                | ECX.                                     

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Loads the current value of the processor's time-stamp counter (a 64-bit MSR)
into the EDX:EAX registers and also loads the IA32_TSC_AUX MSR (address C000_0103H)
into the ECX register. The EDX register is loaded with the high-order 32 bits
of the IA32_TSC MSR; the EAX register is loaded with the low-order 32 bits of
the IA32_TSC MSR; and the ECX register is loaded with the low-order 32-bits
of IA32_TSC_AUX MSR. On processors that support the Intel 64 architecture, the
high-order 32 bits of each of RAX, RDX, and RCX are cleared.

The processor monotonically increments the time-stamp counter MSR every clock
cycle and resets it to 0 whenever the processor is reset. See “Time Stamp Counter”
in Chapter 17 of the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3B, for specific details of the time stamp counter behavior.

When in protected or virtual 8086 mode, the time stamp disable (TSD) flag in
register CR4 restricts the use of the RDTSCP instruction as follows. When the
TSD flag is clear, the RDTSCP instruction can be executed at any privilege level;
when the flag is set, the instruction can only be executed at privilege level
0. (When in real-address mode, the RDTSCP instruction is always enabled.)

The RDTSCP instruction waits until all previous instructions have been executed
before reading the counter.

   | |  
---- | -----
 However, The presence of the RDTSCP       | subsequent instructions may begin execution
 instruction is indicated by CPUID leaf    | before the read operation is performed.    
 80000001H, EDX bit 27. If the bit is      |                                            
 set to 1 then RDTSCP is present on the    |                                            
 processor. See “Changes to Instruction    |                                            
 Behavior in VMX Non-Root Operation”       |                                            
 in Chapter 25 of the Intel® 64 and IA-32  |                                            
 Architectures Software Developer's Manual,|                                            
 Volume 3C, for more information about     |                                            
 the behavior of this instruction in       |                                            
 VMX non-root operation.                   |                                            


### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**| If the TSD flag in register CR4 is set                       
       | and the CPL is greater than 0.                               
 **``#UD``**   | If the LOCK prefix is used. If CPUID.80000001H:EDX.RDTSCP[bit
       | 27] = 0.                                                     

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#UD``**| If the LOCK prefix is used. If CPUID.80000001H:EDX.RDTSCP[bit
    | 27] = 0.                                                     

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**| If the TSD flag in register CR4 is set.                      
 **``#UD``**   | If the LOCK prefix is used. If CPUID.80000001H:EDX.RDTSCP[bit
       | 27] = 0.                                                     

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
