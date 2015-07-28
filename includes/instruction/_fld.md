## FLD - Load Floating Point Value

> Operation

``` slim
IF SRC is ST(i)
  THEN
     temp <- ST(i);
FI;
TOP <- TOP − 1;
IF SRC is memory-operand
  THEN
     ST(0) <- ConvertToDoubleExtendedPrecisionFP(SRC);
  ELSE (\* SRC is ST(i) \*)
     ST(0) <- temp;
FI;

```

 Opcode | Instruction| 64-Bit Mode| Compat/Leg Mode| Description                            
 ---  | --- | --- | --- | ---
 D9 /0  | FLD m32fp  | Valid      | Valid          | Push m32fp onto the FPU register stack.
 DD /0  | FLD m64fp  | Valid      | Valid          | Push m64fp onto the FPU register stack.
 DB /5  | FLD m80fp  | Valid      | Valid          | Push m80fp onto the FPU register stack.
 D9 C0+i| FLD ST(i)  | Valid      | Valid          | Push ST(i) onto the FPU register stack.

### Description
Pushes the source operand onto the FPU register stack. The source operand can
be in single-precision, doubleprecision, or double extended-precision floating-point
format. If the source operand is in single-precision or double-precision floating-point
format, it is automatically converted to the double extended-precision floatingpoint
format before being pushed on the stack.

The FLD instruction can also push the value in a selected FPU register [ST(i)]
onto the stack. Here, pushing register ST(0) duplicates the stack top.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
   | |  
---- | -----
 C1        | Set to 1 if stack overflow occurred;
           | otherwise, set to 0.                
 C0, C2, C3| Undefined.                          

### Floating-Point Exceptions
   | |  
---- | -----
 **``#IS``**| Stack underflow or overflow occurred.         
 **``#IA``**| Source operand is an SNaN. Does not           
    | occur if the source operand is in double      
    | extended-precision floating-point format      
    | (FLD m80fp or FLD ST(i)).                     
 **``#D``** | Source operand is a denormal value.           
    | Does not occur if the source operand          
    | is in double extended-precision floating-point
    | format.                                       

### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If destination is located in a non-writable
                | segment. If a memory operand effective     
                | address is outside the CS, DS, ES, FS,     
                | or GS segment limit. If the DS, ES,        
                | FS, or GS register is used to access       
                | memory and it contains a NULL segment      
                | selector.                                  
 **``#SS(0)``**         | If a memory operand effective address      
                | is outside the SS segment limit.           
 **``#NM``**            | CR0.EM[bit 2] or CR0.TS[bit 3] = 1.        
 **``#PF(fault-code)``**| If a page fault occurs.                    
 **``#AC(0)``**         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3.                                         
 **``#UD``**            | If the LOCK prefix is used.                

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#GP``**| If a memory operand effective address
    | is outside the CS, DS, ES, FS, or GS 
    | segment limit.                       
 **``#SS``**| If a memory operand effective address
    | is outside the SS segment limit.     
 **``#NM``**| CR0.EM[bit 2] or CR0.TS[bit 3] = 1.  
 **``#UD``**| If the LOCK prefix is used.          

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory operand effective address 
                | is outside the CS, DS, ES, FS, or GS  
                | segment limit.                        
 **``#SS(0)``**         | If a memory operand effective address 
                | is outside the SS segment limit.      
 **``#NM``**            | CR0.EM[bit 2] or CR0.TS[bit 3] = 1.   
 **``#PF(fault-code)``**| If a page fault occurs.               
 **``#AC(0)``**         | If alignment checking is enabled and  
                | an unaligned memory reference is made.
 **``#UD``**            | If the LOCK prefix is used.           

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
                | 3.                                         
 **``#UD``**            | If the LOCK prefix is used.                
