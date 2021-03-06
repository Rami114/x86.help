## POPCNT  -  Return the Count of Number of Bits Set to 1

> Operation

``` slim
Count = 0;
For (i=0; i < OperandSize; i++)
{
     THEN Count++; FI;
}
DEST <- Count;```

### Flags Affected
OF, SF, ZF, AF, CF, PF are all cleared. ZF is set if SRC = 0, otherwise ZF is
cleared


> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 POPCNT:| int _mm_popcnt_u32(unsigned int a);    
 POPCNT:| int64_t _mm_popcnt_u64(unsigned __int64
        | a);                                    

```

 Opcode           | Instruction      | Op/En| 64-Bit Mode| Compat/Leg Mode| Description    
 ---  | --- | --- | --- | --- | ---
 F3               | POPCNT r16, r/m16| RM   | Valid      | Valid          | POPCNT on r/m16
 F3               | POPCNT r32, r/m32| RM   | Valid      | Valid          | POPCNT on r/m32
 F3 REX.W 0F B8 /r| POPCNT r64, r/m64| RM   | Valid      | N.E.           | POPCNT on r/m64

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
This instruction calculates of number of bits set to 1 in the second operand
(source) and returns the count in the first operand (a destination register).



### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**          | If a memory operand effective address    
                 | is outside the CS, DS, ES, FS or GS      
                 | segments.                                
 **``#SS(0)``**          | If a memory operand effective address    
                 | is outside the SS segment limit.         
 **``#PF``** (fault-code)| For a page fault.                        
 **``#AC(0)``**          | If an unaligned memory reference is      
                 | made while the current privilege level   
                 | is 3 and alignment checking is enabled.  
 **``#UD``**             | If CPUID.01H:ECX.POPCNT [Bit 23] = 0.    
                 | If LOCK prefix is used. Either the prefix
                 | REP (F3h) or REPN (F2H) is used.         

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**| If any part of the operand lies outside  
       | of the effective address space from      
       | 0 to 0FFFFH.                             
 **``#SS(0)``**| If a memory operand effective address    
       | is outside the SS segment limit.         
 **``#UD``**   | If CPUID.01H:ECX.POPCNT [Bit 23] = 0.    
       | If LOCK prefix is used. Either the prefix
       | REP (F3h) or REPN (F2H) is used.         

### Virtual 8086 Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**          | If any part of the operand lies outside  
                 | of the effective address space from      
                 | 0 to 0FFFFH.                             
 **``#SS(0)``**          | If a memory operand effective address    
                 | is outside the SS segment limit.         
 **``#PF``** (fault-code)| For a page fault.                        
 **``#AC(0)``**          | If an unaligned memory reference is      
                 | made while alignment checking is enabled.
 **``#UD``**             | If CPUID.01H:ECX.POPCNT [Bit 23] = 0.    
                 | If LOCK prefix is used. Either the prefix
                 | REP (F3h) or REPN (F2H) is used.         

### Compatibility Mode Exceptions
Same exceptions as in Protected Mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**          | If the memory address is in a non-canonical
                 | form.                                      
 **``#SS(0)``**          | If a memory address referencing the        
                 | SS segment is in a non-canonical form.     
 **``#PF``** (fault-code)| For a page fault.                          
 **``#AC(0)``**          | If alignment checking is enabled and       
                 | an unaligned memory reference is made      
                 | while the current privilege level is       
                 | 3.                                         
 **``#UD``**             | If CPUID.01H:ECX.POPCNT [Bit 23] = 0.      
                 | If LOCK prefix is used. Either the prefix  
                 | REP (F3h) or REPN (F2H) is used.           
