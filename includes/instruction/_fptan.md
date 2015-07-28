## FPTAN - Partial Tangent

> Operation
``` slim

IF ST(0) < 263
  THEN
     C2 <- 0;
     ST(0) <- tan(ST(0));
     TOP <- TOP − 1;
     ST(0) <- 1.0;
  ELSE (\* Source operand is out-of-range \*)
     C2 <- 1;
FI;

```

 Opcode| Instruction| 64-Bit Mode| Compat/Leg Mode| Description                            
 ---  | --- | --- | --- | ---
 D9 F2 | FPTAN      | Valid      | Valid          | Replace ST(0) with its tangent and push
       |            |            |                | 1 onto the FPU stack.                  

### Description
Computes the tangent of the source operand in register ST(0), stores the result
in ST(0), and pushes a 1.0 onto the FPU register stack. The source operand must
be given in radians and must be less than ±263. The following table shows the
unmasked results obtained when computing the partial tangent of various classes
of numbers, assuming that underflow does not occur.


### Table 3-43. FPTAN Results
   | |  
---- | -----
 ST(0) SRC − ∞| ST(0) DEST
 − F          | − F to + F
 − 0          | -0        
 + 0          | +0        
 + F + ∞      | − F to + F
 NaN          | NaN       
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

The value 1.0 is pushed onto the register stack after the tangent has been computed
to maintain compatibility with the Intel 8087 and Intel287 math coprocessors.
This operation also simplifies the calculation of other trigonometric functions.
For instance, the cotangent (which is the reciprocal of the tangent) can be
computed by executing a FDIVR instruction after the FPTAN instruction.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
   | |  
---- | -----
 C1    | Set to 0 if stack underflow occurred;   
       | set to 1 if stack overflow occurred.    
       | Set if result was rounded up; cleared   
       | otherwise.                              
 C2    | Set to 1 if outside range (−263 < source
       | operand < +263); otherwise, set to 0.   
 C0, C3| Undefined.                              

### Floating-Point Exceptions
   | |  
---- | -----
 #IS| Stack underflow or overflow occurred.
 #IA| Source operand is an SNaN value, ∞,  
    | or unsupported format.               
 #D | Source operand is a denormal value.  
 #U | Result is too small for destination  
    | format.                              
 #P | Value cannot be represented exactly  
    | in destination format.               

### Protected Mode Exceptions
   | |  
---- | -----
 #NM| CR0.EM[bit 2] or CR0.TS[bit 3] = 1.     
 #MF| If there is a pending x87 FPU exception.
 #UD| If the LOCK prefix is used.             

### Real-Address Mode Exceptions
Same exceptions as in protected mode.


### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
