## STC - Set Carry Flag

> Operation
``` slim

CF <- 1;

```

 Opcode\*| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description 
 ---  | --- | --- | --- | --- | ---
 F9     | STC        | NP   | Valid      | Valid          | Set CF flag.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Sets the CF flag in the EFLAGS register.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### Flags Affected
The CF flag is set. The OF, ZF, SF, AF, and PF flags are unaffected.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 #UD| If the LOCK prefix is used.
