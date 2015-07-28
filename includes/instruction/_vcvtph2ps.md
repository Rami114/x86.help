## VCVTPH2PS - Convert 16-bit FP Values to Single-Precision FP Values

> Operation
``` slim

vCvt_h2s(SRC1[15:0])
{
RETURN Cvt_Half_Precision_To_Single_Precision(SRC1[15:0]);
}
VCVTPH2PS (VEX.256 encoded version)
DEST[31:0] <-vCvt_h2s(SRC1[15:0]);
DEST[63:32] <-vCvt_h2s(SRC1[31:16]);
DEST[95:64] <-vCvt_h2s(SRC1[47:32]);
DEST[127:96] <-vCvt_h2s(SRC1[63:48]);
DEST[159:128] <-vCvt_h2s(SRC1[79:64]);
DEST[191:160] <-vCvt_h2s(SRC1[95:80]);
DEST[223:192] <-vCvt_h2s(SRC1[111:96]);
DEST[255:224] <-vCvt_h2s(SRC1[127:112]);
VCVTPH2PS (VEX.128 encoded version)
DEST[31:0] <-vCvt_h2s(SRC1[15:0]);
DEST[63:32] <-vCvt_h2s(SRC1[31:16]);
DEST[95:64] <-vCvt_h2s(SRC1[47:32]);
DEST[127:96] <-vCvt_h2s(SRC1[63:48]);
DEST[VLMAX-1:128] <-0

```

 Opcode/Instruction                      | Op/En| 64/32bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 VEX.256.66.0F38.W0 13 /r VCVTPH2PS ymm1,| RM   | V/V          | F16C              | Convert eight packed half precision        
 xmm2/m128                               |      |              |                   | (16-bit) floating-point values in xmm2/m128
                                         |      |              |                   | to packed single-precision floating-point  
                                         |      |              |                   | value in ymm1.                             
 VEX.128.66.0F38.W0 13 /r VCVTPH2PS xmm1,| RM   | V/V          | F16C              | Convert four packed half precision (16-bit)
 xmm2/m64                                |      |              |                   | floating-point values in xmm2/m64 to       
                                         |      |              |                   | packed single-precision floating-point     
                                         |      |              |                   | value in xmm1.                             

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Converts four/eight packed half precision (16-bits) floating-point values in
the low-order 64/128 bits of an XMM/YMM register or 64/128-bit memory location
to four/eight packed single-precision floating-point values and writes the converted
values into the destination XMM/YMM register. If case of a denormal operand,
the correct normal result is returned. MXCSR.DAZ is ignored and is treated as
if it 0. No denormal exception is reported on MXCSR. 128-bit version: The source
operand is a XMM register or 64-bit memory location. The destination operand
is a XMM register. The upper bits (VLMAX-1:128) of the corresponding destination
YMM register are zeroed. 256-bit version: The source operand is a XMM register
or 128-bit memory location. The destination operand is a YMM register. The diagram
below illustrates how data is converted from four packed half precision (in
64 bits) to four single precision (in 128 bits) FP values. Note: VEX.vvvv is
reserved (must be 1111b).

   | |  
---- | -----
 VCVTPH2PS xmm1, xmm2/mem64, 95 convert| imm8 0 xmm2/mem64 convert convert
 95 VS2                                | 0 xmm1                           
 Figure 4-31.                          | VCVTPH2PS (128-bit Version)      


### Flags Affected
None


### Intel C/C++ Compiler Intrinsic Equivalent
__m128 _mm_cvtph_ps ( __m128i m1);

__m256 _mm256_cvtph_ps ( __m128i m1)


### SIMD Floating-Point Exceptions
Invalid


### Other Exceptions
Exceptions Type 11 (do not report #AC); additionally

   | |  
---- | -----
 #UD| If VEX.W=1.
