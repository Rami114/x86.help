## CLC - Clear Carry Flag

> Operation

``` slim
CF <- 0;

```

 Opcode| Instruction| Op/En| 64-bit Mode| Compat/Leg Mode| Description   
 ---  | --- | --- | --- | --- | ---
 F8    | CLC        | NP   | Valid      | Valid          | Clear CF flag.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Clears the CF flag in the EFLAGS register. Operation is the same in all non-64-bit
modes and 64-bit mode.



### Flags Affected
The CF flag is set to 0. The OF, ZF, SF, AF, and PF flags are unaffected.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 **``#UD``**| If the LOCK prefix is used.
