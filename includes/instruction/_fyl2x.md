## FYL2X - Compute y * log2x

> Operation

``` slim
ST(1) <- ST(1) * log<sub>2</sub>ST(0);
PopRegisterStack;

```

 Opcode| Instruction| 64-Bit Mode| Compat/Leg Mode| Description                                      
 ---  | --- | --- | --- | ---
 D9 F1 | FYL2X      | Valid      | Valid          | Replace ST(1) with (ST(1) * log<sub>2</sub>ST(0))
       |            |            |                | and pop the register stack.                      

### Description
Computes (ST(1) \* log<sub>2</sub> (ST(0))), stores the result in resister ST(1),
and pops the FPU register stack. The source operand in ST(0) must be a non-zero
positive number.

The following table shows the results obtained when taking the log of various
classes of numbers, assuming that neither overflow nor underflow occurs.


### Table 3-58. FYL2X Results
ST(0)

   | |  
---- | -----
 − ∞| − F| ±0 + ∞****− ∞| +0<+F<+1 + ∞+ F + 0 − 0 − F − ∞| + 1 −0 −0 +0 +0| + F > + 1 − ∞− F − 0 + 0 + F + ∞| + ∞− ∞− ∞+∞+∞| NaN NaN NaN NaN NaN NaN NaN
 NaN| NaN| NaN          | NaN                            | NaN            | NaN                             | NaN          | NaN                        
<aside class="notification">
F Means finite floating-point value. * Indicates floating-point invalid-operation
(**``#IA)``** exception. ** Indicates floating-point zero-divide (**``#Z)``** exception.
</aside>

If the divide-by-zero exception is masked and register ST(0) contains ±0, the
instruction returns ∞ with a sign that is the opposite of the sign of the source
operand in register ST(1).

The FYL2X instruction is designed with a built-in multiplication to optimize
### the calculation of logarithms with an arbitrary positive base (b)

log<sub>b</sub>x ← (log<sub>2</sub>b)-1 * log<sub>2</sub>x

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
   | |  
---- | -----
 C1        | Set to 0 if stack underflow occurred.
           | Set if result was rounded up; cleared
           | otherwise.                           
 C0, C2, C3| Undefined.                           

### Floating-Point Exceptions
   | |  
---- | -----
 **``#IS``**| Stack underflow occurred.               
 **``#IA``**| Either operand is an SNaN or unsupported
    | format. Source operand in register ST(0)
    | is a negative finite value (not -0).    
 **``#Z``** | Source operand in register ST(0) is     
    | ±0.                                     
 **``#D``** | Source operand is a denormal value.     
 **``#U``** | Result is too small for destination     
    | format.                                 
 **``#O``** | Result is too large for destination     
    | format.                                 
 **``#P``** | Value cannot be represented exactly     
    | in destination format.                  

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
