## CMC - Complement Carry Flag

> Operation
``` slim

EFLAGS.CF[bit 0]<- NOT EFLAGS.CF[bit 0];

```

 Opcode| Instruction| Op/En| 64-bit Mode| Compat/Leg Mode| Description        
 ---  | --- | --- | --- | --- | ---
 F5    | CMC        | NP   | Valid      | Valid          | Complement CF flag.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Complements the CF flag in the EFLAGS register. CMC operation is the same in
non-64-bit modes and 64-bit mode.



### Flags Affected
The CF flag contains the complement of its original value. The OF, ZF, SF, AF,
and PF flags are unaffected.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 #UD| If the LOCK prefix is used.
