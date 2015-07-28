## CMPPD - Compare Packed Double-Precision Floating-Point Values

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
  DEFAULT: Reserved;
CMPPD (128-bit Legacy SSE version)
CMP0 <- SRC1[63:0] OP3 SRC2[63:0];
CMP1 <- SRC1[127:64] OP3 SRC2[127:64];
IF CMP0 = TRUE
  THEN DEST[63:0] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[63:0] <- 0000000000000000H; FI;
IF CMP1 = TRUE
  THEN DEST[127:64] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[127:64] <- 0000000000000000H; FI;
DEST[VLMAX-1:128] (Unmodified)
VCMPPD (VEX.128 encoded version)
CMP0 <- SRC1[63:0] OP5 SRC2[63:0];
CMP1 <- SRC1[127:64] OP5 SRC2[127:64];
IF CMP0 = TRUE
  THEN DEST[63:0] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[63:0] <- 0000000000000000H; FI;
IF CMP1 = TRUE
  THEN DEST[127:64] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[127:64] <- 0000000000000000H; FI;
DEST[VLMAX-1:128] <- 0
VCMPPD (VEX.256 encoded version)
CMP0 <- SRC1[63:0] OP5 SRC2[63:0];
CMP1 <- SRC1[127:64] OP5 SRC2[127:64];
CMP2 <- SRC1[191:128] OP5 SRC2[191:128];
CMP3 <- SRC1[255:192] OP5 SRC2[255:192];
IF CMP0 = TRUE
  THEN DEST[63:0] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[63:0] <- 0000000000000000H; FI;
IF CMP1 = TRUE
  THEN DEST[127:64] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[127:64] <- 0000000000000000H; FI;
IF CMP2 = TRUE
  THEN DEST[191:128] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[191:128] <- 0000000000000000H; FI;
IF CMP3 = TRUE
  THEN DEST[255:192] <- FFFFFFFFFFFFFFFFH;
  ELSE DEST[255:192] <- 0000000000000000H; FI;

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
   | |  
---- | -----
 CMPPD for equality:                               | __m128d _mm_cmpeq_pd(__m128d a, __m128d   
                                                   | b)                                        
 CMPPD for less-than: CMPPD for less-than-or-equal:| __m128d _mm_cmplt_pd(__m128d a, __m128d   
 __m128d _mm_cmple_pd(__m128d a, __m128d           | b)                                        
 b)                                                |                                           
 CMPPD for greater-than:                           | __m128d _mm_cmpgt_pd(__m128d a, __m128d   
                                                   | b)                                        
 CMPPD for greater-than-or-equal:                  | __m128d _mm_cmpge_pd(__m128d a, __m128d   
                                                   | b)                                        
 CMPPD for inequality:                             | __m128d _mm_cmpneq_pd(__m128d a, __m128d  
                                                   | b)                                        
 CMPPD for not-less-than:                          | __m128d _mm_cmpnlt_pd(__m128d a, __m128d  
                                                   | b)                                        
 CMPPD for not-greater-than:                       | __m128d _mm_cmpngt_pd(__m128d a, __m128d  
                                                   | b)                                        
 CMPPD for not-greater-than-or-equal:              | __m128d _mm_cmpnge_pd(__m128d a, __m128d  
                                                   | b)                                        
 CMPPD for ordered:                                | __m128d _mm_cmpord_pd(__m128d a, __m128d  
                                                   | b)                                        
 CMPPD for unordered:                              | __m128d _mm_cmpunord_pd(__m128d a, __m128d
                                                   | b)                                        
 CMPPD for not-less-than-or-equal: __m256          | __m128d _mm_cmpnle_pd(__m128d a, __m128d  
 _mm256_cmp_pd(__m256 a, __m256 b, const           | b)                                        
 int imm)                                          |                                           
 VCMPPD:                                           | __m128 _mm_cmp_pd(__m128 a, __m128 b,     
                                                   | const int imm)                            

