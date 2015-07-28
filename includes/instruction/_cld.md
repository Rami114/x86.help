## CLD - Clear Direction Flag

> Operation

``` slim
DF <- 0;

```

 Opcode| Instruction| Op/En| 64-bit Mode| Compat/Leg Mode| Description   
 ---  | --- | --- | --- | --- | ---
 FC    | CLD        | NP   | Valid      | Valid          | Clear DF flag.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Clears the DF flag in the EFLAGS register. When the DF flag is set to 0, string
operations increment the index registers (ESI and/or EDI). Operation is the
same in all non-64-bit modes and 64-bit mode.



### Flags Affected
The DF flag is set to 0. The CF, OF, ZF, SF, AF, and PF flags are unaffected.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 **``#UD``**| If the LOCK prefix is used.
