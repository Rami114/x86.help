## PMOVSX  -  Packed Move with Sign Extend

> Operation
``` slim

PMOVSXBW
  DEST[15:0] <- SignExtend(SRC[7:0]);
  DEST[31:16] <- SignExtend(SRC[15:8]);
  DEST[47:32] <- SignExtend(SRC[23:16]);
  DEST[63:48] <- SignExtend(SRC[31:24]);
  DEST[79:64] <- SignExtend(SRC[39:32]);
  DEST[95:80] <- SignExtend(SRC[47:40]);
  DEST[111:96] <- SignExtend(SRC[55:48]);
  DEST[127:112] <- SignExtend(SRC[63:56]);
PMOVSXBD
  DEST[31:0] <- SignExtend(SRC[7:0]);
  DEST[63:32] <- SignExtend(SRC[15:8]);
  DEST[95:64] <- SignExtend(SRC[23:16]);
  DEST[127:96] <- SignExtend(SRC[31:24]);
PMOVSXBQ
  DEST[63:0] <- SignExtend(SRC[7:0]);
  DEST[127:64] <- SignExtend(SRC[15:8]);
PMOVSXWD
  DEST[31:0] <- SignExtend(SRC[15:0]);
  DEST[63:32] <- SignExtend(SRC[31:16]);
  DEST[95:64] <- SignExtend(SRC[47:32]);
  DEST[127:96] <- SignExtend(SRC[63:48]);
PMOVSXWQ
  DEST[63:0] <- SignExtend(SRC[15:0]);
  DEST[127:64] <- SignExtend(SRC[31:16]);
PMOVSXDQ
  DEST[63:0] <- SignExtend(SRC[31:0]);
  DEST[127:64] <- SignExtend(SRC[63:32]);
VPMOVSXBW (VEX.128 encoded version)
Packed_Sign_Extend_BYTE_to_WORD()
DEST[VLMAX-1:128] <- 0
VPMOVSXBD (VEX.128 encoded version)
Packed_Sign_Extend_BYTE_to_DWORD()
DEST[VLMAX-1:128] <- 0
VPMOVSXBQ (VEX.128 encoded version)
Packed_Sign_Extend_BYTE_to_QWORD()
DEST[VLMAX-1:128] <- 0
VPMOVSXWD (VEX.128 encoded version)
Packed_Sign_Extend_WORD_to_DWORD()
DEST[VLMAX-1:128] <- 0
VPMOVSXWQ (VEX.128 encoded version)
Packed_Sign_Extend_WORD_to_QWORD()
DEST[VLMAX-1:128] <- 0
VPMOVSXDQ (VEX.128 encoded version)
Packed_Sign_Extend_DWORD_to_QWORD()
DEST[VLMAX-1:128] <- 0
VPMOVSXBW (VEX.256 encoded version)
Packed_Sign_Extend_BYTE_to_WORD(DEST[127:0], SRC[63:0])
Packed_Sign_Extend_BYTE_to_WORD(DEST[255:128], SRC[127:64])
VPMOVSXBD (VEX.256 encoded version)
Packed_Sign_Extend_BYTE_to_DWORD(DEST[127:0], SRC[31:0])
Packed_Sign_Extend_BYTE_to_DWORD(DEST[255:128], SRC[63:32])
VPMOVSXBQ (VEX.256 encoded version)
Packed_Sign_Extend_BYTE_to_QWORD(DEST[127:0], SRC[15:0])
Packed_Sign_Extend_BYTE_to_QWORD(DEST[255:128], SRC[31:16])
VPMOVSXWD (VEX.256 encoded version)
Packed_Sign_Extend_WORD_to_DWORD(DEST[127:0], SRC[63:0])
Packed_Sign_Extend_WORD_to_DWORD(DEST[255:128], SRC[127:64])
VPMOVSXWQ (VEX.256 encoded version)
Packed_Sign_Extend_WORD_to_QWORD(DEST[127:0], SRC[31:0])
Packed_Sign_Extend_WORD_to_QWORD(DEST[255:128], SRC[63:32])
VPMOVSXDQ (VEX.256 encoded version)
Packed_Sign_Extend_DWORD_to_QWORD(DEST[127:0], SRC[63:0])
Packed_Sign_Extend_DWORD_to_QWORD(DEST[255:128], SRC[127:64])

```

 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 66 0f 38 20 /r PMOVSXBW xmm1, xmm2/m64| RM   | V/V                   | SSE4_1            | Sign extend 8 packed signed 8-bit integers 
                                       |      |                       |                   | in the low 8 bytes of xmm2/m64 to 8        
                                       |      |                       |                   | packed signed 16-bit integers in xmm1.     
 66 0f 38 21 /r PMOVSXBD xmm1, xmm2/m32| RM   | V/V                   | SSE4_1            | Sign extend 4 packed signed 8-bit integers 
                                       |      |                       |                   | in the low 4 bytes of xmm2/m32 to 4        
                                       |      |                       |                   | packed signed 32-bit integers in xmm1.     
 66 0f 38 22 /r PMOVSXBQ xmm1, xmm2/m16| RM   | V/V                   | SSE4_1            | Sign extend 2 packed signed 8-bit integers 
                                       |      |                       |                   | in the low 2 bytes of xmm2/m16 to 2        
                                       |      |                       |                   | packed signed 64-bit integers in xmm1.     
 66 0f 38 23 /r PMOVSXWD xmm1, xmm2/m64| RM   | V/V                   | SSE4_1            | Sign extend 4 packed signed 16-bit integers
                                       |      |                       |                   | in the low 8 bytes of xmm2/m64 to 4        
                                       |      |                       |                   | packed signed 32-bit integers in xmm1.     
 66 0f 38 24 /r PMOVSXWQ xmm1, xmm2/m32| RM   | V/V                   | SSE4_1            | Sign extend 2 packed signed 16-bit integers
                                       |      |                       |                   | in the low 4 bytes of xmm2/m32 to 2        
                                       |      |                       |                   | packed signed 64-bit integers in xmm1.     
 66 0f 38 25 /r PMOVSXDQ xmm1, xmm2/m64| RM   | V/V                   | SSE4_1            | Sign extend 2 packed signed 32-bit integers
                                       |      |                       |                   | in the low 8 bytes of xmm2/m64 to 2        
                                       |      |                       |                   | packed signed 64-bit integers in xmm1.     
 VEX.128.66.0F38.WIG 20 /r VPMOVSXBW   | RM   | V/V                   | AVX               | Sign extend 8 packed 8-bit integers        
 xmm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 8        
                                       |      |                       |                   | packed 16-bit integers in xmm1.            
 VEX.128.66.0F38.WIG 21 /r VPMOVSXBD   | RM   | V/V                   | AVX               | Sign extend 4 packed 8-bit integers        
 xmm1, xmm2/m32                        |      |                       |                   | in the low 4 bytes of xmm2/m32 to 4        
                                       |      |                       |                   | packed 32-bit integers in xmm1.            
 VEX.128.66.0F38.WIG 22 /r VPMOVSXBQ   | RM   | V/V                   | AVX               | Sign extend 2 packed 8-bit integers        
 xmm1, xmm2/m16                        |      |                       |                   | in the low 2 bytes of xmm2/m16 to 2        
                                       |      |                       |                   | packed 64-bit integers in xmm1.            
 VEX.128.66.0F38.WIG 23 /r VPMOVSXWD   | RM   | V/V                   | AVX               | Sign extend 4 packed 16-bit integers       
 xmm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 4        
                                       |      |                       |                   | packed 32-bit integers in xmm1.            
 VEX.128.66.0F38.WIG 24 /r VPMOVSXWQ   | RM   | V/V                   | AVX               | Sign extend 2 packed 16-bit integers       
 xmm1, xmm2/m32                        |      |                       |                   | in the low 4 bytes of xmm2/m32 to 2        
                                       |      |                       |                   | packed 64-bit integers in xmm1.            
 VEX.128.66.0F38.WIG 25 /r VPMOVSXDQ   | RM   | V/V                   | AVX               | Sign extend 2 packed 32-bit integers       
 xmm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 2        
                                       |      |                       |                   | packed 64-bit integers in xmm1.            
 VEX.256.66.0F38.WIG 20 /r VPMOVSXBW   | RM   | V/V                   | AVX2              | Sign extend 16 packed 8-bit integers       
 ymm1, xmm2/m128                       |      |                       |                   | in xmm2/m128 to 16 packed 16-bit integers  
                                       |      |                       |                   | in ymm1.                                   
 VEX.256.66.0F38.WIG 21 /r VPMOVSXBD   | RM   | V/V                   | AVX2              | Sign extend 8 packed 8-bit integers        
 ymm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 8        
                                       |      |                       |                   | packed 32-bit integers in ymm1.            
 VEX.256.66.0F38.WIG 22 /r VPMOVSXBQ   | RM   | V/V                   | AVX2              | Sign extend 4 packed 8-bit integers        
 ymm1, xmm2/m32                        |      |                       |                   | in the low 4 bytes of xmm2/m32 to 4        
                                       |      |                       |                   | packed 64-bit integers in ymm1.            
 VEX.256.66.0F38.WIG 23 /r VPMOVSXWD   | RM   | V/V                   | AVX2              | Sign extend 8 packed 16-bit integers       
 ymm1, xmm2/m128                       |      |                       |                   | in the low 16 bytes of xmm2/m128 to        
                                       |      |                       |                   | 8 packed 32bit integers in ymm1.           
 Opcode/Instruction                    | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 VEX.256.66.0F38.WIG 24 /r VPMOVSXWQ   | RM   | V/V                   | AVX2              | Sign extend 4 packed 16-bit integers       
 ymm1, xmm2/m64                        |      |                       |                   | in the low 8 bytes of xmm2/m64 to 4        
                                       |      |                       |                   | packed 64-bit integers in ymm1.            
 VEX.256.66.0F38.WIG 25 /r VPMOVSXDQ   | RM   | V/V                   | AVX2              | Sign extend 4 packed 32-bit integers       
 ymm1, xmm2/m128                       |      |                       |                   | in the low 16 bytes of xmm2/m128 to        
                                       |      |                       |                   | 4 packed 64bit integers in ymm1.           

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Sign-extend the low byte/word/dword values in each word/dword/qword element
of the source operand (second operand) to word/dword/qword integers and stored
as packed data in the destination operand (first operand). 128-bit Legacy SSE
version: Bits (VLMAX-1:128) of the corresponding YMM destination register remain
unchanged. VEX.128 encoded version: Bits (VLMAX-1:128) of the destination YMM
register are zeroed. VEX.256 encoded version: The destination register is YMM
Register. Note: VEX.vvvv is reserved and must be 1111b, VEX.L must be 0, otherwise
the instruction will #UD.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)PMOVSXBW:| __m128i _mm_ cvtepi8_epi16 ( __m128i   
             | a);                                    
 VPMOVSXBW:  | __m256i _mm256_cvtepi8_epi16 ( __m128i 
             | a);                                    
 (V)PMOVSXBD:| __m128i _mm_ cvtepi8_epi32 ( __m128i   
             | a);                                    
 VPMOVSXBD:  | __m256i _mm256_cvtepi8_epi32 ( __m128i 
             | a);                                    
 (V)PMOVSXBQ:| __m128i _mm_ cvtepi8_epi64 ( __m128i   
             | a);                                    
 VPMOVSXBQ:  | __m256i _mm256_cvtepi8_epi64 ( __m128i 
             | a);                                    
 (V)PMOVSXWD:| __m128i _mm_ cvtepi16_epi32 ( __m128i  
             | a);                                    
 VPMOVSXWD:  | __m256i _mm256_cvtepi16_epi32 ( __m128i
             | a);                                    
 (V)PMOVSXWQ:| __m128i _mm_ cvtepi16_epi64 ( __m128i  
             | a);                                    
 VPMOVSXWQ:  | __m256i _mm256_cvtepi16_epi64 ( __m128i
             | a);                                    
 (V)PMOVSXDQ:| __m128i _mm_ cvtepi32_epi64 ( __m128i  
             | a);                                    
 VPMOVSXDQ:  | __m256i _mm256_cvtepi32_epi64 ( __m128i
             | a);                                    

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1. If VEX.vvvv != 1111B.
