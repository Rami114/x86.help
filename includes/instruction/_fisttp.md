## FISTTP - Store Integer with Truncation

> Operation

``` slim
DEST <- ST;
pop ST;

```

 Opcode| Instruction  | 64-Bit Mode| Compat/Leg Mode| Description                           
 ---  | --- | --- | --- | ---
 DF /1 | FISTTP m16int| Valid      | Valid          | Store ST(0) in m16int with truncation.
 DB /1 | FISTTP m32int| Valid      | Valid          | Store ST(0) in m32int with truncation.
 DD /1 | FISTTP m64int| Valid      | Valid          | Store ST(0) in m64int with truncation.

### Description
FISTTP converts the value in ST into a signed integer using truncation (chop)
as rounding mode, transfers the result to the destination, and pop ST. FISTTP
accepts word, short integer, and long integer destinations.

The following table shows the results obtained when storing various classes
of numbers in integer format.


### Table 3-38. FISTTP Results
   | |  
---- | -----
 ST(0)                             | DEST     
 Value Too Large for DEST Format   | − ∞ or   
 − 1                               | − I 0 + I
 or Value Too Large for DEST Format| + ∞NaN   
<aside class="notification">
F Means finite floating-point value. Ι Means integer. \* Indicates floating-point
invalid-operation (#IA) exception.
</aside>

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### Flags Affected
C1 is cleared; C0, C2, C3 undefined.


### Numeric Exceptions
Invalid, Stack Invalid (stack underflow), Precision.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If the destination is in a nonwritable   
                | segment. For an illegal memory operand   
                | effective address in the CS, DS, ES,     
                | FS or GS segments.                       
 **``#SS(0)``**         | For an illegal address in the SS segment.
 **``#PF(fault-code)``**| For a page fault.                        
 **``#AC(0)``**         | If alignment checking is enabled and     
                | an unaligned memory reference is made    
                | while the current privilege level is     
                | 3.                                       
 **``#NM``**            | If CR0.EM[bit 2] = 1. If CR0.TS[bit      
                | 3] = 1.                                  
 **``#UD``**            | If CPUID.01H:ECX.SSE3[bit 0] = 0. If     
                | the LOCK prefix is used.                 

### Real Address Mode Exceptions
   | |  
---- | -----
 GP(0)| If any part of the operand would lie  
      | outside of the effective address space
      | from 0 to 0FFFFH.                     
 **``#NM``**  | If CR0.EM[bit 2] = 1. If CR0.TS[bit   
      | 3] = 1.                               
 **``#UD``**  | If CPUID.01H:ECX.SSE3[bit 0] = 0. If  
      | the LOCK prefix is used.              

### Virtual 8086 Mode Exceptions
   | |  
---- | -----
 GP(0)          | If any part of the operand would lie  
                | outside of the effective address space
                | from 0 to 0FFFFH.                     
 **``#NM``**            | If CR0.EM[bit 2] = 1. If CR0.TS[bit   
                | 3] = 1.                               
 **``#UD``**            | If CPUID.01H:ECX.SSE3[bit 0] = 0. If  
                | the LOCK prefix is used.              
 **``#PF(fault-code)``**| For a page fault.                     
 **``#AC(0)``**         | For unaligned memory reference if the 
                | current privilege is 3.               

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If a memory address referencing the        
                | SS segment is in a non-canonical form.     
 **``#GP(0)``**         | If the memory address is in a non-canonical
                | form.                                      
 **``#NM``**            | CR0.EM[bit 2] or CR0.TS[bit 3] = 1.        
 **``#MF``**            | If there is a pending x87 FPU exception.   
 **``#PF(fault-code)``**| If a page fault occurs.                    
 **``#AC(0)``**         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3. If the LOCK prefix is used.             
