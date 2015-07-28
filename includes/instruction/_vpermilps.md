## VPERMILPS  -  Permute Single-Precision Floating-Point Values

> Operation

``` slim
Select4(SRC, control) {
CASE (control[1:0]) OF```

###   0
###   1
###   2
###   3
ESAC;
RETURN TMP
}
VPERMILPS (256-bit immediate version)
DEST[31:0] <- Select4(SRC1[127:0], imm8[1:0]);
DEST[63:32] <- Select4(SRC1[127:0], imm8[3:2]);
DEST[95:64] <- Select4(SRC1[127:0], imm8[5:4]);
DEST[127:96] <- Select4(SRC1[127:0], imm8[7:6]);
DEST[159:128] <- Select4(SRC1[255:128], imm8[1:0]);
DEST[191:160] <- Select4(SRC1[255:128], imm8[3:2]);
DEST[223:192] <- Select4(SRC1[255:128], imm8[5:4]);
DEST[255:224] <- Select4(SRC1[255:128], imm8[7:6]);
VPERMILPS (128-bit immediate version)
DEST[31:0] <- Select4(SRC1[127:0], imm8[1:0]);
DEST[63:32] <- Select4(SRC1[127:0], imm8[3:2]);
DEST[95:64] <- Select4(SRC1[127:0], imm8[5:4]);
DEST[127:96] <- Select4(SRC1[127:0], imm8[7:6]);
DEST[VLMAX-1:128] <- 0
VPERMILPS (256-bit variable version)
DEST[31:0] <- Select4(SRC1[127:0], SRC2[1:0]);
DEST[63:32] <- Select4(SRC1[127:0], SRC2[33:32]);
DEST[95:64] <- Select4(SRC1[127:0], SRC2[65:64]);
DEST[127:96] <- Select4(SRC1[127:0], SRC2[97:96]);
DEST[159:128] <- Select4(SRC1[255:128], SRC2[129:128]);
DEST[191:160] <- Select4(SRC1[255:128], SRC2[161:160]);
DEST[223:192] <- Select4(SRC1[255:128], SRC2[193:192]);
DEST[255:224] <- Select4(SRC1[255:128], SRC2[225:224]);
VPERMILPS (128-bit variable version)
DEST[31:0] <- Select4(SRC1[127:0], SRC2[1:0]);
DEST[63:32] <- Select4(SRC1[127:0], SRC2[33:32]);
DEST[95:64] <- Select4(SRC1[127:0], SRC2[65:64]);
DEST[127:96] <- Select4(SRC1[127:0], SRC2[97:96]);
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 VPERM1LPS:| __m128 _mm_permute_ps (__m128 a, int
           | control);                           
 VPERM1LPS:| __m256 _mm256_permute_ps (__m256 a, 
           | int control);                       
 VPERM1LPS:| __m128 _mm_permutevar_ps (__m128 a, 
           | __m128i control);                   
 VPERM1LPS:| __m256 _mm256_permutevar_ps (__m256 
           | a, __m256i control);                

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 VEX.NDS.128.66.0F38.W0 0C /r VPERMILPS| RVM  | V/V                   | AVX               | Permute single-precision floating-point    
 xmm1, xmm2, xmm3/m128                 |      |                       |                   | values in xmm2 using controls from xmm3/mem
                                       |      |                       |                   | and store result in xmm1.                  
 VEX.128.66.0F3A.W0 04 /r ib VPERMILPS | RMI  | V/V                   | AVX               | Permute single-precision floating-point    
 xmm1, xmm2/m128, imm8                 |      |                       |                   | values in xmm2/mem using controls from     
                                       |      |                       |                   | imm8 and store result in xmm1.             
 VEX.NDS.256.66.0F38.W0 0C /r VPERMILPS| RVM  | V/V                   | AVX               | Permute single-precision floating-point    
 ymm1, ymm2, ymm3/m256                 |      |                       |                   | values in ymm2 using controls from ymm3/mem
                                       |      |                       |                   | and store result in ymm1.                  
 VEX.256.66.0F3A.W0 04 /r ib VPERMILPS | RMI  | V/V                   | AVX               | Permute single-precision floating-point    
 ymm1, ymm2/m256, imm8                 |      |                       |                   | values in ymm2/mem using controls from     
                                       |      |                       |                   | imm8 and store result in ymm1.             

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8         | NA       

### Description
(variable control version) Permute single-precision floating-point values in
the first source operand (second operand) using 8-bit control fields in the
low bytes of corresponding elements the shuffle control (third operand) and
store results in the destination operand (first operand). The first source operand
is a YMM register, the second source operand is a YMM register or a 256-bit
memory location, and the destination operand is a YMM register.

   | |  
---- | -----
 SRC1| X7      | X6      | X5                   | X4      | X3                         | X2     | X1      | X0      
 DEST| X7 .. X4| X7 .. X4| X7 .. X4 Figure 4-40.| X7 .. X4| X3 ..X0 VPERMILPS Operation| X3 ..X0| X3 .. X0| X3 .. X0
There is one control byte per destination single-precision element. Each control
byte is aligned with the low 8 bits of the corresponding single-precision destination
element. Each control byte contains a 2-bit select field (see Figure 4-41) that
determines which of the source elements are selected. Source elements are restricted
to lie in the same source 128-bit region as the destination.

Bit

   | |  
---- | -----
 255 ignored| 226 Control Field 7| 225 224 sel Figure 4-41.| 63 ignored VPERMILPS Shuffle Control| 34 Control Field 2| 33 32 sel| 31 ignored Control Field 1| 1 sel| 0
(immediate control version) Permute single-precision floating-point values in
the first source operand (second operand) using four 2-bit control fields in
the 8-bit immediate and store results in the destination operand (first operand).
The source operand is a YMM register or 256-bit memory location and the destination
operand is a YMM register. This is similar to a wider version of PSHUFD, just
operating on single-precision floating-point values. Note: For the VEX.128.66.0F3A
04 instruction version, VEX.vvvv is reserved and must be 1111b otherwise instruction
will **``#UD.``** Note: For the VEX.256.66.0F3A 04 instruction version, VEX.vvvv is
reserved and must be 1111b otherwise instruction will **``#UD.``**



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.W = 1.
