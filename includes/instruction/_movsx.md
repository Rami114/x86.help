## MOVSX/MOVSXD - Move with Sign-Extension

> Operation
``` slim

DEST <- SignExtend(SRC);

```

 Opcode          | Instruction      | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                                    
 ---  | --- | --- | --- | --- | ---
 0F BE /r        | MOVSX r16, r/m8  | RM   | Valid      | Valid          | Move byte to word with sign-extension.         
 0F BE /r        | MOVSX r32, r/m8  | RM   | Valid      | Valid          | Move byte to doubleword with signextension.    
 REX + 0F BE /r  | MOVSX r64, r/m8\* | RM   | Valid      | N.E.           | Move byte to quadword with sign-extension.     
 0F BF /r        | MOVSX r32, r/m16 | RM   | Valid      | Valid          | Move word to doubleword, with signextension.   
 REX.W + 0F BF /r| MOVSX r64, r/m16 | RM   | Valid      | N.E.           | Move word to quadword with sign-extension.     
 REX.W\*\* + 63 /r | MOVSXD r64, r/m32| RM   | Valid      | N.E.           | Move doubleword to quadword with signextension.
<aside class="notification">
\* In 64-bit mode, r/m8 can not be encoded to access the following byte
registers if a REX prefix is used: AH, BH, CH, DH. \*\* The use of MOVSXD without
REX.W in 64-bit mode is discouraged, Regular MOV should be used instead of using
MOVSXD without REX.W.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Copies the contents of the source operand (register or memory location) to the
destination operand (register) and sign extends the value to 16 or 32 bits (see
Figure 7-6 in the Intel® 64 and IA-32 Architectures Software Developer's Manual,
Volume 1). The size of the converted value depends on the operand-size attribute.

In 64-bit mode, the instruction's default operation size is 32 bits. Use of
the REX.R prefix permits access to additional registers (R8-R15). Use of the
REX.W prefix promotes operation to 64 bits. See the summary chart at the beginning
of this section for encoding data and limits.



### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
 #GP(0)         | If a memory operand effective address
                | is outside the CS, DS, ES, FS, or GS 
                | segment limit. If the DS, ES, FS, or 
                | GS register contains a NULL segment  
                | selector.                            
 #SS(0)         | If a memory operand effective address
                | is outside the SS segment limit.     
 #PF(fault-code)| If a page fault occurs.              
 #AC(0)         | If alignment checking is enabled and 
                | an unaligned memory reference is made
                | while the current privilege level is 
                | 3.                                   
 #UD            | If the LOCK prefix is used.          

### Real-Address Mode Exceptions
   | |  
---- | -----
 #GP| If a memory operand effective address
    | is outside the CS, DS, ES, FS, or GS 
    | segment limit.                       
 #SS| If a memory operand effective address
    | is outside the SS segment limit.     
 #UD| If the LOCK prefix is used.          

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 #GP(0)         | If a memory operand effective address
                | is outside the CS, DS, ES, FS, or GS 
                | segment limit.                       
 #SS(0)         | If a memory operand effective address
                | is outside the SS segment limit.     
 #PF(fault-code)| If a page fault occurs.              
 #UD            | If the LOCK prefix is used.          

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 #SS(0)         | If a memory address referencing the        
                | SS segment is in a non-canonical form.     
 #GP(0)         | If the memory address is in a non-canonical
                | form.                                      
 #PF(fault-code)| If a page fault occurs.                    
 #AC(0)         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3.                                         
 #UD            | If the LOCK prefix is used.                
