## STMXCSR - Store MXCSR Register State

> Operation

``` slim
m32 <- MXCSR;

```

 Opcode\*/Instruction             | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                        
 ---  | --- | --- | --- | ---
 0F AE /3 STMXCSR m32            | M    | V/V                   | SSE               | Store contents of MXCSR register to
                                 |      |                       |                   | m32.                               
 VEX.LZ.0F.WIG AE /3 VSTMXCSR m32| M    | V/V                   | AVX               | Store contents of MXCSR register to
                                 |      |                       |                   | m32.                               

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (w)| NA       | NA       | NA       

### Description
Stores the contents of the MXCSR control and status register to the destination
operand. The destination operand is a 32-bit memory location. The reserved bits
in the MXCSR register are stored as 0s.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.
VEX.L must be 0, otherwise instructions will #UD.

<aside class="notification">
In VEX-encoded versions, VEX.vvvv is reserved and must be 1111b, otherwise
instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
_mm_getcsr(void)


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L= 1, If VEX.vvvv != 1111B.
