## CMPSS - Compare Scalar Single-Precision Floating-Point Values

> Operation

``` slim
CASE (COMPARISON PREDICATE) OF
  0: OP3 <- EQ_OQ; OP5 <- EQ_OQ;
  1: OP3 <- LT_OS; OP5 <- LT_OS;
  2: OP3 <- LE_OS; OP5 <- LE_OS;
  3: OP3 <- UNORD_Q; OP5 <- UNORD_Q;
  4: OP3 <- NEQ_UQ; OP5 <- NEQ_UQ;
  5: OP3 <- NLT_US; OP5 <- NLT_US;
  6: OP3 <- NLE_US; OP5 <- NLE_US;
  7: OP3 <- ORD_Q; OP5 <- ORD_Q;
  8: OP5 <- EQ_UQ;
  9: OP5 <- NGE_US;
  10: OP5 <- NGT_US;
  11: OP5 <- FALSE_OQ;
  12: OP5 <- NEQ_OQ;
  13: OP5 <- GE_OS;
  14: OP5 <- GT_OS;
  15: OP5 <- TRUE_UQ;
  16: OP5 <- EQ_OS;
  17: OP5 <- LT_OQ;
  18: OP5 <- LE_OQ;
  19: OP5 <- UNORD_S;
  20: OP5 <- NEQ_US;
  21: OP5 <- NLT_UQ;
  22: OP5 <- NLE_UQ;
  23: OP5 <- ORD_S;
  24: OP5 <- EQ_US;
  25: OP5 <- NGE_UQ;
  26: OP5 <- NGT_UQ;
  27: OP5 <- FALSE_OS;
  28: OP5 <- NEQ_OS;
  29: OP5 <- GE_OQ;
  30: OP5 <- GT_OQ;
  31: OP5 <- TRUE_US;
  DEFAULT: Reserved
ESAC;
CMPSS (128-bit Legacy SSE version)
CMP0 <- DEST[31:0] OP3 SRC[31:0];
IF CMP0 = TRUE
THEN DEST[31:0] <- FFFFFFFFH;
ELSE DEST[31:0] <- 00000000H; FI;
DEST[VLMAX-1:32] (Unmodified)
VCMPSS (VEX.128 encoded version)
CMP0 <- SRC1[31:0] OP5 SRC2[31:0];
IF CMP0 = TRUE
THEN DEST[31:0] <- FFFFFFFFH;
ELSE DEST[31:0] <- 00000000H; FI;
DEST[127:32] <- SRC1[127:32]
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 CMPSS for equality:                                   | __m128 _mm_cmpeq_ss(__m128 a, __m128   
                                                       | b)                                     
 CMPSS for less-than:                                  | __m128 _mm_cmplt_ss(__m128 a, __m128   
                                                       | b)                                     
 CMPSS for less-than-or-equal:                         | __m128 _mm_cmple_ss(__m128 a, __m128   
                                                       | b)                                     
 CMPSS for greater-than:                               | __m128 _mm_cmpgt_ss(__m128 a, __m128   
                                                       | b)                                     
 CMPSS for greater-than-or-equal:                      | __m128 _mm_cmpge_ss(__m128 a, __m128   
                                                       | b)                                     
 CMPSS for inequality:                                 | __m128 _mm_cmpneq_ss(__m128 a, __m128  
                                                       | b)                                     
 CMPSS for not-less-than:                              | __m128 _mm_cmpnlt_ss(__m128 a, __m128  
                                                       | b)                                     
 CMPSS for not-greater-than: CMPSS for                 | __m128 _mm_cmpngt_ss(__m128 a, __m128  
 not-greater-than-or-equal: __m128 _mm_cmpnge_ss(__m128| b)                                     
 a, __m128 b)                                          |                                        
 CMPSS for ordered:                                    | __m128 _mm_cmpord_ss(__m128 a, __m128  
                                                       | b)                                     
 CMPSS for unordered:                                  | __m128 _mm_cmpunord_ss(__m128 a, __m128
                                                       | b)                                     
 CMPSS for not-less-than-or-equal:                     | __m128 _mm_cmpnle_ss(__m128 a, __m128  
                                                       | b)                                     
 VCMPSS:                                               | __m128 _mm_cmp_ss(__m128 a, __m128 b,  
                                                       | const int imm)                         

```

 Opcode/Instruction                   | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 F3 0F C2 /r ib CMPSS xmm1, xmm2/m32, | RMI  | V/V           | SSE               | Compare low single-precision floating-point
 imm8                                 |      |               |                   | value in xmm2/m32 and xmm1 using imm8      
                                      |      |               |                   | as comparison predicate.                   
 VEX.NDS.LIG.F3.0F.WIG C2 /r ib VCMPSS| RVMI | V/V           | AVX               | Compare low single precision floating-point
 xmm1, xmm2, xmm3/m32, imm8           |      |               |                   | value in xmm3/m32 and xmm2 using bits      
                                      |      |               |                   | 4:0 of imm8 as comparison predicate.       

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
Compares the low single-precision floating-point values in the source operand
(second operand) and the destination operand (first operand) and returns the
results of the comparison to the destination operand. The comparison predicate
operand (third operand) specifies the type of comparison performed. The comparison
result is a doubleword mask of all 1s (comparison true) or all 0s (comparison
false). The sign of zero is ignored for comparisons, so that -0.0 is equal to
+0.0. 128-bit Legacy SSE version: The first source and destination operand (first
operand) is an XMM register. The second source operand (second operand) can
be an XMM register or 64-bit memory location. The comparison predicate operand
is an 8-bit immediate, bits 2:0 of the immediate define the type of comparison
to be performed (see Table 3-7). Bits 7:3 of the immediate is reserved. Bits
(VLMAX-1:32) of the corresponding YMM destination register remain unchanged.

