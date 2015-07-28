## UCOMISD - Unordered Compare Scalar Double-Precision Floating-Point Values and Set EFLAGS

> Operation

``` slim
RESULT <- UnorderedCompare(SRC1[63:0] < > SRC2[63:0]) {
(\* Set EFLAGS \*)
CASE (RESULT) OF
```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                 
 ---  | --- | --- | --- | ---
 66 0F 2E /r UCOMISD xmm1, xmm2/m64    | RM   | V/V                   | SSE2              | Compares (unordered) the low doubleprecision
                                       |      |                       |                   | floating-point values in xmm1 and xmm2/m64  
                                       |      |                       |                   | and set the EFLAGS accordingly.             
 VEX.LIG.66.0F.WIG 2E /r VUCOMISD xmm1,| RM   | V/V                   | AVX               | Compare low double precision floating-point 
 xmm2/m64                              |      |                       |                   | values in xmm1 and xmm2/mem64 and set       
                                       |      |                       |                   | the EFLAGS flags accordingly.               

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r)| ModRM:r/m (r)| NA       | NA       

### Description
Performs an unordered compare of the double-precision floating-point values
in the low quadwords of source operand 1 (first operand) and source operand
2 (second operand), and sets the ZF, PF, and CF flags in the EFLAGS register
according to the result (unordered, greater than, less than, or equal). The
OF, SF and AF flags in the EFLAGS register are set to 0. The unordered result
is returned if either source operand is a NaN (QNaN or SNaN). The sign of zero
is ignored for comparisons, so that -0.0 is equal to +0.0.

Source operand 1 is an XMM register; source operand 2 can be an XMM register
or a 64 bit memory location.

The UCOMISD instruction differs from the COMISD instruction in that it signals
a SIMD floating-point invalid operation exception (#I) only when a source operand
is an SNaN. The COMISD instruction signals an invalid operation exception if
a source operand is either a QNaN or an SNaN.

The EFLAGS register is not updated if an unmasked SIMD floating-point exception
is generated.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). Note: In VEX-encoded versions,
VEX.vvvv is reserved and must be 1111b, otherwise instructions will #UD.



###   UNORDERED
###   GREATER_THAN
###   LESS_THAN
###   EQUAL
ESAC;
OF, AF, SF <- 0;

### Intel C/C++ Compiler Intrinsic Equivalent
int _mm_ucomieq_sd(__m128d a, __m128d b) int _mm_ucomilt_sd(__m128d a, __m128d
b) int _mm_ucomile_sd(__m128d a, __m128d b) int _mm_ucomigt_sd(__m128d a, __m128d
b) int _mm_ucomige_sd(__m128d a, __m128d b) int _mm_ucomineq_sd(__m128d a, __m128d
b)


### SIMD Floating-Point Exceptions
Invalid (if SNaN operands), Denormal.


### Other Exceptions
See Exceptions Type 3; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
