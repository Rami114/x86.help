## PSUBQ - Subtract Packed Quadword Integers

> Operation
``` slim

PSUBQ (with 64-Bit operands)
  DEST[63:0] <- DEST[63:0] − SRC[63:0];
PSUBQ (with 128-Bit operands)
  DEST[63:0] <- DEST[63:0] − SRC[63:0];
  DEST[127:64] <- DEST[127:64] − SRC[127:64];
VPSUBQ (VEX.128 encoded version)
DEST[63:0] <- SRC1[63:0]-SRC2[63:0]
DEST[127:64] <- SRC1[127:64]-SRC2[127:64]
DEST[VLMAX-1:128] <- 0
VPSUBQ (VEX.256 encoded version)
DEST[63:0] <- SRC1[63:0]-SRC2[63:0]
DEST[127:64] <- SRC1[127:64]-SRC2[127:64]
DEST[191:128] <- SRC1[191:128]-SRC2[191:128]
DEST[255:192] <- SRC1[255:192]-SRC2[255:192]

```

 Opcode/Instruction                      | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                          
 ---  | --- | --- | --- | ---
 0F FB /r1 PSUBQ mm1, mm2/m64            | RM   | V/V                   | SSE2              | Subtract quadword integer in mm1 from
                                         |      |                       |                   | mm2 /m64.                            
 66 0F FB /r PSUBQ xmm1, xmm2/m128       | RM   | V/V                   | SSE2              | Subtract packed quadword integers in 
                                         |      |                       |                   | xmm1 from xmm2 /m128.                
 VEX.NDS.128.66.0F.WIG FB/r VPSUBQ xmm1, | RVM  | V/V                   | AVX               | Subtract packed quadword integers in 
 xmm2, xmm3/m128                         |      |                       |                   | xmm3/m128 from xmm2.                 
 VEX.NDS.256.66.0F.WIG FB /r VPSUBQ ymm1,| RVM  | V/V                   | AVX2              | Subtract packed quadword integers in 
 ymm2, ymm3/m256                         |      |                       |                   | ymm3/m256 from ymm2.                 
<aside class="notification">
1. See note in Section 2.4, “Instruction Exception Specification” in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 2A
and Section 22.25.3, “Exception Conditions of Legacy SIMD Instructions Operating
on MMX Registers” in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3A.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r, w)| ModRM:r/m (r)| NA           | NA       
 RVM  | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| NA       

### Description
Subtracts the second operand (source operand) from the first operand (destination
operand) and stores the result in the destination operand. When packed quadword
operands are used, a SIMD subtract is performed. When a quadword result is too
large to be represented in 64 bits (overflow), the result is wrapped around
and the low 64 bits are written to the destination element (that is, the carry
is ignored).

<aside class="notification">
Note that the (V)PSUBQ instruction can operate on either unsigned or signed
(two's complement notation) integers; however, it does not set bits in the EFLAGS
register to indicate overflow and/or a carry. To prevent undetected overflow
conditions, software must control the ranges of the values upon which it operates.
</aside>

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).

Legacy SSE version: The source operand can be a quadword integer stored in an
### MMX technology register or a 64bit memory location. 128-bit Legacy SSE version
The second source operand is an XMM register or a 128-bit memory location. The
first source operand and destination operands are XMM registers. Bits (VLMAX-1:128)
of the corresponding YMM destination register remain unchanged. VEX.128 encoded
version: The second source operand is an XMM register or a 128-bit memory location.
The first source operand and destination operands are XMM registers. Bits (VLMAX-1:128)
of the destination YMM register are zeroed. VEX.256 encoded version: The second
source operand is an YMM register or a 256-bit memory location. The first source
operand and destination operands are YMM registers.

<aside class="notification">
VEX.L must be 0, otherwise instructions will #UD.
</aside>



### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 PSUBQ:   | __m64 _mm_sub_si64(__m64 m1, __m64 m2)   
 (V)PSUBQ:| __m128i _mm_sub_epi64(__m128i m1, __m128i
          | m2)                                      
 VPSUBQ:  | __m256i _mm256_sub_epi64(__m256i m1,     
          | __m256i m2)                              

### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1.
