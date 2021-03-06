## PMOVMSKB - Move Byte Mask

> Operation

``` slim
PMOVMSKB (with 64-bit source operand and r32)
  r32[0] <- SRC[7];
  r32[1] <- SRC[15];
  (\* Repeat operation for bytes 2 through 6 \*)
  r32[7] <- SRC[63];
  r32[31:8] <- ZERO_FILL;
(V)PMOVMSKB (with 128-bit source operand and r32)
  r32[0] <- SRC[7];
  r32[1] <- SRC[15];
  (\* Repeat operation for bytes 2 through 14 \*)
  r32[15] <- SRC[127];
  r32[31:16] <- ZERO_FILL;
VPMOVMSKB (with 256-bit source operand and r32)
r32[0] <- SRC[7];
r32[1] <- SRC[15];
(\* Repeat operation for bytes 3rd through 31\*)
r32[31] <- SRC[255];
PMOVMSKB (with 64-bit source operand and r64)
  r64[0] <- SRC[7];
  r64[1] <- SRC[15];
  (\* Repeat operation for bytes 2 through 6 \*)
  r64[7] <- SRC[63];
  r64[63:8] <- ZERO_FILL;
(V)PMOVMSKB (with 128-bit source operand and r64)
  r64[0] <- SRC[7];
  r64[1] <- SRC[15];
  (\* Repeat operation for bytes 2 through 14 \*)
  r64[15] <- SRC[127];
  r64[63:16] <- ZERO_FILL;
VPMOVMSKB (with 256-bit source operand and r64)
r64[0] <- SRC[7];
r64[1] <- SRC[15];
(\* Repeat operation for bytes 2 through 31\*)
r64[31] <- SRC[255];
r64[63:32] <- ZERO_FILL;

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PMOVMSKB:   | int _mm_movemask_pi8(__m64 a)        
 (V)PMOVMSKB:| int _mm_movemask_epi8 ( __m128i a)   
 VPMOVMSKB:  | int _mm256_movemask_epi8 ( __m256i a)

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 0F D7 /r1 PMOVMSKB reg, mm            | RM   | V/V                   | SSE               | Move a byte mask of mm to reg. The upper
                                       |      |                       |                   | bits of r32 or r64 are zeroed           
 66 0F D7 /r PMOVMSKB reg, xmm         | RM   | V/V                   | SSE2              | Move a byte mask of xmm to reg. The     
                                       |      |                       |                   | upper bits of r32 or r64 are zeroed     
 VEX.128.66.0F.WIG D7 /r VPMOVMSKB reg,| RM   | V/V                   | AVX               | Move a byte mask of xmm1 to reg. The    
 xmm1                                  |      |                       |                   | upper bits of r32 or r64 are filled     
                                       |      |                       |                   | with zeros.                             
 VEX.256.66.0F.WIG D7 /r VPMOVMSKB reg,| RM   | V/V                   | AVX2              | Move a 32-bit mask of ymm1 to reg. The  
 ymm1                                  |      |                       |                   | upper bits of r64 are filled with zeros.
<aside class="notification">
1. See note in Section 2.4, “Instruction Exception Specification” in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 2A
and Section 22.25.3, “Exception Conditions of Legacy SIMD Instructions Operating
on MMX Registers” in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3A.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Creates a mask made up of the most significant bit of each byte of the source
operand (second operand) and stores the result in the low byte or word of the
destination operand (first operand). The byte mask is 8 bits for 64-bit source
operand, 16 bits for 128-bit source operand and 32 bits for 256-bit source operand.
The destination operand is a general-purpose register.

In 64-bit mode, the instruction can access additional registers (XMM8-XMM15,
R8-R15) when used with a REX.R prefix. The default operand size is 64-bit in
64-bit mode. Legacy SSE version: The source operand is an MMX technology register.
128-bit Legacy SSE version: The source operand is an XMM register.

VEX.128 encoded version: The source operand is an XMM register. VEX.256 encoded
version: The source operand is a YMM register. Note: VEX.vvvv is reserved and
must be 1111b, VEX.L must be 0, otherwise the instruction will **``#UD.``**



### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 7; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1. If VEX.vvvv != 1111B.