```

 Opcode/Instruction                   | Op/En| 64/32bit Mode| CPUID Feature Flag| Description                                  
 ---  | --- | --- | --- | ---
 66 0F C2 /r ib CMPPD xmm1, xmm2/m128,| RMI  | V/V          | SSE2              | Compare packed double-precision floatingpoint
 imm8                                 |      |              |                   | values in xmm2/m128 and xmm1 using imm8      
                                      |      |              |                   | as comparison predicate.                     
 VEX.NDS.128.66.0F.WIG C2 /r ib VCMPPD| RVMI | V/V          | AVX               | Compare packed double-precision floatingpoint
 xmm1, xmm2, xmm3/m128, imm8          |      |              |                   | values in xmm3/m128 and xmm2 using bits      
                                      |      |              |                   | 4:0 of imm8 as a comparison predicate.       
 VEX.NDS.256.66.0F.WIG C2 /r ib VCMPPD| RVMI | V/V          | AVX               | Compare packed double-precision floatingpoint
 ymm1, ymm2, ymm3/m256, imm8          |      |              |                   | values in ymm3/m256 and ymm2 using bits      
                                      |      |              |                   | 4:0 of imm8 as a comparison predicate.       

### Instruction Operand Encoding
 Op/En| Operand 1       | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (r, w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)   | VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
Performs a SIMD compare of the packed double-precision floating-point values
in the source operand (second operand) and the destination operand (first operand)
and returns the results of the comparison to the destination operand. The comparison
predicate operand (third operand) specifies the type of comparison performed
on each of the pairs of packed values. The result of each comparison is a quadword
mask of all 1s (comparison true) or all 0s (comparison false). The sign of zero
is ignored for comparisons, so that -0.0 is equal to +0.0. 128-bit Legacy SSE
version: The first source and destination operand (first operand) is an XMM
register. The second source operand (second operand) can be an XMM register
or 128-bit memory location. The comparison predicate operand is an 8-bit immediate,
bits 2:0 of the immediate define the type of comparison to be performed (see
Table 3-7). Bits 7:3 of the immediate is reserved. Bits (VLMAX-1:128) of the
corresponding YMM destination register remain unchanged. Two comparisons are
performed with results written to bits 127:0 of the destination operand.


### Table 3-7. Comparison Predicate for CMPPD and CMPPS Instructions
   | |  
---- | -----
 Predicate EQ LT LE UNORD NEQ NLT| imm8 Encoding 000B 001B 010B 011B 100B| Description Equal Less-than Less-than-or-equal     | Relation where: A Is 1st Operand B Is  | Emulation Swap Operands, Use LT Swap | Result if NaN Operand False False False| QNaN Oper-and Signals Invalid No Yes 
                                 | 101B                                  | Greater than Greater-than-or-equal Unordered       | 2nd Operand A = B A < B A ≤ B A > B    | Operands, Use LE                     | False False True True True             | Yes Yes Yes No No Yes (Contd.)       
                                 |                                       | Not-equal Not-less-than Table 3-7.                 | A ≥ B A, B = Unordered A != B NOT(A <   |                                      |                                        |                                      
                                 |                                       |                                                    | B) Comparison Predicate for CMPPD and  |                                      |                                        |                                      
                                 |                                       |                                                    | CMPPS Instructions                     |                                      |                                        |                                      
 Predicate NLE ORD               | imm8 Encoding 110B 111B               | Description Not-less-than-or-equal Not-greater-than| Relation where: A Is 1st Operand B Is  | Emulation Swap Operands, Use NLT Swap| Result if NaN Operand True True True   | QNaN Oper-and Signals Invalid Yes Yes
                                 |                                       | Not-greater-than-or-equal Ordered                  | 2nd Operand NOT(A ≤ B) NOT(A > B) NOT(A| Operands, Use NLE                    | False                                  | Yes No                               
                                 |                                       |                                                    | ≥ B) A , B = Ordered                   |                                      |                                        |                                      
The unordered relationship is true when at least one of the two source operands
being compared is a NaN; the ordered relationship is true when neither source
operand is a NaN.

A subsequent computational instruction that uses the mask result in the destination
operand as an input operand will not generate an exception, because a mask of
all 0s corresponds to a floating-point value of +0.0 and a mask of all 1s corresponds
to a QNaN.

<aside class="notification">
Note that the processors with “CPUID.1H:ECX.AVX =0” do not implement the greater-than,
greater-than-or-equal, not-greater-than, and not-greater-than-or-equal relations.
These comparisons can be made either by using the inverse relationship (that
is, use the “not-less-than-or-equal” to make a “greater-than” comparison) or
by using software emulation. When using software emulation, the program must
swap the operands (copying registers when necessary to protect the data that
will now be in the destination), and then perform the compare using a different
predicate. The predicate to be used for these emulations is listed in Table
3-7 under the heading Emulation.
</aside>

Compilers and assemblers may implement the following two-operand pseudo-ops
in addition to the three-operand CMPPD instruction, for processors with “CPUID.1H:ECX.AVX
=0”. See Table 3-8. Compiler should treat reserved Imm8 values as illegal syntax.

   | |  
---- | -----
 : Pseudo-Op CMPEQPD xmm1, xmm2 CMPLTPD  | Table 3-8.| Pseudo-Op and CMPPD Implementation CMPPD
 xmm1, xmm2 CMPLEPD xmm1, xmm2 CMPUNORDPD|           | Implementation CMPPD xmm1, xmm2, 0 CMPPD
 xmm1, xmm2 CMPNEQPD xmm1, xmm2 CMPNLTPD |           | xmm1, xmm2, 1 CMPPD xmm1, xmm2, 2 CMPPD 
 xmm1, xmm2 CMPNLEPD xmm1, xmm2 CMPORDPD |           | xmm1, xmm2, 3 CMPPD xmm1, xmm2, 4 CMPPD 
 xmm1, xmm2                              |           | xmm1, xmm2, 5 CMPPD xmm1, xmm2, 6 CMPPD 
                                         |           | xmm1, xmm2, 7                           
The greater-than relations that the processor does not implement, require more
than one instruction to emulate in software and therefore should not be implemented
as pseudo-ops. (For these, the programmer should reverse the operands of the
corresponding less than relations and use move instructions to ensure that the
mask is moved to the correct destination register and that the source operand
is left intact.)

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).

### Enhanced Comparison Predicate for VEX-Encoded VCMPPD VEX.128 encoded version
The first source operand (second operand) is an XMM register. The second source
operand (third operand) can be an XMM register or a 128-bit memory location.
Bits (VLMAX-1:128) of the destination YMM register are zeroed. Two comparisons
are performed with results written to bits 127:0 of the destination operand.

VEX.256 encoded version: The first source operand (second operand) is a YMM
register. The second source operand (third operand) can be a YMM register or
a 256-bit memory location. The destination operand (first operand) is a YMM
register. Four comparisons are performed with results written to the destination
### operand. The comparison predicate operand is an 8-bit immediate

 - For instructions encoded using the VEX prefix, bits 4:0 define the type of comparison
to be performed (see Table 3-9). Bits 5 through 7 of the immediate are reserved.
Table 3-9. Comparison Predicate for VCMPPD and VCMPPS Instructions

   | |  
---- | -----
 Predicate      | imm8 Value    | Description                                     | Result: A Is 1st Operand, B Is 2nd Operand| Signals **``#IA``** on QNAN Unordered1
                |               |                                                 | A = B                                     |                               
 EQ_OQ (EQ)     | 0H            | Equal (ordered, non-signaling)                  | True                                      | No                            
 LT_OS (LT)     | 1H            | Less-than (ordered, signaling)                  | False                                     | Yes                           
 LE_OS (LE)     | 2H            | Less-than-or-equal (ordered, signaling)         | True                                      | Yes                           
 UNORD_Q (UNORD)| 3H            | Unordered (non-signaling)                       | False                                     | No                            
 NEQ_UQ (NEQ)   | 4H            | Not-equal (unordered, nonsignaling)             | False                                     | No                            
 NLT_US (NLT)   | 5H            | Not-less-than (unordered, signaling)            | True                                      | Yes                           
 NLE_US (NLE)   | 6H            | Not-less-than-or-equal (unordered, signaling)   | False                                     | Yes                           
 ORD_Q (ORD)    | 7H            | Ordered (non-signaling)                         | True                                      | No                            
 EQ_UQ          | 8H            | Equal (unordered, non-signaling)                | True                                      | No                            
 NGE_US (NGE)   | 9H            | Not-greater-than-or-equal (unordered,           | False                                     | Yes                           
                |               | signaling)                                      |                                           |                               
 NGT_US (NGT)   | AH            | Not-greater-than (unordered, signaling)         | True                                      | Yes                           
 FALSE_OQ(FALSE)| BH            | False (ordered, non-signaling)                  | False                                     | No                            
 NEQ_OQ         | CH            | Not-equal (ordered, non-signaling)              | False                                     | No                            
 GE_OS (GE)     | DH            | Greater-than-or-equal (ordered, signaling)      | True                                      | Yes                           
 GT_OS (GT)     | EH            | Greater-than (ordered, signaling)               | False                                     | Yes                           
 TRUE_UQ(TRUE)  | FH            | True (unordered, non-signaling)                 | True                                      | No                            
 EQ_OS          | 10H           | Equal (ordered, signaling)                      | True                                      | Yes                           
 LT_OQ          | 11H           | Less-than (ordered, nonsignaling)               | False                                     | No                            
 LE_OQ          | 12H           | Less-than-or-equal (ordered, nonsignaling)      | True                                      | No                            
 UNORD_S        | 13H           | Unordered (signaling)                           | False                                     | Yes                           
 NEQ_US         | 14H           | Not-equal (unordered, signaling)                | False                                     | Yes                           
 NLT_UQ         | 15H           | Not-less-than (unordered, nonsignaling)         | True                                      | No                            
 NLE_UQ         | 16H           | Not-less-than-or-equal (unordered, nonsignaling)| False                                     | No                            
 ORD_S          | 17H           | Ordered (signaling)                             | True                                      | Yes                           
 EQ_US          | 18H Table 3-9.| Equal (unordered, signaling) Comparison         | True                                      | Yes (Contd.)                  
                |               | Predicate for VCMPPD and VCMPPS Instructions    |                                           |                               
 Predicate      | imm8 Value    | Description                                     | Result: A Is 1st Operand, B Is 2nd Operand| Signals **``#IA``** on QNAN Unordered1
                |               |                                                 | A = B                                     |                               
 NGE_UQ         | 19H           | Not-greater-than-or-equal (unordered,           | False                                     | No                            
                |               | nonsignaling)                                   |                                           |                               
 NGT_UQ         | 1AH           | Not-greater-than (unordered, nonsignaling)      | True                                      | No                            
 FALSE_OS       | 1BH           | False (ordered, signaling)                      | False                                     | Yes                           
 NEQ_OS         | 1CH           | Not-equal (ordered, signaling)                  | False                                     | Yes                           
 GE_OQ          | 1DH           | Greater-than-or-equal (ordered, nonsignaling)   | True                                      | No                            
 GT_OQ          | 1EH           | Greater-than (ordered, nonsignaling)            | False                                     | No                            
 TRUE_US        | 1FH           | True (unordered, signaling)                     | True                                      | Yes                           
<aside class="notification">
1. If either operand A or B is a NAN.
</aside>

Processors with “CPUID.1H:ECX.AVX =1” implement the full complement of 32 predicates
shown in Table 3-9, software emulation is no longer needed. Compilers and assemblers
may implement the following three-operand pseudo-ops in addition to the four-operand
VCMPPD instruction. See Table 3-10, where the notations of reg1 reg2, and reg3
represent either XMM registers or YMM registers. Compiler should treat reserved
Imm8 values as illegal syntax. Alternately, intrinsics can map the pseudo-ops
to pre-defined constants to support a simpler intrinsic interface.

   | |  
---- | -----
 : Pseudo-Op VCMPEQPD reg1, reg2, reg3          | Table 3-10. Table 3-10.| Pseudo-Op and VCMPPD Implementation      
 VCMPLTPD reg1, reg2, reg3 VCMPLEPD reg1,       |                        | CMPPD Implementation VCMPPD reg1, reg2,  
 reg2, reg3 VCMPUNORDPD reg1, reg2, reg3        |                        | reg3, 0 VCMPPD reg1, reg2, reg3, 1 VCMPPD
 VCMPNEQPD reg1, reg2, reg3 VCMPNLTPD           |                        | reg1, reg2, reg3, 2 VCMPPD reg1, reg2,   
 reg1, reg2, reg3 VCMPNLEPD reg1, reg2,         |                        | reg3, 3 VCMPPD reg1, reg2, reg3, 4 VCMPPD
 reg3 VCMPORDPD reg1, reg2, reg3 VCMPEQ_UQPD    |                        | reg1, reg2, reg3, 5 VCMPPD reg1, reg2,   
 reg1, reg2, reg3 VCMPNGEPD reg1, reg2,         |                        | reg3, 6 VCMPPD reg1, reg2, reg3, 7 VCMPPD
 reg3 VCMPNGTPD reg1, reg2, reg3 VCMPFALSEPD    |                        | reg1, reg2, reg3, 8 VCMPPD reg1, reg2,   
 reg1, reg2, reg3 VCMPNEQ_OQPD reg1,            |                        | reg3, 9 VCMPPD reg1, reg2, reg3, 0AH     
 reg2, reg3 VCMPGEPD reg1, reg2, reg3           |                        | VCMPPD reg1, reg2, reg3, 0BH VCMPPD      
 VCMPGTPD reg1, reg2, reg3 VCMPTRUEPD           |                        | reg1, reg2, reg3, 0CH VCMPPD reg1, reg2, 
 reg1, reg2, reg3 VCMPEQ_OSPD reg1, reg2,       |                        | reg3, 0DH VCMPPD reg1, reg2, reg3, 0EH   
 reg3 VCMPLT_OQPD reg1, reg2, reg3 VCMPLE_OQPD  |                        | VCMPPD reg1, reg2, reg3, 0FH VCMPPD      
 reg1, reg2, reg3 Pseudo-Op VCMPUNORD_SPD       |                        | reg1, reg2, reg3, 10H VCMPPD reg1, reg2, 
 reg1, reg2, reg3 VCMPNEQ_USPD reg1,            |                        | reg3, 11H VCMPPD reg1, reg2, reg3, 12H   
 reg2, reg3 VCMPNLT_UQPD reg1, reg2,            |                        | Pseudo-Op and VCMPPD Implementation      
 reg3 VCMPNLE_UQPD reg1, reg2, reg3 VCMPORD_SPD |                        | CMPPD Implementation VCMPPD reg1, reg2,  
 reg1, reg2, reg3 VCMPEQ_USPD reg1, reg2,       |                        | reg3, 13H VCMPPD reg1, reg2, reg3, 14H   
 reg3 VCMPNGE_UQPD reg1, reg2, reg3 VCMPNGT_UQPD|                        | VCMPPD reg1, reg2, reg3, 15H VCMPPD      
 reg1, reg2, reg3 VCMPFALSE_OSPD reg1,          |                        | reg1, reg2, reg3, 16H VCMPPD reg1, reg2, 
 reg2, reg3 VCMPNEQ_OSPD reg1, reg2,            |                        | reg3, 17H VCMPPD reg1, reg2, reg3, 18H   
 reg3 VCMPGE_OQPD reg1, reg2, reg3 VCMPGT_OQPD  |                        | VCMPPD reg1, reg2, reg3, 19H VCMPPD      
 reg1, reg2, reg3 VCMPTRUE_USPD reg1,           |                        | reg1, reg2, reg3, 1AH VCMPPD reg1, reg2, 
 reg2, reg3                                     |                        | reg3, 1BH VCMPPD reg1, reg2, reg3, 1CH   
                                                |                        | VCMPPD reg1, reg2, reg3, 1DH VCMPPD      
                                                |                        | reg1, reg2, reg3, 1EH VCMPPD reg1, reg2, 
                                                |                        | reg3, 1FH                                


### SIMD Floating-Point Exceptions
Invalid if SNaN operand and invalid if QNaN and predicate as listed in above
table, Denormal.


### Other Exceptions
See Exceptions Type 2.
