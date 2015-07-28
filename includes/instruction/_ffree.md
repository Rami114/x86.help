## FFREE - Free Floating-Point Register

> Operation
``` slim

TAG(i) <- 11B;

```

 Opcode | Instruction| 64-Bit Mode| Compat/Leg Mode| Description                 
 ---  | --- | --- | --- | ---
 DD C0+i| FFREE ST(i)| Valid      | Valid          | Sets tag for ST(i) to empty.

### Description
Sets the tag in the FPU tag register associated with register ST(i) to empty
(11B). The contents of ST(i) and the FPU stack-top pointer (TOP) are not affected.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
C0, C1, C2, C3 undefined.


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
