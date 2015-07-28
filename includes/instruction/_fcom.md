## FCOM/FCOMP/FCOMPP - Compare Floating Point Values

> Operation

``` slim
CASE (relation of operands) OF
```

 Opcode | Instruction| 64-Bit Mode| Compat/Leg Mode| Description                              
 ---  | --- | --- | --- | ---
 D8 /2  | FCOM m32fp | Valid      | Valid          | Compare ST(0) with m32fp.                
 DC /2  | FCOM m64fp | Valid      | Valid          | Compare ST(0) with m64fp.                
 D8 D0+i| FCOM ST(i) | Valid      | Valid          | Compare ST(0) with ST(i).                
 D8 D1  | FCOM       | Valid      | Valid          | Compare ST(0) with ST(1).                
 D8 /3  | FCOMP m32fp| Valid      | Valid          | Compare ST(0) with m32fp and pop register
        |            |            |                | stack.                                   
 DC /3  | FCOMP m64fp| Valid      | Valid          | Compare ST(0) with m64fp and pop register
        |            |            |                | stack.                                   
 D8 D8+i| FCOMP ST(i)| Valid      | Valid          | Compare ST(0) with ST(i) and pop register
        |            |            |                | stack.                                   
 D8 D9  | FCOMP      | Valid      | Valid          | Compare ST(0) with ST(1) and pop register
        |            |            |                | stack.                                   
 DE D9  | FCOMPP     | Valid      | Valid          | Compare ST(0) with ST(1) and pop register
        |            |            |                | stack twice.                             

### Description
Compares the contents of register ST(0) and source value and sets condition
code flags C0, C2, and C3 in the FPU status word according to the results (see
the table below). The source operand can be a data register or a memory location.
If no source operand is given, the value in ST(0) is compared with the value
in ST(1). The sign of zero is ignored, so that -0.0 is equal to +0.0.


### Table 3-31. FCOM/FCOMP/FCOMPP Results
   | |  
---- | -----
 Condition| C3 0 0 1 1| C2 0 0 0 1| C0 0 1 0 1
<aside class="notification">
* Flags not set if unmasked invalid-arithmetic-operand (#IA) exception
is generated.
</aside>

This instruction checks the class of the numbers being compared (see “FXAM - Examine
ModR/M” in this chapter). If either operand is a NaN or is in an unsupported
format, an invalid-arithmetic-operand exception (**``#IA)``** is raised and, if the
exception is masked, the condition flags are set to “unordered.” If the invalid-arithmetic-operand
exception is unmasked, the condition code flags are not set.

The FCOMP instruction pops the register stack following the comparison operation
and the FCOMPP instruction pops the register stack twice following the comparison
operation. To pop the register stack, the processor marks the ST(0) register
as empty and increments the stack pointer (TOP) by 1.

The FCOM instructions perform the same operation as the FUCOM instructions.
The only difference is how they handle QNaN operands. The FCOM instructions
raise an invalid-arithmetic-operand exception (**``#IA)``** when either or both of the
operands is a NaN value or is in an unsupported format. The FUCOM instructions
perform the same operation as the FCOM instructions, except that they do not
generate an invalid-arithmetic-operand exception for QNaNs.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



###   ST > SRC
###   ST < SRC
###   ST = SRC
ESAC;
IF ST(0) or SRC = NaN or unsupported format
  THEN
     **``#IA``**
     IF FPUControlWord.IM = 1
       THEN
          C3, C2, C0 <- 111;
     FI;
FI;
IF Instruction = FCOMP
  THEN
     PopRegisterStack;
FI;
IF Instruction = FCOMPP
  THEN
     PopRegisterStack;
     PopRegisterStack;
FI;

### FPU Flags Affected
   | |  
---- | -----
 C1        | Set to 0.                  
 C0, C2, C3| See table on previous page.

### Floating-Point Exceptions
   | |  
---- | -----
 **``#IS``**| Stack underflow occurred.                
 **``#IA``**| One or both operands are NaN values      
    | or have unsupported formats. Register    
    | is marked empty.                         
 **``#D``** | One or both operands are denormal values.

### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory operand effective address
                | is outside the CS, DS, ES, FS, or GS 
                | segment limit. If the DS, ES, FS, or 
                | GS register contains a NULL segment  
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
