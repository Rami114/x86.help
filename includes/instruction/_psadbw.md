## PSADBW - Compute Sum of Absolute Differences

> Operation

``` slim
PSADBW (when using 64-bit operands)
  TEMP0 <- ABS(DEST[7:0] − SRC[7:0]);
  (\* Repeat operation for bytes 2 through 6 \*)
  TEMP7 <- ABS(DEST[63:56] − SRC[63:56]);
  DEST[15:0] <- SUM(TEMP0:TEMP7);
  DEST[63:16] <- 000000000000H;
PSADBW (when using 128-bit operands)
  TEMP0 <- ABS(DEST[7:0] − SRC[7:0]);
  (\* Repeat operation for bytes 2 through 14 \*)
  TEMP15 <- ABS(DEST[127:120] − SRC[127:120]);
  DEST[15:0] <- SUM(TEMP0:TEMP7);
  DEST[63:16] <- 000000000000H;
  DEST[79:64] <- SUM(TEMP8:TEMP15);
  DEST[127:80] <- 000000000000H;
  DEST[VLMAX-1:128] (Unmodified)
VPSADBW (VEX.128 encoded version)
TEMP0 <- ABS(SRC1[7:0] - SRC2[7:0])
(\* Repeat operation for bytes 2 through 14 \*)
TEMP15 <- ABS(SRC1[127:120] - SRC2[127:120])
DEST[15:0] <-SUM(TEMP0:TEMP7)
DEST[63:16] <- 000000000000H
DEST[79:64] <- SUM(TEMP8:TEMP15)
DEST[127:80] <- 00000000000
DEST[VLMAX-1:128] <- 0
VPSADBW (VEX.256 encoded version)
TEMP0 <- ABS(SRC1[7:0] - SRC2[7:0])
(\* Repeat operation for bytes 2 through 30\*)
TEMP31 <- ABS(SRC1[255:248] - SRC2[255:248])
DEST[15:0] <-SUM(TEMP0:TEMP7)
DEST[63:16] <- 000000000000H
DEST[79:64] <- SUM(TEMP8:TEMP15)
DEST[127:80] <- 00000000000H
DEST[143:128] <-SUM(TEMP16:TEMP23)
DEST[191:144] <- 000000000000H
DEST[207:192] <- SUM(TEMP24:TEMP31)
DEST[223:208] <- 00000000000H

```

 Opcode/Instruction                 | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 0F F6 /r1 PSADBW mm1, mm2/m64      | RM   | V/V                   | SSE               | Computes the absolute differences of      
                                    |      |                       |                   | the packed unsigned byte integers from    
                                    |      |                       |                   | mm2 /m64 and mm1; differences are then    
                                    |      |                       |                   | summed to produce an unsigned word integer
                                    |      |                       |                   | result.                                   
 66 0F F6 /r PSADBW xmm1, xmm2/m128 | RM   | V/V                   | SSE2              | Computes the absolute differences of      
                                    |      |                       |                   | the packed unsigned byte integers from    
                                    |      |                       |                   | xmm2 /m128 and xmm1; the 8 low differences
                                    |      |                       |                   | and 8 high differences are then summed    
                                    |      |                       |                   | separately to produce two unsigned word   
                                    |      |                       |                   | integer results.                          
 VEX.NDS.128.66.0F.WIG F6 /r VPSADBW| RVM  | V/V                   | AVX               | Computes the absolute differences of      
 xmm1, xmm2, xmm3/m128              |      |                       |                   | the packed unsigned byte integers from    
                                    |      |                       |                   | xmm3 /m128 and xmm2; the 8 low differences
                                    |      |                       |                   | and 8 high differences are then summed    
                                    |      |                       |                   | separately to produce two unsigned word   
                                    |      |                       |                   | integer results.                          
 VEX.NDS.256.66.0F.WIG F6 /r VPSADBW| RVM  | V/V                   | AVX2              | Computes the absolute differences of      
 ymm1, ymm2, ymm3/m256              |      |                       |                   | the packed unsigned byte integers from    
                                    |      |                       |                   | ymm3 /m256 and ymm2; then each consecutive
                                    |      |                       |                   | 8 differences are summed separately       
                                    |      |                       |                   | to produce four unsigned word integer     
                                    |      |                       |                   | results.                                  
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
Computes the absolute value of the difference of 8 unsigned byte integers from
the source operand (second operand) and from the destination operand (first
operand). These 8 differences are then summed to produce an unsigned word integer
result that is stored in the destination operand. Figure 4-10 shows the operation
of the PSADBW instruction when using 64-bit operands.

When operating on 64-bit operands, the word integer result is stored in the
low word of the destination operand, and the remaining bytes in the destination
operand are cleared to all 0s.

When operating on 128-bit operands, two packed results are computed. Here, the
8 low-order bytes of the source and destination operands are operated on to
produce a word result that is stored in the low word of the destination operand,
and the 8 high-order bytes are operated on to produce a word result that is
stored in bits 64 through 79 of the destination operand. The remaining bytes
of the destination operand are cleared.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). Legacy SSE version: The source
operand can be an MMX technology register or a 64-bit memory location. The destination
operand is an MMX technology register.

128-bit Legacy SSE version: The first source operand and destination register
are XMM registers. The second source operand is an XMM register or a 128-bit
memory location. Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged.

VEX.128 encoded version: The first source operand and destination register are
XMM registers. The second source operand is an XMM register or a 128-bit memory
location. Bits (VLMAX-1:128) of the destination YMM register are zeroed. VEX.256
encoded version: The first source operand and destination register are YMM registers.
The second source operand is an YMM register or a 256-bit memory location.

<aside class="notification">
VEX.L must be 0, otherwise the instruction will #UD.
</aside>

   | |  
---- | -----
 SRC Figure 4-10.| X7 Y7 ABS(X7:Y7) 00H| X6 Y6 ABS(X6:Y6) 00H PSADBW Instruction| X5 Y5 ABS(X5:Y5) 00H| X4 Y4 ABS(X4:Y4) 00H| X3 Y3 ABS(X3:Y3) 00H| X2 Y2 ABS(X2:Y2) 00H| X1 Y1 ABS(X1:Y1) SUM(TEMP7...TEMP0)| X0 Y0 ABS(X0:Y0) DEST
                 |                     | Operation Using 64-bit Operands        |                     |                     |                     |                     |                                    |                      


### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PSADBW:   | __m64 _mm_sad_pu8(__m64 a,__m64 b)     
 (V)PSADBW:| __m128i _mm_sad_epu8(__m128i a, __m128i
           | b)                                     
 VPSADBW:  | __m256i _mm256_sad_epu8( __m256i a,    
           | __m256i b)                             

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1.
