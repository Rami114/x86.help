## FXAM - Examine ModR/M

> Operation
``` slim

C1 <- sign bit of ST; (\* 0 for positive, 1 for negative \*)
CASE (class of value or number in ST(0)) OF
  Unsupported:C3, C2, C0 <- 000;
```

 Opcode| Instruction| 64-Bit Mode| Compat/Leg Mode| Description                       
 ---  | --- | --- | --- | ---
 D9 E5 | FXAM       | Valid      | Valid          | Classify value or number in ST(0).

### Description
Examines the contents of the ST(0) register and sets the condition code flags
C0, C2, and C3 in the FPU status word to indicate the class of value or number
in the register (see the table below).


### Table 3-52. FXAM Results
   | |  
---- | -----
 Class| C3 0 0 0 0 1 1 1| C2 0 0 1 1 0 0 1| C0 0 1 0 1 0 1 0
The C1 flag is set to the sign of the value in ST(0), regardless of whether
the register is empty or full.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



###   NaN
###   Normal
###   Infinity
###   Zero
###   Empty
###   Denormal
ESAC;

### FPU Flags Affected
   | |  
---- | -----
 C1        | Sign of value in ST(0).
 C0, C2, C3| See Table 3-52.        

### Floating-Point Exceptions
None.


### Protected Mode Exceptions
   | |  
---- | -----
 #NM| CR0.EM[bit 2] or CR0.TS[bit 3] = 1.     
 #MF| If there is a pending x87 FPU exception.
 #UD| If the LOCK prefix is used.             

### Real-Address Mode Exceptions
Same exceptions as in protected mode.


### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
