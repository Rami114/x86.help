## PSRLDQ - Shift Double Quadword Right Logical

> Operation
``` slim

PSRLDQ(128-bit Legacy SSE version)
TEMP <- COUNT
IF (TEMP > 15) THEN TEMP <- 16; FI
DEST <- DEST >> (TEMP \* 8)
DEST[VLMAX-1:128] (Unmodified)
VPSRLDQ (VEX.128 encoded version)
TEMP <- COUNT
IF (TEMP > 15) THEN TEMP <- 16; FI
DEST <- SRC >> (TEMP \* 8)
DEST[VLMAX-1:128] <- 0
VPSRLDQ (VEX.256 encoded version)
TEMP <- COUNT
IF (TEMP > 15) THEN TEMP <- 16; FI
DEST[127:0] <- SRC[127:0] >> (TEMP \* 8)
DEST[255:128] <- SRC[255:128] >> (TEMP \* 8)

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 66 0F 73 /3 ib PSRLDQ xmm1, imm8      | MI   | V/V                   | SSE2              | Shift xmm1 right by imm8 while shifting
                                       |      |                       |                   | in 0s.                                 
 VEX.NDD.128.66.0F.WIG 73 /3 ib VPSRLDQ| VMI  | V/V                   | AVX               | Shift xmm2 right by imm8 bytes while   
 xmm1, xmm2, imm8                      |      |                       |                   | shifting in 0s.                        
 VEX.NDD.256.66.0F.WIG 73 /3 ib VPSRLDQ| VMI  | V/V                   | AVX2              | Shift ymm1 right by imm8 bytes while   
 ymm1, ymm2, imm8                      |      |                       |                   | shifting in 0s.                        

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 MI   | ModRM:r/m (r, w)| imm8         | NA       | NA       
 VMI  | VEX.vvvv (w)    | ModRM:r/m (r)| imm8     | NA       

### Description
Shifts the destination operand (first operand) to the right by the number of
bytes specified in the count operand (second operand). The empty high-order
bytes are cleared (set to all 0s). If the value specified by the count operand
is greater than 15, the destination operand is set to all 0s. The count operand
is an 8-bit immediate.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: The
source and destination operands are the same. Bits (VLMAX-1:128) of the corresponding
YMM destination register remain unchanged. VEX.128 encoded version: The source
and destination operands are XMM registers. Bits (VLMAX-1:128) of the destination
YMM register are zeroed. VEX.256 encoded version: The source operand is a YMM
register. The destination operand is a YMM register. The count operand applies
to both the low and high 128-bit lanes.

<aside class="notification">
VEX.vvvv encodes the destination register, and VEX.B + ModRM.r/m encodes
the source register. VEX.L must be 0, otherwise instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 (V)PSRLDQ:| __m128i _mm_srli_si128 ( __m128i a,
           | int imm)                           
 VPSRLDQ:  | __m256i _mm256_srli_si256 ( __m256i
           | a, const int imm)                  

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 7; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
