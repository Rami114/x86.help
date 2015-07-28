## SFENCE - Store Fence

> Operation
``` slim

Wait_On_Following_Stores_Until(preceding_stores_globally_visible);

```

 Opcode\* | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                 
 ---  | --- | --- | --- | --- | ---
 0F AE /7| SFENCE     | NP   | Valid      | Valid          | Serializes store operations.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
Performs a serializing operation on all store-to-memory instructions that were
issued prior the SFENCE instruction. This serializing operation guarantees that
every store instruction that precedes the SFENCE instruction in program order
becomes globally visible before any store instruction that follows the SFENCE
instruction. The SFENCE instruction is ordered with respect to store instructions,
other SFENCE instructions, any LFENCE and MFENCE instructions, and any serializing
instructions (such as the CPUID instruction). It is not ordered with respect
to load instructions.

Weakly ordered memory types can be used to achieve higher processor performance
through such techniques as out-of-order issue, write-combining, and write-collapsing.
The degree to which a consumer of data recognizes or knows that the data is
weakly ordered varies among applications and may be unknown to the producer
of this data. The SFENCE instruction provides a performance-efficient way of
ensuring store ordering between routines that produce weakly-ordered results
and routines that consume this data.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### Intel C/C++ Compiler Intrinsic Equivalent
void _mm_sfence(void)


### Exceptions (All Operating Modes)
   | |  
---- | -----
 #UD| If CPUID.01H:EDX.SSE[bit 25] = 0. If
    | the LOCK prefix is used.            
