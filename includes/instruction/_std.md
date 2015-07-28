## STD - Set Direction Flag

> Operation

``` slim
DF <- 1;

```

 Opcode\*| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description 
 ---  | --- | --- | --- | --- | ---
 FD     | STD        | NP   | Valid      | Valid          | Set DF flag.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Sets the DF flag in the EFLAGS register. When the DF flag is set to 1, string
operations decrement the index registers (ESI and/or EDI).

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### Flags Affected
The DF flag is set. The CF, OF, ZF, SF, AF, and PF flags are unaffected.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 **``#UD``**| If the LOCK prefix is used.
