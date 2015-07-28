## MOVDDUP - Move One Double-FP and Duplicate

> Operation

``` slim
IF (Source = m64)
  THEN
     (* Load instruction *)
     xmm1[63:0] = m64;
     xmm1[127:64] = m64;
  ELSE
     (* Move instruction *)
     xmm1[63:0] = xmm2[63:0];
     xmm1[127:64] = xmm2[63:0];
FI;
MOVDDUP (128-bit Legacy SSE version)
DEST[63:0] <- SRC[63:0]
DEST[127:64] <- SRC[63:0]
DEST[VLMAX-1:128] (Unmodified)
VMOVDDUP (VEX.128 encoded version)
DEST[63:0] <- SRC[63:0]
DEST[127:64] <- SRC[63:0]
DEST[VLMAX-1:128] <- 0
VMOVDDUP (VEX.256 encoded version)
DEST[63:0] <- SRC[63:0]
DEST[127:64] <- SRC[63:0]
DEST[191:128] <- SRC[191:128]
DEST[255:192] <- SRC[191:128]

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 MOVDDUP:| __m128d _mm_movedup_pd(__m128d a)  
 MOVDDUP:| __m128d _mm_loaddup_pd(double const
         | * dp)                              

```

 Opcode/Instruction                    | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                   
 ---  | --- | --- | --- | ---
 F2 0F 12 /r MOVDDUP xmm1, xmm2/m64    | RM   | V/V           | SSE3              | Move one double-precision floating-point      
                                       |      |               |                   | value from the lower 64-bit operand           
                                       |      |               |                   | in xmm2/m64 to xmm1 and duplicate.            
 VEX.128.F2.0F.WIG 12 /r VMOVDDUP xmm1,| RM   | V/V           | AVX               | Move double-precision floating-point          
 xmm2/m64                              |      |               |                   | values from xmm2/mem and duplicate into       
                                       |      |               |                   | xmm1.                                         
 VEX.256.F2.0F.WIG 12 /r VMOVDDUP ymm1,| RM   | V/V           | AVX               | Move even index double-precision floatingpoint
 ymm2/m256                             |      |               |                   | values from ymm2/mem and duplicate each       
                                       |      |               |                   | element into ymm1.                            

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
The linear address corresponds to the address of the least-significant byte
of the referenced memory data. When a memory address is indicated, the 8 bytes
of data at memory location m64 are loaded. When the register-register form of
this operation is used, the lower half of the 128-bit source register is duplicated
and copied into the 128-bit destination register. See Figure 3-24.

MOVDDUP xmm1, xmm2/m64

   | |  
---- | -----
 [63:0]| xmm2/m64
### RESULT

   | |  
---- | -----
 xmm1[127:64]| xmm2/m64[63:0][127:64]| xmm1[63:0]| xmm2/m64[63:0]xmm1 [63:0]
OM15997

   | |  
---- | -----
 Figure 3-24.| MOVDDUP - Move One Double-FP and Duplicate
In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15).



### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.vvvv != 1111B.
