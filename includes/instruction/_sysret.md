## SYSRET - Return From Fast System Call

> Operation
``` slim

IF (CS.L != 1 ) or (IA32_EFER.LMA != 1) or (IA32_EFER.SCE != 1)
(\* Not in 64-Bit Mode or SYSCALL/SYSRET not enabled in IA32_EFER \*)
  THEN #UD; FI;
IF (CPL != 0) OR (RCX is not canonical) THEN #GP(0); FI;
1.
  in RFLAGS retain the fixed values.
IF (operand size is 64-bit)
  THEN (\* Return to 64-Bit Mode \*)
     RIP <- RCX;
  ELSE (\* Return to Compatibility Mode \*)
     RIP <- ECX;
FI;
RFLAGS <- (R11 & 3C7FD7H) | 2;
IF (operand size is 64-bit)
  THEN CS.Selector <- IA32_STAR[63:48]+16;
  ELSE CS.Selector <- IA32_STAR[63:48];
FI;
CS.Selector <- CS.Selector OR 3;
(\* Set rest of CS to a fixed value \*)
CS.Base <- 0;
CS.Limit <- FFFFFH;
CS.Type <- 11;
CS.S <- 1;
CS.DPL <- 3;
CS.P <- 1;
IF (operand size is 64-bit)
  THEN (\* Return to 64-Bit Mode \*)
     CS.L <- 1;
     CS.D <- 0;
  ELSE (\* Return to Compatibility Mode \*)
     CS.L <- 0;
     CS.D <- 1;
FI;
CS.G <- 1;
CPL <- 0;
SS.Selector <- (IA32_STAR[63:48]+8) OR 3;
(\* Set rest of SS to a fixed value \*)
SS.Base <- 0;
SS.Limit <- FFFFFH;
SS.Type <- 3;
SS.S <- 1;
SS.DPL <- 3;
SS.P <- 1;
SS.B <- 1;
SS.G <- 1;

```

 Opcode       | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                           
 ---  | --- | --- | --- | --- | ---
 0F 07        | SYSRET     | NP   | Valid      | Invalid        | Return to compatibility mode from fast
              |            |      |            |                | system call                           
 REX.W + 0F 07| SYSRET     | NP   | Valid      | Invalid        | Return to 64-bit mode from fast system
              |            |      |            |                | call                                  

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
SYSRET is a companion instruction to the SYSCALL instruction. It returns from
an OS system-call handler to user code at privilege level 3. It does so by loading
RIP from RCX and loading RFLAGS from R11.1 With a 64-bit operand size, SYSRET
remains in 64-bit mode; otherwise, it enters compatibility mode and only the
low 32 bits of the registers are loaded.

SYSRET loads the CS and SS selectors with values derived from bits 63:48 of
the IA32_STAR MSR. However, the CS and SS descriptor caches are not loaded from
the descriptors (in GDT or LDT) referenced by those selectors. Instead, the
descriptor caches are loaded with fixed values. See the Operation section for
details. It is the responsibility of OS software to ensure that the descriptors
(in GDT or LDT) referenced by those selector values correspond to the fixed
values loaded into the descriptor caches; the SYSRET instruction does not ensure
this correspondence.

The SYSRET instruction does not modify the stack pointer (ESP or RSP). For that
reason, it is necessary for software to switch to the user stack. The OS may
load the user stack pointer (if it was saved after SYSCALL) before executing
SYSRET; alternatively, user code may load the stack pointer (if it was saved
before SYSCALL) after receiving control from SYSRET.

If the OS loads the stack pointer before executing SYSRET, it must ensure that
the handler of any interrupt or exception delivered between restoring the stack
pointer and successful execution of SYSRET is not invoked with the user stack.
### It can do so using approaches such as the following

 - External interrupts. The OS can prevent an external interrupt from being delivered
by clearing EFLAGS.IF before loading the user stack pointer.
 - Nonmaskable interrupts (NMIs). The OS can ensure that the NMI handler is invoked
with the correct stack by using the interrupt stack table (IST) mechanism for
gate 2 (NMI) in the IDT (see Section 6.14.5, “Interrupt Stack Table,” in Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 3A).
 - General-protection exceptions (#GP). The SYSRET instruction generates #GP(0)
if the value of RCX is not canonical. The OS can address this possibility using
one or more of the following approaches:  - Confirming that the value of RCX is
canonical before executing SYSRET.  - Using paging to ensure that the SYSCALL
instruction will never save a non-canonical value into RCX.  - Using the IST mechanism
for gate 13 (#GP) in the IDT.



### Flags Affected
All.


### Protected Mode Exceptions
   | |  
---- | -----
 #UD| The SYSRET instruction is not recognized
    | in protected mode.                      

### Real-Address Mode Exceptions
   | |  
---- | -----
 #UD| The SYSRET instruction is not recognized
    | in real-address mode.                   

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 #UD| The SYSRET instruction is not recognized
    | in virtual-8086 mode.                   

### Compatibility Mode Exceptions
   | |  
---- | -----
 #UD| The SYSRET instruction is not recognized
    | in compatibility mode.                  

### 64-Bit Mode Exceptions
   | |  
---- | -----
 #UD   | If IA32_EFER.SCE = 0. If the LOCK prefix   
       | is used.                                   
 #GP(0)| If CPL != 0. If RCX contains a non-canonical
       | address.                                   
