## LAHF - Load Status Flags into AH Register

> Operation
``` slim

IF 64-Bit Mode
  THEN
     IF CPUID.80000001H:ECX.LAHF-SAHF[bit 0] = 1;
       THEN AH <- RFLAGS(SF:ZF:0:AF:0:PF:1:CF);
       ELSE #UD;
     FI;
  ELSE
     AH <- EFLAGS(SF:ZF:0:AF:0:PF:1:CF);
FI;

```

 Opcode| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                             
 ---  | --- | --- | --- | --- | ---
 9F    | LAHF       | NP   | Invalid\*   | Valid          | Load: AH ← EFLAGS(SF:ZF:0:AF:0:PF:1:CF).
<aside class="notification">
\*Valid in specific steppings. See Description section.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
This instruction executes as described above in compatibility mode and legacy
mode. It is valid in 64-bit mode only if CPUID.80000001H:ECX.LAHF-SAHF[bit 0]
= 1.



### Flags Affected
None. The state of the flags in the EFLAGS register is not affected.


### Protected Mode Exceptions
   | |  
---- | -----
 #UD| If the LOCK prefix is used.

### Real-Address Mode Exceptions
Same exceptions as in protected mode.


### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 #UD| If CPUID.80000001H:ECX.LAHF-SAHF[bit
    | 0] = 0. If the LOCK prefix is used. 
