## PMOVZX  -  Packed Move with Zero Extend

> Operation

``` slim
PMOVZXBW
  DEST[15:0] <- ZeroExtend(SRC[7:0]);
  DEST[31:16] <- ZeroExtend(SRC[15:8]);
  DEST[47:32] <- ZeroExtend(SRC[23:16]);
  DEST[63:48] <- ZeroExtend(SRC[31:24]);
  DEST[79:64] <- ZeroExtend(SRC[39:32]);
  DEST[95:80] <- ZeroExtend(SRC[47:40]);
  DEST[111:96] <- ZeroExtend(SRC[55:48]);
  DEST[127:112] <- ZeroExtend(SRC[63:56]);
PMOVZXBD
  DEST[31:0] <- ZeroExtend(SRC[7:0]);
  DEST[63:32] <- ZeroExtend(SRC[15:8]);
  DEST[95:64] <- ZeroExtend(SRC[23:16]);
  DEST[127:96] <- ZeroExtend(SRC[31:24]);
PMOVZXQB
  DEST[63:0] <- ZeroExtend(SRC[7:0]);
  DEST[127:64] <- ZeroExtend(SRC[15:8]);
PMOVZXWD
  DEST[31:0] <- ZeroExtend(SRC[15:0]);
  DEST[63:32] <- ZeroExtend(SRC[31:16]);
  DEST[95:64] <- ZeroExtend(SRC[47:32]);
  DEST[127:96] <- ZeroExtend(SRC[63:48]);
PMOVZXWQ
  DEST[63:0] <- ZeroExtend(SRC[15:0]);
  DEST[127:64] <- ZeroExtend(SRC[31:16]);
PMOVZXDQ
  DEST[63:0] <- ZeroExtend(SRC[31:0]);
  DEST[127:64] <- ZeroExtend(SRC[63:32]);
VPMOVZXBW (VEX.128 encoded version)
Packed_Zero_Extend_BYTE_to_WORD()
DEST[VLMAX-1:128] <- 0
VPMOVZXBD (VEX.128 encoded version)
Packed_Zero_Extend_BYTE_to_DWORD()
DEST[VLMAX-1:128] <- 0
VPMOVZXBQ (VEX.128 encoded version)
Packed_Zero_Extend_BYTE_to_QWORD()
DEST[VLMAX-1:128] <- 0
VPMOVZXWD (VEX.128 encoded version)
Packed_Zero_Extend_WORD_to_DWORD()
DEST[VLMAX-1:128] <- 0
VPMOVZXWQ (VEX.128 encoded version)
Packed_Zero_Extend_WORD_to_QWORD()
DEST[VLMAX-1:128] <- 0
VPMOVZXDQ (VEX.128 encoded version)
Packed_Zero_Extend_DWORD_to_QWORD()
DEST[VLMAX-1:128] <- 0
VPMOVZXBW (VEX.256 encoded version)
Packed_Zero_Extend_BYTE_to_WORD(DEST[127:0], SRC[63:0])
Packed_Zero_Extend_BYTE_to_WORD(DEST[255:128], SRC[127:64])
VPMOVZXBD (VEX.256 encoded version)
Packed_Zero_Extend_BYTE_to_DWORD(DEST[127:0], SRC[31:0])
Packed_Zero_Extend_BYTE_to_DWORD(DEST[255:128], SRC[63:32])
VPMOVZXBQ (VEX.256 encoded version)
Packed_Zero_Extend_BYTE_to_QWORD(DEST[127:0], SRC[15:0])
Packed_Zero_Extend_BYTE_to_QWORD(DEST[255:128], SRC[31:16])
VPMOVZXWD (VEX.256 encoded version)
Packed_Zero_Extend_WORD_to_DWORD(DEST[127:0], SRC[63:0])
Packed_Zero_Extend_WORD_to_DWORD(DEST[255:128], SRC[127:64])
VPMOVZXWQ (VEX.256 encoded version)
Packed_Zero_Extend_WORD_to_QWORD(DEST[127:0], SRC[31:0])
Packed_Zero_Extend_WORD_to_QWORD(DEST[255:128], SRC[63:32])
VPMOVZXDQ (VEX.256 encoded version)
Packed_Zero_Extend_DWORD_to_QWORD(DEST[127:0], SRC[63:0])
Packed_Zero_Extend_DWORD_to_QWORD(DEST[255:128], SRC[127:64])```

