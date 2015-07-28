## FUCOM/FUCOMP/FUCOMPP - Unordered Compare Floating Point Values

> Operation

``` slim
CASE (relation of operands) OF
```

 Opcode | Instruction | 64-Bit Mode| Compat/Leg Mode| Description                              
 ---  | --- | --- | --- | ---
 DD E0+i| FUCOM ST(i) | Valid      | Valid          | Compare ST(0) with ST(i).                
 DD E1  | FUCOM       | Valid      | Valid          | Compare ST(0) with ST(1).                
 DD E8+i| FUCOMP ST(i)| Valid      | Valid          | Compare ST(0) with ST(i) and pop register
        |             |            |                | stack.                                   
 DD E9  | FUCOMP      | Valid      | Valid          | Compare ST(0) with ST(1) and pop register
        |             |            |                | stack.                                   
 DA E9  | FUCOMPP     | Valid      | Valid          | Compare ST(0) with ST(1) and pop register
        |             |            |                | stack twice.                             

### Description
Performs an unordered comparison of the contents of register ST(0) and ST(i)
and sets condition code flags C0, C2, and C3 in the FPU status word according
to the results (see the table below). If no operand is specified, the contents
of registers ST(0) and ST(1) are compared. The sign of zero is ignored, so that
-0.0 is equal to +0.0.


### Table 3-51. FUCOM/FUCOMP/FUCOMPP Results
   | |  
---- | -----
 Comparison Results*| C3| C2| C0
 ST0 > ST(i)        | 0 | 0 | 0 
 ST0 < ST(i)        | 0 | 0 | 1 
 ST0 = ST(i)        | 1 | 0 | 0 
 Unordered          | 1 | 1 | 1 
<aside class="notification">
* Flags not set if unmasked invalid-arithmetic-operand (#IA) exception
is generated.
</aside>

An unordered comparison checks the class of the numbers being compared (see
“FXAM - Examine ModR/M” in this chapter). The FUCOM/FUCOMP/FUCOMPP instructions
perform the same operations as the FCOM/FCOMP/FCOMPP instructions. The only
difference is that the FUCOM/FUCOMP/FUCOMPP instructions raise the invalid-arithmeticoperand
exception (**``#IA)``** only when either or both operands are an SNaN or are in an unsupported
format; QNaNs cause the condition code flags to be set to unordered, but do
not cause an exception to be generated. The FCOM/FCOMP/FCOMPP instructions raise
an invalid-operation exception when either or both of the operands are a NaN
value of any kind or are in an unsupported format.

As with the FCOM/FCOMP/FCOMPP instructions, if the operation results in an invalid-arithmetic-operand
exception being raised, the condition code flags are set only if the exception
is masked.

The FUCOMP instruction pops the register stack following the comparison operation
and the FUCOMPP instruction pops the register stack twice following the comparison
operation. To pop the register stack, the processor marks the ST(0) register
as empty and increments the stack pointer (TOP) by 1.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



###   ST > SRC
###   ST < SRC
###   ST = SRC
ESAC;
IF ST(0) or SRC = QNaN, but not SNaN or unsupported format
  THEN
     C3, C2, C0 <- 111;
  ELSE (* ST(0) or SRC is SNaN or unsupported format *)
     **``#IA;``**
     IF FPUControlWord.IM = 1
       THEN
          C3, C2, C0 <- 111;
     FI;
FI;
IF Instruction = FUCOMP
  THEN
     PopRegisterStack;
FI;
IF Instruction = FUCOMPP
  THEN
     PopRegisterStack;
FI;

### FPU Flags Affected
   | |  
---- | -----
 C1        | Set to 0 if stack underflow occurred.
 C0, C2, C3| See Table 3-51.                      

### Floating-Point Exceptions
   | |  
---- | -----
 **``#IS``**| Stack underflow occurred.                
 **``#IA``**| One or both operands are SNaN values     
    | or have unsupported formats. Detection   
    | of a QNaN value in and of itself does    
    | not raise an invalid-operand exception.  
 **``#D``** | One or both operands are denormal values.

### Protected Mode Exceptions
   | |  
---- | -----
 **``#NM``**| CR0.EM[bit 2] or CR0.TS[bit 3] = 1.     
 **``#MF``**| If there is a pending x87 FPU exception.
 **``#UD``**| If the LOCK prefix is used.             

### Real-Address Mode Exceptions
Same exceptions as in protected mode.


### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
