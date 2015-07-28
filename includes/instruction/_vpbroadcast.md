## VPBROADCAST - Broadcast Integer Data

> Operation
``` slim

VPBROADCASTB (VEX.128 encoded version)
temp <- SRC[7:0]
FOR j <- 0 TO 15
DEST[7+j\*8: j\*8] <- temp
ENDFOR
DEST[VLMAX-1:128] <- 0
VPBROADCASTB (VEX.256 encoded version)
temp <- SRC[7:0]
FOR j <- 0 TO 31
DEST[7+j\*8: j\*8] <- temp
ENDFOR
VPBROADCASTW (VEX.128 encoded version)
temp <- SRC[15:0]
FOR j <- 0 TO 7
DEST[15+j\*16: j\*16] <- temp
ENDFOR
DEST[VLMAX-1:128] <- 0
VPBROADCASTW (VEX.256 encoded version)
temp <- SRC[15:0]
FOR j <- 0 TO 15
DEST[15+j\*16: j\*16] <- temp
ENDFOR
VPBROADCASTD (128 bit version)
temp <- SRC[31:0]
FOR j <- 0 TO 3
DEST[31+j\*32: j\*32] <- temp
ENDFOR
DEST[VLMAX-1:128] <- 0
VPBROADCASTD (VEX.256 encoded version)
temp <- SRC[31:0]
FOR j <- 0 TO 7
DEST[31+j\*32: j\*32] <- temp
ENDFOR
VPBROADCASTQ (VEX.128 encoded version)
temp <- SRC[63:0]
DEST[63:0] <- temp
DEST[127:64] <- temp
DEST[VLMAX-1:128] <- 0
VPBROADCASTQ (VEX.256 encoded version)
temp <- SRC[63:0]
DEST[63:0] <- temp
DEST[127:64] <- temp
DEST[191:128] <- temp
DEST[255:192] <- temp
VBROADCASTI128
temp <- SRC[127:0]
DEST[127:0] <- temp
DEST[VLMAX-1:128] <- temp

```

 Opcode/Instruction                     | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                            
 ---  | --- | --- | --- | ---
 VEX.128.66.0F38.W0 78 /r VPBROADCASTB  | RM   | V/V            | AVX2              | Broadcast a byte integer in the source 
 xmm1, xmm2/m8                          |      |                |                   | operand to sixteen locations in xmm1.  
 VEX.256.66.0F38.W0 78 /r VPBROADCASTB  | RM   | V/V            | AVX2              | Broadcast a byte integer in the source 
 ymm1, xmm2/m8                          |      |                |                   | operand to thirtytwo locations in ymm1.
 VEX.128.66.0F38.W0 79 /r VPBROADCASTW  | RM   | V/V            | AVX2              | Broadcast a word integer in the source 
 xmm1, xmm2/m16                         |      |                |                   | operand to eight locations in xmm1.    
 VEX.256.66.0F38.W0 79 /r VPBROADCASTW  | RM   | V/V            | AVX2              | Broadcast a word integer in the source 
 ymm1, xmm2/m16                         |      |                |                   | operand to sixteen locations in ymm1.  
 VEX.128.66.0F38.W0 58 /r VPBROADCASTD  | RM   | V/V            | AVX2              | Broadcast a dword integer in the source
 xmm1, xmm2/m32                         |      |                |                   | operand to four locations in xmm1.     
 VEX.256.66.0F38.W0 58 /r VPBROADCASTD  | RM   | V/V            | AVX2              | Broadcast a dword integer in the source
 ymm1, xmm2/m32                         |      |                |                   | operand to eight locations in ymm1.    
 VEX.128.66.0F38.W0 59 /r VPBROADCASTQ  | RM   | V/V            | AVX2              | Broadcast a qword element in mem to    
 xmm1, xmm2/m64                         |      |                |                   | two locations in xmm1.                 
 VEX.256.66.0F38.W0 59 /r VPBROADCASTQ  | RM   | V/V            | AVX2              | Broadcast a qword element in mem to    
 ymm1, xmm2/m64                         |      |                |                   | four locations in ymm1.                
 VEX.256.66.0F38.W0 5A /r VBROADCASTI128| RM   | V/V            | AVX2              | Broadcast 128 bits of integer data in  
 ymm1, m128                             |      |                |                   | mem to low and high 128-bits in ymm1.  

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Load integer data from the source operand (second operand) and broadcast to
all elements of the destination operand (first operand). The destination operand
is a YMM register. The source operand is 8-bit, 16-bit 32-bit, 64-bit memory
location or the low 8-bit, 16-bit 32-bit, 64-bit data in an XMM register. VPBROADCASTB/D/W/Q
also support XMM register as the source operand. VBROADCASTI128: The destination
operand is a YMM register. The source operand is 128-bit memory location. Register
source encodings for VBROADCASTI128 are reserved and will #UD. VPBROADCASTB/W/D/Q
is supported in both 128-bit and 256-bit wide versions.

VBROADCASTI128 is only supported as a 256-bit wide version. Note: In VEX-encoded
versions, VEX.vvvv is reserved and must be 1111b otherwise instructions will
#UD. Attempts to execute any VPBROADCAST\* instruction with VEX.W = 1 will cause
#UD. If VBROADCASTI128 is encoded with VEX.L= 0, an attempt to execute the instruction
encoded with VEX.L= 0 will cause an #UD exception.

   | |  
---- | -----
 m32                                     | X0                
 X0                                      | X0                
 VPBROADCASTD Operation (VEX.256 encoded | Figure 4-33.      
 version)                                |                   
 m32                                     | X0                
 X0                                      | X0                
 VPBROADCASTD Operation (128-bit version)| Figure 4-34. X0 X0
 VPBROADCASTQ Operation                  | Figure 4-35. X0 X0
 VBROADCASTI128 Operation                | Figure 4-36.      


### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 VPBROADCASTB:  | __m256i _mm256_broadcastb_epi8(__m128i     
                | );                                         
 VPBROADCASTW:  | __m256i _mm256_broadcastw_epi16(__m128i    
                | );                                         
 VPBROADCASTD:  | __m256i _mm256_broadcastd_epi32(__m128i    
                | );                                         
 VPBROADCASTQ:  | __m256i _mm256_broadcastq_epi64(__m128i    
                | );                                         
 VPBROADCASTB:  | __m128i _mm_broadcastb_epi8(__m128i        
                | );                                         
 VPBROADCASTW:  | __m128i _mm_broadcastw_epi16(__m128i       
                | );                                         
 VPBROADCASTD:  | __m128i _mm_broadcastd_epi32(__m128i       
                | );                                         
 VPBROADCASTQ:  | __m128i _mm_broadcastq_epi64(__m128i       
                | );                                         
 VBROADCASTI128:| __m256i _mm256_broadcastsi128_si256(__m128i
                | );                                         

### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 #UD| If VEX.W = 1, If VEX.L = 0 for VBROADCASTI128.
