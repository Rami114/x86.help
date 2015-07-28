## PEXT  -  Parallel Bits Extract

> Operation
``` slim

TEMP <- SRC1;
MASK <- SRC2;
DEST <- 0 ;
m<- 0, k<- 0;
DO WHILE m< OperandSize
     IF MASK[ m] = 1 THEN
       DEST[ k] <- TEMP[ m];
       k <- k+ 1;
     FI
     m <- m+ 1;
OD

```

 Opcode/Instruction                    | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 VEX.NDS.LZ.F3.0F38.W0 F5 /r PEXT r32a,| RVM  | V/V            | BMI2              | Parallel extract of bits from r32b using
 r32b, r/m32                           |      |                |                   | mask in r/m32, result is written to     
                                       |      |                |                   | r32a.                                   
 VEX.NDS.LZ.F3.0F38.W1 F5 /r PEXT r64a,| RVM  | V/N.E.         | BMI2              | Parallel extract of bits from r64b using
 r64b, r/m64                           |      |                |                   | mask in r/m64, result is written to     
                                       |      |                |                   | r64a.                                   

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2   | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVM  | ModRM:reg (w)| VEX.vvvv (r)| ModRM:r/m (r)| NA       

### Description
PEXT uses a mask in the second source operand (the third operand) to transfer
either contiguous or non-contiguous bits in the first source operand (the second
operand) to contiguous low order bit positions in the destination (the first
operand). For each bit set in the MASK, PEXT extracts the corresponding bits
from the first source operand and writes them into contiguous lower bits of
destination operand. The remaining upper bits of destination are zeroed.

SRC1

   | |  
---- | -----
 S<sub>31</sub>| S<sub>30</sub>| S<sub>29</sub> S<sub>28</sub>| S<sub>27</sub>| S<sub>7</sub>| S<sub>6</sub>| S<sub>5</sub>| S<sub>4</sub>| S<sub>3</sub>| S<sub>2</sub>| S<sub>1</sub>| S<sub>0</sub>
SRC2

   | |  
---- | -----
 0| 0| 0| 1| 0            | 1             | 0| 1| 0| 0             | 1            | 0            | 0 (mask)           
 0| 0| 0| 0| 0 Figure 4-5.| 0 PEXT Example| 0| 0| 0| S<sub>28</sub>| S<sub>7</sub>| S<sub>5</sub>| S<sub>2</sub> bit 0
This instruction is not supported in real mode and virtual-8086 mode. The operand
size is always 32 bits if not in 64-bit mode. In 64-bit mode operand size 64
requires VEX.W1. VEX.W1 is ignored in non-64-bit modes. An attempt to execute
this instruction with VEX.L not equal to 0 will cause #UD.



### Flags Affected
None.


### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PEXT:| unsigned __int32 _pext_u32(unsigned 
      | __int32 src, unsigned __int32 mask);
 PEXT:| unsigned __int64 _pext_u64(unsigned 
      | __int64 src, unsigned __int32 mask);

### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Section 2.5.1, “Exception Conditions for VEX-Encoded GPR Instructions”,
Table 2-29; additionally

   | |  
---- | -----
 #UD| If VEX.W = 1.
