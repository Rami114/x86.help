## COMISS - Compare Scalar Ordered Single-Precision Floating-Point Values and Set EFLAGS

> Operation

``` slim
RESULT <- OrderedCompare(SRC1[31:0] <> SRC2[31:0]) {
(\* Set EFLAGS \*) CASE (RESULT) OF```

###   UNORDERED
###   GREATER_THAN
###   LESS_THAN
###   EQUAL
ESAC;
OF,AF,SF <- 0; }

> Intel C/C++ Compiler Intrinsic Equivalents

``` slim
int _mm_comieq_ss (__m128 a, __m128 b) int _mm_comilt_ss (__m128 a, __m128 b)
int _mm_comile_ss (__m128 a, __m128 b) int _mm_comigt_ss (__m128 a, __m128 b)
int _mm_comige_ss (__m128 a, __m128 b) int _mm_comineq_ss (__m128 a, __m128
b)


```

 Opcode/Instruction                         | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 0F 2F /r COMISS xmm1, xmm2/m32             | RM   | V/V           | SSE               | Compare low single-precision floating-point
                                            |      |               |                   | values in xmm1 and xmm2/mem32 and set      
                                            |      |               |                   | the EFLAGS flags accordingly.              
 VEX.LIG.0F.WIG 2F /r VCOMISS xmm1, xmm2/m32| RM   | V/V           | AVX               | Compare low single precision floating-point
                                            |      |               |                   | values in xmm1 and xmm2/mem32 and set      
                                            |      |               |                   | the EFLAGS flags accordingly.              

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r)| ModRM:r/m (r)| NA       | NA       

### Description
Compares the single-precision floating-point values in the low doublewords of
operand 1 (first operand) and operand 2 (second operand), and sets the ZF, PF,
and CF flags in the EFLAGS register according to the result (unordered, greater
than, less than, or equal). The OF, SF, and AF flags in the EFLAGS register
are set to 0. The unordered result is returned if either source operand is a
NaN (QNaN or SNaN). The sign of zero is ignored for comparisons, so that -0.0
is equal to +0.0.

Operand 1 is an XMM register; Operand 2 can be an XMM register or a 32 bit memory
location.

The COMISS instruction differs from the UCOMISS instruction in that it signals
a SIMD floating-point invalid operation exception (**``#I)``** when a source operand
is either a QNaN or SNaN. The UCOMISS instruction signals an invalid numeric
exception only if a source operand is an SNaN.

The EFLAGS register is not updated if an unmasked SIMD floating-point exception
is generated.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). Note: In VEX-encoded versions, VEX.vvvv is reserved
and must be 1111b, otherwise instructions will **``#UD.``**



### SIMD Floating-Point Exceptions
Invalid (if SNaN or QNaN operands), Denormal.


### Other Exceptions
See Exceptions Type 3; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
