## CMPPS - Compare Packed Single-Precision Floating-Point Values

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
EASC;
CMPPS (128-bit Legacy SSE version)
CMP0 <- SRC1[31:0] OP3 SRC2[31:0];
CMP1 <- SRC1[63:32] OP3 SRC2[63:32];
CMP2 <- SRC1[95:64] OP3 SRC2[95:64];
CMP3 <- SRC1[127:96] OP3 SRC2[127:96];
IF CMP0 = TRUE
  THEN DEST[31:0] <-FFFFFFFFH;
  ELSE DEST[31:0] <- 000000000H; FI;
IF CMP1 = TRUE
  THEN DEST[63:32] <- FFFFFFFFH;
  ELSE DEST[63:32] <- 000000000H; FI;
IF CMP2 = TRUE
  THEN DEST[95:64] <- FFFFFFFFH;
  ELSE DEST[95:64] <- 000000000H; FI;
IF CMP3 = TRUE
  THEN DEST[127:96] <- FFFFFFFFH;
  ELSE DEST[127:96] <-000000000H; FI;
DEST[VLMAX-1:128] (Unmodified)
VCMPPS (VEX.128 encoded version)
CMP0 <- SRC1[31:0] OP5 SRC2[31:0];
CMP1 <- SRC1[63:32] OP5 SRC2[63:32];
CMP2 <- SRC1[95:64] OP5 SRC2[95:64];
CMP3 <- SRC1[127:96] OP5 SRC2[127:96];
IF CMP0 = TRUE
  THEN DEST[31:0] <-FFFFFFFFH;
  ELSE DEST[31:0] <- 000000000H; FI;
IF CMP1 = TRUE
  THEN DEST[63:32] <- FFFFFFFFH;
  ELSE DEST[63:32] <- 000000000H; FI;
IF CMP2 = TRUE
  THEN DEST[95:64] <- FFFFFFFFH;
  ELSE DEST[95:64] <- 000000000H; FI;
IF CMP3 = TRUE
  THEN DEST[127:96] <- FFFFFFFFH;
  ELSE DEST[127:96] <-000000000H; FI;
DEST[VLMAX-1:128] <- 0
VCMPPS (VEX.256 encoded version)
CMP0 <- SRC1[31:0] OP5 SRC2[31:0];
CMP1 <- SRC1[63:32] OP5 SRC2[63:32];
CMP2 <- SRC1[95:64] OP5 SRC2[95:64];
CMP3 <- SRC1[127:96] OP5 SRC2[127:96];
CMP4 <- SRC1[159:128] OP5 SRC2[159:128];
CMP5 <- SRC1[191:160] OP5 SRC2[191:160];
CMP6 <- SRC1[223:192] OP5 SRC2[223:192];
CMP7 <- SRC1[255:224] OP5 SRC2[255:224];
IF CMP0 = TRUE
  THEN DEST[31:0] <-FFFFFFFFH;
  ELSE DEST[31:0] <- 000000000H; FI;
IF CMP1 = TRUE
  THEN DEST[63:32] <- FFFFFFFFH;
  ELSE DEST[63:32] <-000000000H; FI;
IF CMP2 = TRUE
  THEN DEST[95:64] <- FFFFFFFFH;
  ELSE DEST[95:64] <- 000000000H; FI;
IF CMP3 = TRUE
  THEN DEST[127:96] <- FFFFFFFFH;
  ELSE DEST[127:96] <- 000000000H; FI;
IF CMP4 = TRUE
  THEN DEST[159:128] <- FFFFFFFFH;
  ELSE DEST[159:128] <- 000000000H; FI;
IF CMP5 = TRUE
  THEN DEST[191:160] <- FFFFFFFFH;
  ELSE DEST[191:160] <- 000000000H; FI;
IF CMP6 = TRUE
  THEN DEST[223:192] <- FFFFFFFFH;
  ELSE DEST[223:192] <-000000000H; FI;
IF CMP7 = TRUE
  THEN DEST[255:224] <- FFFFFFFFH;
  ELSE DEST[255:224] <- 000000000H; FI;

