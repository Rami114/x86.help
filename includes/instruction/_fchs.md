## FCHS - Change Sign

> Operation
``` slim

SignBit(ST(0)) <- NOT (SignBit(ST(0)));

```

 Opcode| Instruction| 64-Bit Mode| Compat/Leg Mode| Description               
 ---  | --- | --- | --- | ---
 D9 E0 | FCHS       | Valid      | Valid          | Complements sign of ST(0).

### Description
Complements the sign bit of ST(0). This operation changes a positive value into
a negative value of equal magnitude or vice versa. The following table shows
the results obtained when changing the sign of various classes of numbers.


### Table 3-30. FCHS Results
   | |  
---- | -----
 ST(0) SRC| ST(0) DEST + ∞+ F + 0 − 0 − F − ∞NaN
<aside class="notification">
\* F means finite floating-point value.
</aside>

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
   | |  
---- | -----
 C1        | Set to 0. 
 C0, C2, C3| Undefined.

### Floating-Point Exceptions
   | |  
---- | -----
 #IS| Stack underflow occurred.

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
