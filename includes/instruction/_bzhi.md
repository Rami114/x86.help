## BZHI  -  Zero High Bits Starting with Specified Bit Position

> Operation

``` slim
N <- SRC2[7:0]
DEST <- SRC1
IF (N < OperandSize)
  DEST[OperandSize-1:N] <- 0
FI
IF (N > OperandSize - 1)
  CF <- 1
ELSE
  CF <- 0
FI```

### Flags Affected
ZF, CF and SF flags are updated based on the result. OF flag is cleared. AF
and PF flags are undefined.


> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 BZHI:| unsigned __int32 _bzhi_u32(unsigned  
      | __int32 src, unsigned __int32 index);
 BZHI:| unsigned __int64 _bzhi_u64(unsigned  
      | __int64 src, unsigned __int32 index);

```

 Opcode/Instruction                  | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 VEX.NDS1.LZ.0F38.W0 F5 /r BZHI r32a,| RMV  | V/V            | BMI2              | Zero bits in r/m32 starting with the   
 r/m32, r32b                         |      |                |                   | position in r32b, write result to r32a.
 VEX.NDS1.LZ.0F38.W1 F5 /r BZHI r64a,| RMV  | V/N.E.         | BMI2              | Zero bits in r/m64 starting with the   
 r/m64, r64b                         |      |                |                   | position in r64b, write result to r64a.
<aside class="notification">
1. ModRM:r/m is used to encode the first source operand (second operand)
and VEX.vvvv encodes the second source operand (third operand).
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3   | Operand 4
 ---  | --- | --- | --- | ---
 RMV  | ModRM:reg (w)| ModRM:r/m (r)| VEX.vvvv (r)| NA       

### Description
BZHI copies the bits of the first source operand (the second operand) into the
destination operand (the first operand) and clears the higher bits in the destination
according to the INDEX value specified by the second source operand (the third
operand). The INDEX is specified by bits 7:0 of the second source operand. The
INDEX value is saturated at the value of OperandSize -1. CF is set, if the number
contained in the 8 low bits of the third operand is greater than OperandSize
-1. This instruction is not supported in real mode and virtual-8086 mode. The
operand size is always 32 bits if not in 64-bit mode. In 64-bit mode operand
size 64 requires VEX.W1. VEX.W1 is ignored in non-64-bit modes. An attempt to
execute this instruction with VEX.L not equal to 0 will cause **``#UD.``**



### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Section 2.5.1, “Exception Conditions for VEX-Encoded GPR Instructions”,
Table 2-29; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.W = 1.
