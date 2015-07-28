## ROUNDPD  -  Round Packed Double Precision Floating-Point Values

> Operation
``` slim

IF (imm[2] = ‘1)
  THEN
     DEST[63:0] <- ConvertDPFPToInteger_M(SRC[63:0]);
     DEST[127:64] <- ConvertDPFPToInteger_M(SRC[127:64]);
  ELSE
     DEST[63:0] <- ConvertDPFPToInteger_Imm(SRC[63:0]);
     DEST[127:64] <- ConvertDPFPToInteger_Imm(SRC[127:64]);
FI
ROUNDPD (128-bit Legacy SSE version)
DEST[63:0] <- RoundToInteger(SRC[63:0]], ROUND_CONTROL)
DEST[127:64] <- RoundToInteger(SRC[127:64]], ROUND_CONTROL)
DEST[VLMAX-1:128] (Unmodified)
VROUNDPD (VEX.128 encoded version)
DEST[63:0] <- RoundToInteger(SRC[63:0]], ROUND_CONTROL)
DEST[127:64] <- RoundToInteger(SRC[127:64]], ROUND_CONTROL)
DEST[VLMAX-1:128] <- 0
VROUNDPD (VEX.256 encoded version)
DEST[63:0] <- RoundToInteger(SRC[63:0], ROUND_CONTROL)
DEST[127:64] <- RoundToInteger(SRC[127:64]], ROUND_CONTROL)
DEST[191:128] <- RoundToInteger(SRC[191:128]], ROUND_CONTROL)
DEST[255:192] <- RoundToInteger(SRC[255:192] ], ROUND_CONTROL)

```

 Opcode\*/Instruction                       | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 66 0F 3A 09 /r ib ROUNDPD xmm1, xmm2/m128,| RMI  | V/V                   | SSE4_1            | Round packed double precision floating-point
 imm8                                      |      |                       |                   | values in xmm2/m128 and place the result    
                                           |      |                       |                   | in The rounding mode is determined by       
                                           |      |                       |                   | imm8.                                       
 VEX.128.66.0F3A.WIG 09 /r ib VROUNDPD     | RMI  | V/V                   | AVX               | Round packed double-precision floating-point
 xmm1, xmm2/m128, imm8                     |      |                       |                   | values in xmm2/m128 and place the result    
                                           |      |                       |                   | in xmm1. The rounding mode is determined    
                                           |      |                       |                   | by imm8.                                    
 VEX.256.66.0F3A.WIG 09 /r ib VROUNDPD     | RMI  | V/V                   | AVX               | Round packed double-precision floating-point
 ymm1, ymm2/m256, imm8                     |      |                       |                   | values in ymm2/m256 and place the result    
                                           |      |                       |                   | in ymm1. The rounding mode is determined    
                                           |      |                       |                   | by imm8.                                    

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8     | NA       

### Description
Round the 2 double-precision floating-point values in the source operand (second
operand) using the rounding mode specified in the immediate operand (third operand)
and place the results in the destination operand (first operand). The rounding
process rounds each input floating-point value to an integer value and returns
the integer result as a single-precision floating-point value.

The immediate operand specifies control fields for the rounding operation, three
bit fields are defined and shown in Figure 4-20. Bit 3 of the immediate byte
controls processor behavior for a precision exception, bit 2 selects the source
of rounding mode control. Bits 1:0 specify a non-sticky rounding-mode value
(Table 4-14 lists the encoded values for rounding-mode field).

The Precision Floating-Point Exception is signaled according to the immediate
operand. If any source operand is an SNaN then it will be converted to a QNaN.
If DAZ is set to ‘1 then denormals will be converted to zero before rounding.
128-bit Legacy SSE version: The second source can be an XMM register or 128-bit
memory location. The destination is not distinct from the first source XMM register
and the upper bits (VLMAX-1:128) of the corresponding YMM register destination
are unmodified. VEX.128 encoded version: the source operand second source operand
or a 128-bit memory location. The destination operand is an XMM register. The
upper bits (VLMAX-1:128) of the corresponding YMM register destination are zeroed.
VEX.256 encoded version: The source operand is a YMM register or a 256-bit memory
location. The destination operand is a YMM register. Note: In VEX-encoded versions,
VEX.vvvv is reserved and must be 1111b, otherwise instructions will #UD.

   | |  
---- | -----
 8| 3 2 1 0
Reserved

P  -  Precision Mask; 0: normal, 1: inexact RS  -  Rounding select; 1: MXCSR.RC,
0: Imm8.RC RC  -  Rounding mode

   | |  
---- | -----
 Figure 4-20.                                 | Bit Control Fields of Immediate Byte   
                                              | for ROUNDxx Instruction                
 Table 4-14.                                  | Rounding Modes and Encoding of Rounding
                                              | Control (RC) Field                     
 Description                                  | RC Field Setting                       
 Rounded result is the closest to the         | 00B nearest (even)                     
 infinitely precise result. If two values     |                                        
 are equally close, the result is the         |                                        
 even value (i.e., the integer value          |                                        
 with the least-significant bit of zero).     |                                        
 Rounded result is closest to but no          | 01B (toward −∞)                        
 greater than the infinitely precise          |                                        
 result.                                      |                                        
 Rounded result is closest to but no          | 10B (toward +∞)                        
 less than the infinitely precise result.     |                                        
 Rounded result is closest to but no          | 11B zero (Truncate)                    
 greater in absolute value than the infinitely|                                        
 precise result.                              |                                        


### Intel C/C++ Compiler Intrinsic Equivalent
__m128 _mm_round_pd(__m128d s1, int iRoundMode);

__m128 _mm_floor_pd(__m128d s1);

__m128 _mm_ceil_pd(__m128d s1)

__m256 _mm256_round_pd(__m256d s1, int iRoundMode);

__m256 _mm256_floor_pd(__m256d s1);

__m256 _mm256_ceil_pd(__m256d s1)


### SIMD Floating-Point Exceptions
Invalid (signaled only if SRC = SNaN) Precision (signaled only if imm[3] = ‘0;
if imm[3] = ‘1, then the Precision Mask in the MXSCSR is ignored and precision
exception is not signaled.) Note that Denormal is not signaled by ROUNDPD.


### Other Exceptions
See Exceptions Type 2; additionally

   | |  
---- | -----
 #UD| If VEX.vvvv != 1111B.
