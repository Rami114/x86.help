## FSINCOS - Sine and Cosine

> Operation

``` slim
IF ST(0) < 263
  THEN
     C2 <- 0;
     TEMP <- cosine(ST(0));
     ST(0) <- sine(ST(0));
     TOP <- TOP − 1;
     ST(0) <- TEMP;
  ELSE (\* Source operand out of range \*)
     C2 <- 1;
FI;

```

 Opcode| Instruction| 64-Bit Mode| Compat/Leg Mode| Description                          
 ---  | --- | --- | --- | ---
 D9 FB | FSINCOS    | Valid      | Valid          | Compute the sine and cosine of ST(0);
       |            |            |                | replace ST(0) with the sine, and push
       |            |            |                | the cosine onto the register stack.  

### Description
Computes both the sine and the cosine of the source operand in register ST(0),
stores the sine in ST(0), and pushes the cosine onto the top of the FPU register
stack. (This instruction is faster than executing the FSIN and FCOS instructions
in succession.) The source operand must be given in radians and must be within
the range −263 to +263. The following table shows the results obtained when
taking the sine and cosine of various classes of numbers, assuming that underflow
does not occur.


### Table 3-46. FSINCOS Results
   | |  
---- | -----
 SRC − ∞| DEST ST(0) Sine
 − F    | − 1 to + 1     
 − 0    | − 0            
 + 0    | + 0            
 + F + ∞| − 1 to + 1     
 NaN    | NaN            
<aside class="notification">
F Means finite floating-point value. \* Indicates floating-point invalid-arithmetic-operand
(#IA) exception.
</aside>

If the source operand is outside the acceptable range, the C2 flag in the FPU
status word is set, and the value in register ST(0) remains unchanged. The instruction
does not raise an exception when the source operand is out of range. It is up
to the program to check the C2 flag for out-of-range conditions. Source values
outside the range −263 to +263 can be reduced to the range of the instruction
by subtracting an appropriate integer multiple of 2π or by using the FPREM instruction
with a divisor of 2π. See the section titled “Pi” in Chapter 8 of the Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1, for a discussion
of the proper value to use for π in performing such reductions.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
   | |  
---- | -----
 C1    | Set to 0 if stack underflow occurred;       
       | set to 1 of stack overflow occurs. Set      
       | if result was rounded up; cleared otherwise.
 C2    | Set to 1 if outside range (−263 < source    
       | operand < +263); otherwise, set to 0.       
 C0, C3| Undefined.                                  

### Floating-Point Exceptions
   | |  
---- | -----
 **``#IS``**| Stack underflow or overflow occurred.
 **``#IA``**| Source operand is an SNaN value, ∞,  
    | or unsupported format.               
 **``#D``** | Source operand is a denormal value.  
 **``#U``** | Result is too small for destination  
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
