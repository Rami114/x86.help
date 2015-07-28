## VEXTRACTF128  -  Extract Packed Floating-Point Values

> Operation

``` slim
VEXTRACTF128 (memory destination form)
CASE (imm8[0]) OF
  0: DEST[127:0] <- SRC1[127:0]
  1: DEST[127:0] <- SRC1[255:128]
ESAC.
VEXTRACTF128 (register destination form)
CASE (imm8[0]) OF
  0: DEST[127:0] <- SRC1[127:0]
  1: DEST[127:0] <- SRC1[255:128]
ESAC.
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                      | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 VEX.256.66.0F3A.W0 19 /r ib VEXTRACTF128| MR   | V/V           | AVX               | Extract 128 bits of packed floating-point
 xmm1/m128, ymm2, imm8                   |      |               |                   | values from ymm2 and store results in    
                                         |      |               |                   | xmm1/mem.                                

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
Extracts 128-bits of packed floating-point values from the source operand (second
operand) at an 128-bit offset from imm8[0] into the destination operand (first
operand). The destination may be either an XMM register or an 128-bit memory
location. VEX.vvvv is reserved and must be 1111b otherwise instructions will
#UD. The high 7 bits of the immediate are ignored. If VEXTRACTF128 is encoded
with VEX.L= 0, an attempt to execute the instruction encoded with VEX.L= 0 will
cause an #UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 VEXTRACTF128:| __m128 _mm256_extractf128_ps (__m256   
              | a, int offset);                        
 VEXTRACTF128:| __m128d _mm256_extractf128_pd (__m256d 
              | a, int offset);                        
 VEXTRACTF128:| __m128i_mm256_extractf128_si256(__m256i
              | a, int offset);                        

### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L= 0 If VEX.W=1.
