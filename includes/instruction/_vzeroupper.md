## VZEROUPPER - Zero Upper Bits of YMM Registers

> Operation

``` slim
VZEROUPPER
IF (64-bit mode)
  YMM0[VLMAX-1:128] <- 0
  YMM1[VLMAX-1:128] <- 0
  YMM2[VLMAX-1:128] <- 0
  YMM3[VLMAX-1:128] <- 0
  YMM4[VLMAX-1:128] <- 0
  YMM5[VLMAX-1:128] <- 0
  YMM6[VLMAX-1:128] <- 0
  YMM7[VLMAX-1:128] <- 0
  YMM8[VLMAX-1:128] <- 0
  YMM9[VLMAX-1:128] <- 0
  YMM10[VLMAX-1:128] <- 0
  YMM11[VLMAX-1:128] <- 0
  YMM12[VLMAX-1:128] <- 0
  YMM13[VLMAX-1:128] <- 0
  YMM14[VLMAX-1:128] <- 0
  YMM15[VLMAX-1:128] <- 0
ELSE
  YMM0[VLMAX-1:128] <- 0
  YMM1[VLMAX-1:128] <- 0
  YMM2[VLMAX-1:128] <- 0
  YMM3[VLMAX-1:128] <- 0
  YMM4[VLMAX-1:128] <- 0
  YMM5[VLMAX-1:128] <- 0
  YMM6[VLMAX-1:128] <- 0
  YMM7[VLMAX-1:128] <- 0
  YMM8-15: unmodified
FI

```

 Opcode/Instruction          | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 VEX.128.0F.WIG 77 VZEROUPPER| NP   | V/V                   | AVX               | Zero upper 128 bits of all YMM registers.

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
The instruction zeros the bits in position 128 and higher of all YMM registers.
The lower 128-bits of the registers (the corresponding XMM registers) are unmodified.

This instruction is recommended when transitioning between AVX and legacy SSE
code - it will eliminate performance penalties caused by false dependencies.
<aside class="notification">
VEX.vvvv is reserved and must be 1111b otherwise instructions will #UD.
In Compatibility and legacy 32-bit mode only the lower 8 registers are modified.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 VZEROUPPER:| _mm256_zeroupper()

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 8.