The unordered relationship is true when at least one of the two source operands
being compared is a NaN; the ordered relationship is true when neither source
operand is a NaN

A subsequent computational instruction that uses the mask result in the destination
operand as an input operand will not generate a fault, since a mask of all 0s
corresponds to a floating-point value of +0.0 and a mask of all 1s corresponds
to a QNaN.

<aside class="notification">
Note that processors with “CPUID.1H:ECX.AVX =0” do not implement the “greater-than”,
“greater-than-or-equal”, “not-greater than”, and “not-greater-than-or-equal
relations” predicates. These comparisons can be made either by using the inverse
relationship (that is, use the “not-less-than-or-equal” to make a “greater-than”
comparison) or by using software emulation. When using software emulation, the
program must swap the operands (copying registers when necessary to protect
the data that will now be in the destination operand), and then perform the
compare using a different predicate. The predicate to be used for these emulations
is listed in Table 3-7 under the heading Emulation.
</aside>

Compilers and assemblers may implement the following two-operand pseudo-ops
in addition to the three-operand CMPSS instruction, for processors with “CPUID.1H:ECX.AVX
=0”. See Table 3-15. Compiler should treat reserved Imm8 values as illegal syntax.


### Table 3-15. Pseudo-Ops and CMPSS
   | |  
---- | -----
 Pseudo-Op            | CMPSS Implementation
 CMPEQSS xmm1, xmm2   | CMPSS xmm1, xmm2, 0 
 CMPLTSS xmm1, xmm2   | CMPSS xmm1, xmm2, 1 
 CMPLESS xmm1, xmm2   | CMPSS xmm1, xmm2, 2 
 CMPUNORDSS xmm1, xmm2| CMPSS xmm1, xmm2, 3 
 CMPNEQSS xmm1, xmm2  | CMPSS xmm1, xmm2, 4 
 CMPNLTSS xmm1, xmm2  | CMPSS xmm1, xmm2, 5 
 CMPNLESS xmm1, xmm2  | CMPSS xmm1, xmm2, 6 
 CMPORDSS xmm1, xmm2  | CMPSS xmm1, xmm2, 7 
The greater-than relations not implemented in the processor require more than
one instruction to emulate in software and therefore should not be implemented
as pseudo-ops. (For these, the programmer should reverse the operands of the
corresponding less than relations and use move instructions to ensure that the
mask is moved to the correct destination register and that the source operand
is left intact.)

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).

### Enhanced Comparison Predicate for VEX-Encoded VCMPSD VEX.128 encoded version
The first source operand (second operand) is an XMM register. The second source
operand (third operand) can be an XMM register or a 32-bit memory location.
Bits (VLMAX-1:128) of the destination YMM register are zeroed. The comparison
### predicate operand is an 8-bit immediate

 - For instructions encoded using the VEX prefix, bits 4:0 define the type of comparison
to be performed (see Table 3-9). Bits 5 through 7 of the immediate are reserved.

Processors with “CPUID.1H:ECX.AVX =1” implement the full complement of 32 predicates
shown in Table 3-9, software emulation is no longer needed. Compilers and assemblers
may implement the following three-operand pseudo-ops in addition to the four-operand
VCMPSS instruction. See Table 3-16, where the notations of reg1 reg2, and reg3
represent either XMM registers or YMM registers. Compiler should treat reserved
Imm8 values as illegal syntax. Alternately, intrinsics can map the pseudo-ops
to pre-defined constants to support a simpler intrinsic interface.

   | |  
