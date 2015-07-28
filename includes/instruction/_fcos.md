## FCOS - Cosine

> Operation

``` slim
IF |ST(0)| < 263
THEN
  C2 <- 0;
  ST(0) <- cosine(ST(0));
ELSE (\* Source operand is out-of-range \*)
  C2 <- 1;
FI;

```

 Opcode| Instruction| 64-Bit Mode| Compat/Leg Mode| Description                   
 ---  | --- | --- | --- | ---
 D9 FF | FCOS       | Valid      | Valid          | Replace ST(0) with its cosine.

### Description
Computes the cosine of the source operand in register ST(0) and stores the result
in ST(0). The source operand must be given in radians and must be within the
range −263 to +263. The following table shows the results obtained when taking
the cosine of various classes of numbers.


### Table 3-33. FCOS Results
   | |  
---- | -----
 ST(0) SRC − ∞| ST(0) DEST
 − F          | −1 to +1  
 − 0          | +1        
 + 0          | +1        
 + F + ∞      | − 1 to + 1
 NaN          | NaN       
<aside class="notification">
F Means finite floating-point value.
</aside>

   | |  
---- | -----
 \*| Indicates floating-point invalid-arithmetic-operand
  | (**``#IA)``** exception.                                   
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
 C1    | Set to 0 if stack underflow occurred.   
       | Set if result was rounded up; cleared   
       | otherwise. Undefined if C2 is 1.        
 C2    | Set to 1 if outside range (−263 < source
       | operand < +263); otherwise, set to 0.   
 C0, C3| Undefined.                              

### Floating-Point Exceptions
   | |  
---- | -----
 **``#IS``**| Stack underflow occurred.          
 **``#IA``**| Source operand is an SNaN value, ∞,
    | or unsupported format.             
 **``#D``** | Source is a denormal value.        
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
