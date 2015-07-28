## SLDT - Store Local Descriptor Table Register

> Intel386 processors. When the destination operand is a memory location, the

``` slim
segment selector is written to memory as a 16-bit quantity, regardless of the
operand size.

In compatibility mode, when the destination operand is a 32-bit register, the
16-bit segment selector is copied into the low-order 16 bits of the register.
The high-order 16 bits of the register are cleared. When the destination operand
is a memory location, the segment selector is written to memory as a 16-bit
quantity, regardless of the operand size.

In 64-bit mode, using a REX prefix in the form of REX.R permits access to additional
registers (R8-R15). The behavior of SLDT with a 64-bit register is to zero-extend
the 16-bit selector and store it in the register. If the destination is memory
and operand size is 64, SLDT will write the 16-bit selector to memory as a 16-bit
quantity, regardless of the operand size

> Operation

``` slim

DEST <- LDTR(SegmentSelector);

```

 Opcode*         | Instruction | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                         
 ---  | --- | --- | --- | --- | ---
 0F 00 /0        | SLDT r/m16  | M    | Valid      | Valid          | Stores segment selector from LDTR in
                 |             |      |            |                | r/m16.                              
 REX.W + 0F 00 /0| SLDT r64/m16| M    | Valid      | Valid          | Stores segment selector from LDTR in
                 |             |      |            |                | r64/m16.                            

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (w)| NA       | NA       | NA       

### Description
Stores the segment selector from the local descriptor table register (LDTR)
in the destination operand. The destination operand can be a general-purpose
register or a memory location. The segment selector stored with this instruction
points to the segment descriptor (located in the GDT) for the current LDT. This
instruction can only be executed in protected mode.

Outside IA-32e mode, when the destination operand is a 32-bit register, the
16-bit segment selector is copied into the low-order 16 bits of the register.
The high-order 16 bits of the register are cleared for the Pentium 4, Intel
Xeon, and P6 family processors. They are undefined for Pentium, Intel486, and


### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If the destination is located in a non-writable
                | segment. If a memory operand effective         
                | address is outside the CS, DS, ES, FS,         
                | or GS segment limit. If the DS, ES,            
                | FS, or GS register is used to access           
                | memory and it contains a NULL segment          
                | selector.                                      
 **``#SS(0)``**         | If a memory operand effective address          
                | is outside the SS segment limit.               
 **``#PF(fault-code)``**| If a page fault occurs.                        
 **``#AC(0)``**         | If alignment checking is enabled and           
                | an unaligned memory reference is made          
                | while the current privilege level is           
                | 3.                                             
 **``#UD``**            | If the LOCK prefix is used.                    

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#UD``**| The SLDT instruction is not recognized
    | in real-address mode.                 
If the LOCK prefix is used.


### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#UD``**| The SLDT instruction is not recognized  
    | in virtual-8086 mode. If the LOCK prefix
    | is used.                                

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If a memory address referencing the        
                | SS segment is in a non-canonical form.     
 **``#GP(0)``**         | If the memory address is in a non-canonical
                | form.                                      
 **``#PF(fault-code)``**| If a page fault occurs.                    
 **``#AC(0)``**         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3.                                         
 **``#UD``**            | If the LOCK prefix is used.                
