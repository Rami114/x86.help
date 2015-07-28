## VCVTPS2PH - Convert Single-Precision FP value to 16-bit FP value

> Operation

``` slim
vCvt_s2h(SRC1[31:0])
{
IF Imm[2] = 0
THEN // using Imm[1:0] for rounding control, see Table 4-16
     RETURN Cvt_Single_Precision_To_Half_Precision_FP_Imm(SRC1[31:0]);
ELSE // using MXCSR.RC for rounding control
     RETURN Cvt_Single_Precision_To_Half_Precision_FP_Mxcsr(SRC1[31:0]);
FI;
}
VCVTPS2PH (VEX.256 encoded version)
DEST[15:0] <- vCvt_s2h(SRC1[31:0]);
DEST[31:16] <- vCvt_s2h(SRC1[63:32]);
DEST[47:32] <- vCvt_s2h(SRC1[95:64]);
DEST[63:48] <- vCvt_s2h(SRC1[127:96]);
DEST[79:64] <- vCvt_s2h(SRC1[159:128]);
DEST[95:80] <- vCvt_s2h(SRC1[191:160]);
DEST[111:96] <- vCvt_s2h(SRC1[223:192]);
DEST[127:112] <- vCvt_s2h(SRC1[255:224]);
DEST[255:128] <- 0; // if DEST is a register
VCVTPS2PH (VEX.128 encoded version)
DEST[15:0] <- vCvt_s2h(SRC1[31:0]);
DEST[31:16] <- vCvt_s2h(SRC1[63:32]);
DEST[47:32] <- vCvt_s2h(SRC1[95:64]);
DEST[63:48] <- vCvt_s2h(SRC1[127:96]);
DEST[VLMAX-1:64] <-0; // if DEST is a register

```

 Opcode/Instruction                   | Op/En  | 64/32bit Mode| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 VEX.256.66.0F3A.W0 1D /r ib VCVTPS2PH| MR imm8| V/V          | F16C              | Convert eight packed single-precision    
 xmm1/m128, ymm2,                     |        |              |                   | floating-point value in ymm2 to packed   
                                      |        |              |                   | half-precision (16-bit) floating-point   
                                      |        |              |                   | value in xmm1/mem. Imm8 provides rounding
                                      |        |              |                   | controls.                                
 VEX.128.66.0F3A.W0.1D /r ib VCVTPS2PH| MR imm8| V/V          | F16C              | Convert four packed single-precision     
 xmm1/m64, xmm2,                      |        |              |                   | floating-point value in xmm2 to packed   
                                      |        |              |                   | halfprecision (16-bit) floating-point    
                                      |        |              |                   | value in xmm1/mem. Imm8 provides rounding
                                      |        |              |                   | controls.                                

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
Convert four or eight packed single-precision floating values in first source
operand to four or eight packed halfprecision (16-bit) floating-point values.
The rounding mode is specified using the immediate field (imm8). Underflow results
(i.e. tiny results) are converted to denormals. MXCSR.FTZ is ignored. If a source
element is denormal relative to input format with MXCSR.DAZ not set, DM masked
and at least one of PM or UM unmasked; a SIMD exception will be raised with
DE, UE and PE set. 128-bit version: The source operand is a XMM register. The
destination operand is a XMM register or 64-bit memory location. The upper-bits
vector register zeroing behavior of VEX prefix encoding still applies if the
destination operand is a xmm register. So the upper bits (255:64) of corresponding
YMM register are zeroed. 256-bit version: The source operand is a YMM register.
The destination operand is a XMM register or 128-bit memory location. The upper-bits
vector register zeroing behavior of VEX prefix encoding still applies if the
destination operand is a xmm register. So the upper bits (255:128) of the corresponding
YMM register are zeroed. Note: VEX.vvvv is reserved (must be 1111b). The diagram
below illustrates how data is converted from four packed single precision (in
128 bits) to four half precision (in 64 bits) FP values.

   | |  
---- | -----
 VCVTPS2PH xmm1/mem64, xmm2, 95 VS2| imm8 0 xmm2                
 convert                           | convert convert            
 95                                | 0 xmm1/mem64               
 Figure 4-32.                      | VCVTPS2PH (128-bit Version)
The immediate byte defines several bit fields that controls rounding operation.
The effect and encoding of RC field are listed in Table 4-16.


### Table 4-16. Immediate Byte Encoding for 16-bit Floating-Point Conversion Instructions
   | |  
---- | -----
 Bits          | Field Name/value           | Description                           | Comment        
 Imm[1:0]      | RC=00B RC=01B RC=10B RC=11B| Round to nearest even Round down Round| If Imm[2] = 0  
               |                            | up Truncate                           |                
 Imm[2]Imm[7:3]| MS1=0 MS1=1 Ignored        | Use imm[1:0] for rounding Use MXCSR.RC| Ignore MXCSR.RC
               |                            | for rounding Ignored by processor     |                


### Flags Affected
None


### Intel C/C++ Compiler Intrinsic Equivalent
__m128i _mm_cvtps_ph ( __m128 m1, const int imm);

__m128i _mm256_cvtps_ph(__m256 m1, const int imm);


### SIMD Floating-Point Exceptions
Invalid, Underflow, Overflow, Precision, Denormal (if MXCSR.DAZ=0);


### Other Exceptions
Exceptions Type 11 (do not report #AC); additionally

   | |  
---- | -----
 **``#UD``**| If VEX.W=1.
