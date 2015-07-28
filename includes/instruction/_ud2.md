## UD2 - Undefined Instruction

> Operation
``` slim

#UD (\* Generates invalid opcode exception \*);

```

 Opcode| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                    
 ---  | --- | --- | --- | --- | ---
 0F 0B | UD2        | NP   | Valid      | Valid          | Raise invalid opcode exception.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Generates an invalid opcode exception. This instruction is provided for software
testing to explicitly generate an invalid opcode exception. The opcode for this
instruction is reserved for this purpose.

Other than raising the invalid opcode exception, this instruction has no effect
on processor state or memory.

Even though it is the execution of the UD2 instruction that causes the invalid
opcode exception, the instruction pointer saved by delivery of the exception
references the UD2 instruction (and not the following instruction).

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### Flags Affected
None.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 #UD| Raises an invalid opcode exception in
    | all operating modes.                 
