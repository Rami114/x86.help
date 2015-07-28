## VPERMILPD  -  Permute Double-Precision Floating-Point Values

> Operation

``` slim
VPERMILPD (256-bit immediate version)
IF (imm8[0] = 0) THEN DEST[63:0]<-SRC1[63:0]
IF (imm8[0] = 1) THEN DEST[63:0]<-SRC1[127:64]
IF (imm8[1] = 0) THEN DEST[127:64]<-SRC1[63:0]
IF (imm8[1] = 1) THEN DEST[127:64]<-SRC1[127:64]
IF (imm8[2] = 0) THEN DEST[191:128]<-SRC1[191:128]
IF (imm8[2] = 1) THEN DEST[191:128]<-SRC1[255:192]
IF (imm8[3] = 0) THEN DEST[255:192]<-SRC1[191:128]
IF (imm8[3] = 1) THEN DEST[255:192]<-SRC1[255:192]
VPERMILPD (128-bit immediate version)
IF (imm8[0] = 0) THEN DEST[63:0]<-SRC1[63:0]
IF (imm8[0] = 1) THEN DEST[63:0]<-SRC1[127:64]
IF (imm8[1] = 0) THEN DEST[127:64]<-SRC1[63:0]
IF (imm8[1] = 1) THEN DEST[127:64]<-SRC1[127:64]
DEST[VLMAX-1:128] <- 0
VPERMILPD (256-bit variable version)
IF (SRC2[1] = 0) THEN DEST[63:0]<-SRC1[63:0]
IF (SRC2[1] = 1) THEN DEST[63:0]<-SRC1[127:64]
IF (SRC2[65] = 0) THEN DEST[127:64]<-SRC1[63:0]
IF (SRC2[65] = 1) THEN DEST[127:64]<-SRC1[127:64]
IF (SRC2[129] = 0) THEN DEST[191:128]<-SRC1[191:128]
IF (SRC2[129] = 1) THEN DEST[191:128]<-SRC1[255:192]
IF (SRC2[193] = 0) THEN DEST[255:192]<-SRC1[191:128]
IF (SRC2[193] = 1) THEN DEST[255:192]<-SRC1[255:192]
VPERMILPD (128-bit variable version)
IF (SRC2[1] = 0) THEN DEST[63:0]<-SRC1[63:0]
IF (SRC2[1] = 1) THEN DEST[63:0]<-SRC1[127:64]
IF (SRC2[65] = 0) THEN DEST[127:64]<-SRC1[63:0]
IF (SRC2[65] = 1) THEN DEST[127:64]<-SRC1[127:64]
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 VPERMILPD:| __m128d _mm_permute_pd (__m128d a, int
           | control)                              
 VPERMILPD:| __m256d _mm256_permute_pd (__m256d a, 
           | int control)                          
 VPERMILPD:| __m128d _mm_permutevar_pd (__m128d a, 
           | __m128i control);                     
 VPERMILPD:| __m256d _mm256_permutevar_pd (__m256d 
           | a, __m256i control);                  

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 VEX.NDS.128.66.0F38.W0 0D /r VPERMILPD| RVM  | V/V                   | AVX               | Permute double-precision floating-point    
 xmm1, xmm2, xmm3/m128                 |      |                       |                   | values in xmm2 using controls from xmm3/mem
                                       |      |                       |                   | and store result in xmm1.                  
 VEX.NDS.256.66.0F38.W0 0D /r VPERMILPD| RVM  | V/V                   | AVX               | Permute double-precision floating-point    
 ymm1, ymm2, ymm3/m256                 |      |                       |                   | values in ymm2 using controls from ymm3/mem
                                       |      |                       |                   | and store result in ymm1.                  
 VEX.128.66.0F3A.W0 05 /r ib VPERMILPD | RMI  | V/V                   | AVX               | Permute double-precision floating-point    
 xmm1, xmm2/m128, imm8                 |      |                       |                   | values in xmm2/mem using controls from     
                                       |      |                       |                   | imm8.                                      
 VEX.256.66.0F3A.W0 05 /r ib VPERMILPD | RMI  | V/V                   | AVX               | Permute double-precision floating-point    
 ymm1, ymm2/m256, imm8                 |      |                       |                   | values in ymm2/mem using controls from     
                                       |      |                       |                   | imm8.                                      

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RVM  | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| NA       
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8         | NA       

### Description
Permute double-precision floating-point values in the first source operand (second
operand) using 8-bit control fields in the low bytes of the second source operand
(third operand) and store results in the destination operand (first operand).
The first source operand is a YMM register, the second source operand is a YMM
register or a 256bit memory location, and the destination operand is a YMM register.

   | |  
---- | -----
 SRC1| X3                 | X2                        | X1    | X0    
 DEST| X2..X3 Figure 4-38.| X2..X3 VPERMILPD operation| X0..X1| X0..X1
There is one control byte per destination double-precision element. Each control
byte is aligned with the low 8 bits of the corresponding double-precision destination
element. Each control byte contains a 1-bit select field (see Figure 4-39) that
determines which of the source elements are selected. Source elements are restricted
to lie in the same source 128-bit region as the destination.

Bit

   | |  
---- | -----
 255| 194 193 sel Control Field 4 Figure 4-39.| 127 VPERMILPD Shuffle Control| 66 ignored Control Field 2| 65 sel| 63| 2 ignored Control Field1| 1 sel
(immediate control version) Permute double-precision floating-point values in
the first source operand (second operand) using two, 1-bit control fields in
the low 2 bits of the 8-bit immediate and store results in the destination operand
(first operand). The source operand is a YMM register or 256-bit memory location
and the destination operand is a YMM register. Note: For the VEX.128.66.0F3A
05 instruction version, VEX.vvvv is reserved and must be 1111b otherwise instruction
will **``#UD.``** Note: For the VEX.256.66.0F3A 05 instruction version, VEX.vvvv is
reserved and must be 1111b otherwise instruction will **``#UD.``**



### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.W = 1
