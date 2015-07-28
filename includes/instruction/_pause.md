## PAUSE - Spin Loop Hint

> Operation
``` slim

Execute_Next_Instruction(DELAY);

```

 Opcode| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                          
 ---  | --- | --- | --- | --- | ---
 F3 90 | PAUSE      | NP   | Valid      | Valid          | Gives hint to processor that improves
       |            |      |            |                | performance of spin-wait loops.      

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Improves the performance of spin-wait loops. When executing a “spin-wait loop,”
processors will suffer a severe performance penalty when exiting the loop because
it detects a possible memory order violation. The PAUSE instruction provides
a hint to the processor that the code sequence is a spin-wait loop. The processor
uses this hint to avoid the memory order violation in most situations, which
greatly improves processor performance. For this reason, it is recommended that
a PAUSE instruction be placed in all spin-wait loops.

An additional function of the PAUSE instruction is to reduce the power consumed
by a processor while executing a spin loop. A processor can execute a spin-wait
loop extremely quickly, causing the processor to consume a lot of power while
it waits for the resource it is spinning on to become available. Inserting a
pause instruction in a spinwait loop greatly reduces the processor's power consumption.

This instruction was introduced in the Pentium 4 processors, but is backward
compatible with all IA-32 processors. In earlier IA-32 processors, the PAUSE
instruction operates like a NOP instruction. The Pentium 4 and Intel Xeon processors
implement the PAUSE instruction as a delay. The delay is finite and can be zero
for some processors. This instruction does not change the architectural state
of the processor (that is, it performs essentially a delaying no-op operation).

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### Numeric Exceptions
None.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 #UD| If the LOCK prefix is used.