```

 Opcode/Instruction                      | Op/En| 64/32bit Mode| CPUID Feature Flag| Description                                  
 ---  | --- | --- | --- | ---
 0F C2 /r ib CMPPS xmm1, xmm2/m128, imm8 | RMI  | V/V          | SSE               | Compare packed single-precision floatingpoint
                                         |      |              |                   | values in xmm2/mem and xmm1 using imm8       
                                         |      |              |                   | as comparison predicate.                     
 VEX.NDS.128.0F.WIG C2 /r ib VCMPPS xmm1,| RVMI | V/V          | AVX               | Compare packed single-precision floatingpoint
 xmm2, xmm3/m128, imm8                   |      |              |                   | values in xmm3/m128 and xmm2 using bits      
                                         |      |              |                   | 4:0 of imm8 as a comparison predicate.       
 VEX.NDS.256.0F.WIG C2 /r ib VCMPPS ymm1,| RVMI | V/V          | AVX               | Compare packed single-precision floatingpoint
 ymm2, ymm3/m256, imm8                   |      |              |                   | values in ymm3/m256 and ymm2 using bits      
                                         |      |              |                   | 4:0 of imm8 as a comparison predicate.       

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
Performs a SIMD compare of the packed single-precision floating-point values
in the source operand (second operand) and the destination operand (first operand)
and returns the results of the comparison to the destination operand. The comparison
predicate operand (third operand) specifies the type of comparison performed
on each of the pairs of packed values. The result of each comparison is a doubleword
mask of all 1s (comparison true) or all 0s (comparison false). The sign of zero
is ignored for comparisons, so that -0.0 is equal to +0.0. 128-bit Legacy SSE
version: The first source and destination operand (first operand) is an XMM
register. The second source operand (second operand) can be an XMM register
or 128-bit memory location. The comparison predicate operand is an 8-bit immediate,
bits 2:0 of the immediate define the type of comparison to be performed (see
Table 3-7). Bits 7:3 of the immediate is reserved. Bits (VLMAX-1:128) of the
corresponding YMM destination register remain unchanged. Four comparisons are
performed with results written to bits 127:0 of the destination operand.

The unordered relationship is true when at least one of the two source operands
being compared is a NaN; the ordered relationship is true when neither source
operand is a NaN.

A subsequent computational instruction that uses the mask result in the destination
operand as an input operand will not generate a fault, because a mask of all
0s corresponds to a floating-point value of +0.0 and a mask of all 1s corresponds
to a QNaN.

<aside class="notification">
Note that processors with “CPUID.1H:ECX.AVX =0” do not implement the “greater-than”,
“greater-than-or-equal”, “not-greater than”, and “not-greater-than-or-equal
relations” predicates. These comparisons can be made either by using the inverse
relationship (that is, use the “not-less-than-or-equal” to make a “greater-than”
comparison) or by using software emulation. When using software emulation, the
program must swap the operands (copying registers when necessary to protect
the data that will now be in the destination), and then perform the compare
using a different predicate. The predicate to be used for these emulations is
listed in Table 3-7 under the heading Emulation.
</aside>

Compilers and assemblers may implement the following two-operand pseudo-ops
in addition to the three-operand CMPPS instruction, for processors with “CPUID.1H:ECX.AVX
=0”. See Table 3-11. Compiler should treat reserved Imm8 values as illegal syntax.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).


### Table 3-11. Pseudo-Ops and CMPPS
   | |  
---- | -----
 Pseudo-Op            | Implementation     
 CMPEQPS xmm1, xmm2   | CMPPS xmm1, xmm2, 0
 CMPLTPS xmm1, xmm2   | CMPPS xmm1, xmm2, 1
 CMPLEPS xmm1, xmm2   | CMPPS xmm1, xmm2, 2
 CMPUNORDPS xmm1, xmm2| CMPPS xmm1, xmm2, 3
 CMPNEQPS xmm1, xmm2  | CMPPS xmm1, xmm2, 4
 CMPNLTPS xmm1, xmm2  | CMPPS xmm1, xmm2, 5
 CMPNLEPS xmm1, xmm2  | CMPPS xmm1, xmm2, 6
 CMPORDPS xmm1, xmm2  | CMPPS xmm1, xmm2, 7
The greater-than relations not implemented by processor require more than one
instruction to emulate in software and therefore should not be implemented as
pseudo-ops. (For these, the programmer should reverse the operands of the corresponding
less than relations and use move instructions to ensure that the mask is moved
to the correct destination register and that the source operand is left intact.)

### Enhanced Comparison Predicate for VEX-Encoded VCMPPS VEX.128 encoded version
The first source operand (second operand) is an XMM register. The second source
operand (third operand) can be an XMM register or a 128-bit memory location.
Bits (VLMAX-1:128) of the destination YMM register are zeroed. Four comparisons
are performed with results written to bits 127:0 of the destination operand.
VEX.256 encoded version: The first source operand (second operand) is a YMM
register. The second source operand (third operand) can be a YMM register or
a 256-bit memory location. The destination operand (first operand) is a YMM
register. Eight comparisons are performed with results written to the destination
### operand. The comparison predicate operand is an 8-bit immediate

 - For instructions encoded using the VEX prefix, bits 4:0 define the type of comparison
to be performed (see Table 3-9). Bits 5 through 7 of the immediate are reserved.

Processors with “CPUID.1H:ECX.AVX =1” implement the full complement of 32 predicates
shown in Table 3-9, software emulation is no longer needed. Compilers and assemblers
may implement the following three-operand pseudo-ops in addition to the four-operand
VCMPPS instruction. See Table 3-12, where the notation of reg1 and reg2 represent
either XMM registers or YMM registers. Compiler should treat reserved Imm8 values
as illegal syntax. Alternately, intrinsics can map the pseudo-ops to pre-defined
constants to support a simpler intrinsic interface.

   | |  
---- | -----
 : Pseudo-Op VCMPEQPS reg1, reg2, reg3          | Table 3-12. Table 3-12.| Pseudo-Op and VCMPPS Implementation           
 VCMPLTPS reg1, reg2, reg3 VCMPLEPS reg1,       |                        | CMPPS Implementation VCMPPS reg1, reg2,       
 reg2, reg3 VCMPUNORDPS reg1, reg2, reg3        |                        | reg3, 0 VCMPPS reg1, reg2, reg3, 1 VCMPPS     
 VCMPNEQPS reg1, reg2, reg3 VCMPNLTPS           |                        | reg1, reg2, reg3, 2 VCMPPS reg1, reg2,        
 reg1, reg2, reg3 VCMPNLEPS reg1, reg2,         |                        | reg3, 3 VCMPPS reg1, reg2, reg3, 4 VCMPPS     
 reg3 VCMPORDPS reg1, reg2, reg3 VCMPEQ_UQPS    |                        | reg1, reg2, reg3, 5 VCMPPS reg1, reg2,        
 reg1, reg2, reg3 VCMPNGEPS reg1, reg2,         |                        | reg3, 6 VCMPPS reg1, reg2, reg3, 7 VCMPPS     
 reg3 VCMPNGTPS reg1, reg2, reg3 VCMPFALSEPS    |                        | reg1, reg2, reg3, 8 VCMPPS reg1, reg2,        
 reg1, reg2, reg3 Pseudo-Op VCMPNEQ_OQPS        |                        | reg3, 9 VCMPPS reg1, reg2, reg3, 0AH          
 reg1, reg2, reg3 VCMPGEPS reg1, reg2,          |                        | VCMPPS reg1, reg2, reg3, 0BH Pseudo-Op        
 reg3 VCMPGTPS reg1, reg2, reg3 VCMPTRUEPS      |                        | and VCMPPS Implementation CMPPS Implementation
 reg1, reg2, reg3 VCMPEQ_OSPS reg1, reg2,       |                        | VCMPPS reg1, reg2, reg3, 0CH VCMPPS           
 reg3 VCMPLT_OQPS reg1, reg2, reg3 VCMPLE_OQPS  |                        | reg1, reg2, reg3, 0DH VCMPPS reg1, reg2,      
 reg1, reg2, reg3 VCMPUNORD_SPS reg1,           |                        | reg3, 0EH VCMPPS reg1, reg2, reg3, 0FH        
 reg2, reg3 VCMPNEQ_USPS reg1, reg2,            |                        | VCMPPS reg1, reg2, reg3, 10H VCMPPS           
 reg3 VCMPNLT_UQPS reg1, reg2, reg3 VCMPNLE_UQPS|                        | reg1, reg2, reg3, 11H VCMPPS reg1, reg2,      
 reg1, reg2, reg3 VCMPORD_SPS reg1, reg2,       |                        | reg3, 12H VCMPPS reg1, reg2, reg3, 13H        
 reg3 VCMPEQ_USPS reg1, reg2, reg3 VCMPNGE_UQPS |                        | VCMPPS reg1, reg2, reg3, 14H VCMPPS           
 reg1, reg2, reg3 VCMPNGT_UQPS reg1,            |                        | reg1, reg2, reg3, 15H VCMPPS reg1, reg2,      
 reg2, reg3 VCMPFALSE_OSPS reg1, reg2,          |                        | reg3, 16H VCMPPS reg1, reg2, reg3, 17H        
 reg3 VCMPNEQ_OSPS reg1, reg2, reg3 VCMPGE_OQPS |                        | VCMPPS reg1, reg2, reg3, 18H VCMPPS           
 reg1, reg2, reg3 VCMPGT_OQPS reg1, reg2,       |                        | reg1, reg2, reg3, 19H VCMPPS reg1, reg2,      
 reg3 VCMPTRUE_USPS reg1, reg2, reg3            |                        | reg3, 1AH VCMPPS reg1, reg2, reg3, 1BH        
                                                |                        | VCMPPS reg1, reg2, reg3, 1CH VCMPPS           
                                                |                        | reg1, reg2, reg3, 1DH VCMPPS reg1, reg2,      
                                                |                        | reg3, 1EH VCMPPS reg1, reg2, reg3, 1FH        


### Intel C/C++ Compiler Intrinsic Equivalents
   | |  
---- | -----
 CMPPS for equality:                     | __m128 _mm_cmpeq_ps(__m128 a, __m128   
                                         | b)                                     
 CMPPS for less-than:                    | __m128 _mm_cmplt_ps(__m128 a, __m128   
                                         | b)                                     
 CMPPS for less-than-or-equal:           | __m128 _mm_cmple_ps(__m128 a, __m128   
                                         | b)                                     
 CMPPS for greater-than:                 | __m128 _mm_cmpgt_ps(__m128 a, __m128   
                                         | b)                                     
 CMPPS for greater-than-or-equal:        | __m128 _mm_cmpge_ps(__m128 a, __m128   
                                         | b)                                     
 CMPPS for inequality:                   | __m128 _mm_cmpneq_ps(__m128 a, __m128  
                                         | b)                                     
 CMPPS for not-less-than:                | __m128 _mm_cmpnlt_ps(__m128 a, __m128  
                                         | b)                                     
 CMPPS for not-greater-than:             | __m128 _mm_cmpngt_ps(__m128 a, __m128  
                                         | b)                                     
 CMPPS for not-greater-than-or-equal:    | __m128 _mm_cmpnge_ps(__m128 a, __m128  
                                         | b)                                     
 CMPPS for ordered:                      | __m128 _mm_cmpord_ps(__m128 a, __m128  
                                         | b)                                     
 CMPPS for unordered:                    | __m128 _mm_cmpunord_ps(__m128 a, __m128
                                         | b)                                     
 CMPPS for not-less-than-or-equal: __m256| __m128 _mm_cmpnle_ps(__m128 a, __m128  
 _mm256_cmp_ps(__m256 a, __m256 b, const | b)                                     
 int imm) __m128 _mm_cmp_ps(__m128 a,    |                                        
 __m128 b, const int imm)                |                                        

### SIMD Floating-Point Exceptions
Invalid if SNaN operand and invalid if QNaN and predicate as listed in above
table, Denormal.


### Other Exceptions
See Exceptions Type 2.