---- | -----
 : Pseudo-Op VCMPEQSS reg1, reg2, reg3          | Table 3-16. Table 3-16.| Pseudo-Op and VCMPSS Implementation      
 VCMPLTSS reg1, reg2, reg3 VCMPLESS reg1,       |                        | CMPSS Implementation VCMPSS reg1, reg2,  
 reg2, reg3 VCMPUNORDSS reg1, reg2, reg3        |                        | reg3, 0 VCMPSS reg1, reg2, reg3, 1 VCMPSS
 VCMPNEQSS reg1, reg2, reg3 VCMPNLTSS           |                        | reg1, reg2, reg3, 2 VCMPSS reg1, reg2,   
 reg1, reg2, reg3 VCMPNLESS reg1, reg2,         |                        | reg3, 3 VCMPSS reg1, reg2, reg3, 4 VCMPSS
 reg3 VCMPORDSS reg1, reg2, reg3 VCMPEQ_UQSS    |                        | reg1, reg2, reg3, 5 VCMPSS reg1, reg2,   
 reg1, reg2, reg3 VCMPNGESS reg1, reg2,         |                        | reg3, 6 VCMPSS reg1, reg2, reg3, 7 VCMPSS
 reg3 VCMPNGTSS reg1, reg2, reg3 VCMPFALSESS    |                        | reg1, reg2, reg3, 8 VCMPSS reg1, reg2,   
 reg1, reg2, reg3 VCMPNEQ_OQSS reg1,            |                        | reg3, 9 VCMPSS reg1, reg2, reg3, 0AH     
 reg2, reg3 VCMPGESS reg1, reg2, reg3           |                        | VCMPSS reg1, reg2, reg3, 0BH VCMPSS      
 VCMPGTSS reg1, reg2, reg3 Pseudo-Op            |                        | reg1, reg2, reg3, 0CH VCMPSS reg1, reg2, 
 VCMPTRUESS reg1, reg2, reg3 VCMPEQ_OSSS        |                        | reg3, 0DH VCMPSS reg1, reg2, reg3, 0EH   
 reg1, reg2, reg3 VCMPLT_OQSS reg1, reg2,       |                        | Pseudo-Op and VCMPSS Implementation      
 reg3 VCMPLE_OQSS reg1, reg2, reg3 VCMPUNORD_SSS|                        | (Contd.) CMPSS Implementation VCMPSS     
 reg1, reg2, reg3 VCMPNEQ_USSS reg1,            |                        | reg1, reg2, reg3, 0FH VCMPSS reg1, reg2, 
 reg2, reg3 VCMPNLT_UQSS reg1, reg2,            |                        | reg3, 10H VCMPSS reg1, reg2, reg3, 11H   
 reg3 VCMPNLE_UQSS reg1, reg2, reg3 VCMPORD_SSS |                        | VCMPSS reg1, reg2, reg3, 12H VCMPSS      
 reg1, reg2, reg3 VCMPEQ_USSS reg1, reg2,       |                        | reg1, reg2, reg3, 13H VCMPSS reg1, reg2, 
 reg3 VCMPNGE_UQSS reg1, reg2, reg3 VCMPNGT_UQSS|                        | reg3, 14H VCMPSS reg1, reg2, reg3, 15H   
 reg1, reg2, reg3 VCMPFALSE_OSSS reg1,          |                        | VCMPSS reg1, reg2, reg3, 16H VCMPSS      
 reg2, reg3 VCMPNEQ_OSSS reg1, reg2,            |                        | reg1, reg2, reg3, 17H VCMPSS reg1, reg2, 
 reg3 VCMPGE_OQSS reg1, reg2, reg3 VCMPGT_OQSS  |                        | reg3, 18H VCMPSS reg1, reg2, reg3, 19H   
 reg1, reg2, reg3 VCMPTRUE_USSS reg1,           |                        | VCMPSS reg1, reg2, reg3, 1AH VCMPSS      
 reg2, reg3                                     |                        | reg1, reg2, reg3, 1BH VCMPSS reg1, reg2, 
                                                |                        | reg3, 1CH VCMPSS reg1, reg2, reg3, 1DH   
                                                |                        | VCMPSS reg1, reg2, reg3, 1EH VCMPSS      
                                                |                        | reg1, reg2, reg3, 1FH                    


### SIMD Floating-Point Exceptions
Invalid if SNaN operand, Invalid if QNaN and predicate as listed in above table,
Denormal.


### Other Exceptions
See Exceptions Type 3.