### Flags Affected
None


> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 (V)PMOVZXBW:| __m128i _mm_ cvtepu8_epi16 ( __m128i   
             | a);                                    
 VPMOVZXBW:  | __m256i _mm256_cvtepu8_epi16 ( __m128i 
             | a);                                    
 (V)PMOVZXBD:| __m128i _mm_ cvtepu8_epi32 ( __m128i   
             | a);                                    
 VPMOVZXBD:  | __m256i _mm256_cvtepu8_epi32 ( __m128i 
             | a);                                    
 (V)PMOVZXBQ:| __m128i _mm_ cvtepu8_epi64 ( __m128i   
             | a);                                    
 VPMOVZXBQ:  | __m256i _mm256_cvtepu8_epi64 ( __m128i 
             | a);                                    
 (V)PMOVZXWD:| __m128i _mm_ cvtepu16_epi32 ( __m128i  
             | a);                                    
 VPMOVZXWD:  | __m256i _mm256_cvtepu16_epi32 ( __m128i
             | a);                                    
 (V)PMOVZXWQ:| __m128i _mm_ cvtepu16_epi64 ( __m128i  
             | a);                                    
 VPMOVZXWQ:  | __m256i _mm256_cvtepu16_epi64 ( __m128i
             | a);                                    
 (V)PMOVZXDQ:| __m128i _mm_ cvtepu32_epi64 ( __m128i  
             | a);                                    
 VPMOVZXDQ:  | __m256i _mm256_cvtepu32_epi64 ( __m128i
             | a);                                    

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                         
 ---  | --- | --- | --- | ---
 66 0f 38 30 /r PMOVZXBW xmm1, xmm2/m64| RM   | V/V                   | SSE4_1            | Zero extend 8 packed 8-bit integers 
                                       |      |                       |                   | in the low 8 bytes of xmm2/m64 to 8 
                                       |      |                       |                   | packed 16-bit integers in xmm1.     
 66 0f 38 31 /r PMOVZXBD xmm1, xmm2/m32| RM   | V/V                   | SSE4_1            | Zero extend 4 packed 8-bit integers 
                                       |      |                       |                   | in the low 4 bytes of xmm2/m32 to 4 
                                       |      |                       |                   | packed 32-bit integers in xmm1.     
 66 0f 38 32 /r PMOVZXBQ xmm1, xmm2/m16| RM   | V/V                   | SSE4_1            | Zero extend 2 packed 8-bit integers 
                                       |      |                       |                   | in the low 2 bytes of xmm2/m16 to 2 
                                       |      |                       |                   | packed 64-bit integers in xmm1.     
 66 0f 38 33 /r PMOVZXWD xmm1, xmm2/m64| RM   | V/V                   | SSE4_1            | Zero extend 4 packed 16-bit integers
                                       |      |                       |                   | in the low 8 bytes of xmm2/m64 to 4 
                                       |      |                       |                   | packed 32-bit integers in xmm1.     
 66 0f 38 34 /r PMOVZXWQ xmm1, xmm2/m32| RM   | V/V                   | SSE4_1            | Zero extend 2 packed 16-bit integers
                                       |      |                       |                   | in the low 4 bytes of xmm2/m32 to 2 
                                       |      |                       |                   | packed 64-bit integers in xmm1.     
 66 0f 38 35 /r PMOVZXDQ xmm1, xmm2/m64| RM   | V/V                   | SSE4_1            | Zero extend 2 packed 32-bit integers
                                       |      |                       |                   | in the low 8 bytes of xmm2/m64 to 2 
                                       |      |                       |                   | packed 64-bit integers in xmm1.     
 VEX.128.66.0F38.WIG 30 /r VPMOVZXBW   | RM   | V/V                   | AVX               | Zero extend 8 packed 8-bit integers 
 xmm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 8 
                                       |      |                       |                   | packed 16-bit integers in xmm1.     
 VEX.128.66.0F38.WIG 31 /r VPMOVZXBD   | RM   | V/V                   | AVX               | Zero extend 4 packed 8-bit integers 
 xmm1, xmm2/m32                        |      |                       |                   | in the low 4 bytes of xmm2/m32 to 4 
                                       |      |                       |                   | packed 32-bit integers in xmm1.     
 VEX.128.66.0F38.WIG 32 /r VPMOVZXBQ   | RM   | V/V                   | AVX               | Zero extend 2 packed 8-bit integers 
 xmm1, xmm2/m16                        |      |                       |                   | in the low 2 bytes of xmm2/m16 to 2 
                                       |      |                       |                   | packed 64-bit integers in xmm1.     
 VEX.128.66.0F38.WIG 33 /r VPMOVZXWD   | RM   | V/V                   | AVX               | Zero extend 4 packed 16-bit integers
 xmm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 4 
                                       |      |                       |                   | packed 32-bit integers in xmm1.     
 VEX.128.66.0F38.WIG 34 /r VPMOVZXWQ   | RM   | V/V                   | AVX               | Zero extend 2 packed 16-bit integers
 xmm1, xmm2/m32                        |      |                       |                   | in the low 4 bytes of xmm2/m32 to 2 
                                       |      |                       |                   | packed 64-bit integers in xmm1.     
 VEX.128.66.0F38.WIG 35 /r VPMOVZXDQ   | RM   | V/V                   | AVX               | Zero extend 2 packed 32-bit integers
 xmm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 2 
                                       |      |                       |                   | packed 64-bit integers in xmm1.     
 VEX.256.66.0F38.WIG 30 /r VPMOVZXBW   | RM   | V/V                   | AVX2              | Zero extend 16 packed 8-bit integers
 ymm1, xmm2/m128                       |      |                       |                   | in the low 16 bytes of xmm2/m128 to 
                                       |      |                       |                   | 16 packed 16-bit integers in ymm1.  
 VEX.256.66.0F38.WIG 31 /r VPMOVZXBD   | RM   | V/V                   | AVX2              | Zero extend 8 packed 8-bit integers 
 ymm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 8 
                                       |      |                       |                   | packed 32-bit integers in ymm1.     
 VEX.256.66.0F38.WIG 32 /r VPMOVZXBQ   | RM   | V/V                   | AVX2              | Zero extend 4 packed 8-bit integers 
 ymm1, xmm2/m32                        |      |                       |                   | in the low 4 bytes of xmm2/m32 to 4 
                                       |      |                       |                   | packed 64-bit integers in ymm1.     
 VEX.256.66.0F38.WIG 33 /r VPMOVZXWD   | RM   | V/V                   | AVX2              | Zero extend 8 packed 16-bit integers
 ymm1, xmm2/m128                       |      |                       |                   | in the low 16 bytes of xmm2/m128 to 
                                       |      |                       |                   | 8 packed 32bit integers in ymm1.    
 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                         
 ---  | --- | --- | --- | ---
 VEX.256.66.0F38.WIG 34 /r VPMOVZXWQ   | RM   | V/V                   | AVX2              | Zero extend 4 packed 16-bit integers
 ymm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 4 
                                       |      |                       |                   | packed 64-bit integers in xmm1.     
 VEX.256.66.0F38.WIG 35 /r VPMOVZXDQ   | RM   | V/V                   | AVX2              | Zero extend 4 packed 32-bit integers
 ymm1, xmm2/m128                       |      |                       |                   | in the low 16 bytes of xmm2/m128 to 
                                       |      |                       |                   | 4 packed 64bit integers in ymm1.    

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Zero-extend the low byte/word/dword values in each word/dword/qword element
of the source operand (second operand) to word/dword/qword integers and stored
as packed data in the destination operand (first operand). 128-bit Legacy SSE
version: Bits (VLMAX-1:128) of the corresponding YMM destination register remain
unchanged. VEX.128 encoded version: Bits (VLMAX-1:128) of the destination YMM
register are zeroed. VEX.256 encoded version: The destination register is YMM
Register.

<aside class="notification">
VEX.vvvv is reserved and must be 1111b, VEX.L must be 0, otherwise the
instruction will **``#UD.``**
</aside>



### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1. If VEX.vvvv != 1111B.
