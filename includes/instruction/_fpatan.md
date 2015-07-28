## FPATAN - Partial Arctangent

> Operation
``` slim

ST(1) <- arctan(ST(1) / ST(0));
PopRegisterStack;

```

 Opcode\*                              | Instruction| 64-Bit Mode| Compat/Leg Mode| Description                           
 ---  | --- | --- | --- | ---
 D9 F3 NOTES: \* See IA-32 Architecture| FPATAN     | Valid      | Valid          | Replace ST(1) with arctan(ST(1)/ST(0))
 Compatibility section below.         |            |            |                | and pop the register stack.           

### Description
Computes the arctangent of the source operand in register ST(1) divided by the
source operand in register ST(0), stores the result in ST(1), and pops the FPU
register stack. The result in register ST(0) has the same sign as the source
operand ST(1) and a magnitude less than +π.

The FPATAN instruction returns the angle between the X axis and the line from
the origin to the point (X,Y), where Y (the ordinate) is ST(1) and X (the abscissa)
is ST(0). The angle depends on the sign of X and Y independently, not just on
the sign of the ratio Y/X. This is because a point (−X,Y) is in the second quadrant,
resulting in an angle between π/2 and π, while a point (X,−Y) is in the fourth
quadrant, resulting in an angle between 0 and −π/2. A point (−X,−Y) is in the
third quadrant, giving an angle between −π/2 and −π.

The following table shows the results obtained when computing the arctangent
of various classes of numbers, assuming that underflow does not occur.


### Table 3-40. FPATAN Results
ST(0)

   | |  
---- | -----
 − ∞    | − F       | − 0  | + 0  | + F       | + ∞   | NaN
 − 3π/4\*| − π/2     | − π/2| − π/2| − π/2     | − π/4\*| NaN
 -p     | −π to −π/2| −π/2 | −π/2 | −π/2 to −0| - 0   | NaN
 -p     | -p        | -p\*  | − 0\* | − 0       | − 0   | NaN
 +p     | + p       | + π\* | + 0\* | + 0       | + 0   | NaN
 +p     | +π to +π/2| + π/2| +π/2 | +π/2 to +0| + 0   | NaN
 +3π/4\* | +π/2      | +π/2 | +π/2 | + π/2     | + π/4\*| NaN
 NaN    | NaN       | NaN  | NaN  | NaN       | NaN   | NaN
<aside class="notification">
F Means finite floating-point value. \* Table 8-10 in the Intel® 64 and
IA-32 Architectures Software Developer's Manual, Volume 1, specifies that the
ratios 0/0 and ∞/∞generate the floating-point invalid arithmetic-operation exception
and, if this exception is masked, the floating-point QNaN indefinite value is
returned. With the FPATAN instruction, the 0/0 or ∞/∞ value is actually not
calculated using division. Instead, the arctangent of the two variables is derived
from a standard mathematical formulation that is generalized to allow complex
numbers as arguments. In this complex variable formulation, arctangent(0,0)
etc. has well defined values. These values are needed to develop a library to
compute transcendental functions with complex arguments, based on the FPU functions
that only allow floating-point values as arguments.
</aside>

There is no restriction on the range of source operands that FPATAN can accept.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.


### IA-32 Architecture Compatibility
The source operands for this instruction are restricted for the 80287 math coprocessor
### to the following range

0 ≤ |ST(1)| < |ST(0)| < +∞



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
 #IS| Stack underflow occurred.                     
 #IA| Source operand is an SNaN value or unsupported
    | format.                                       
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
