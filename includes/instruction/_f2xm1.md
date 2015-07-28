## F2XM1 - Compute 2x-1

> Operation
``` slim

ST(0) <- (2ST(0) − 1);

```

 Opcode| Instruction| 64-Bit Mode| Compat/Leg Mode| Description                     
 ---  | --- | --- | --- | ---
 D9 F0 | F2XM1      | Valid      | Valid          | Replace ST(0) with (2ST(0) - 1).

### Description
Computes the exponential value of 2 to the power of the source operand minus
1. The source operand is located in register ST(0) and the result is also stored
in ST(0). The value of the source operand must lie in the range -1.0 to +1.0.
If the source value is outside this range, the result is undefined.

The following table shows the results obtained when computing the exponential
value of various classes of numbers, assuming that neither overflow nor underflow
occurs.


### Table 3-26. Results Obtained from F2XM1
   | |  
---- | -----
 ST(0) SRC  | ST(0) DEST  
 − 1.0 to −0| − 0.5 to − 0
 − 0        | − 0         
 + 0        | + 0         
 + 0 to +1.0| + 0 to 1.0  
### Values other than 2 can be exponentiated using the following formula

xy ← 2(y \* log2x)

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
   | |  
---- | -----
 C1        | Set to 0 if stack underflow occurred.
           | Set if result was rounded up; cleared
           | otherwise.                           
 C0, C2, C3| Undefined.                           

### Floating-Point Exceptions
   | |  
---- | -----
 #IS| Stack underflow occurred.                     
 #IA| Source operand is an SNaN value or unsupported
    | format.                                       
 #D | Source is a denormal value.                   
 #U | Result is too small for destination           
    | format.                                       
 #P | Value cannot be represented exactly           
    | in destination format.                        

### Protected Mode Exceptions
   | |  
---- | -----
 #NM| CR0.EM[bit 2] or CR0.TS[bit 3] = 1.
 #UD| If the LOCK prefix is used.        

### Real-Address Mode Exceptions
Same exceptions as in protected mode.


### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
