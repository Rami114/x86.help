## VBROADCAST - Broadcast Floating-Point Data

> Operation
``` slim

VBROADCASTSS (128 bit version)
temp <- SRC[31:0]
DEST[31:0] <- temp
DEST[63:32] <- temp
DEST[95:64] <- temp
DEST[127:96] <- temp
DEST[VLMAX-1:128] <- 0
VBROADCASTSS (VEX.256 encoded version)
temp <- SRC[31:0]
DEST[31:0] <- temp
DEST[63:32] <- temp
DEST[95:64] <- temp
DEST[127:96] <- temp
DEST[159:128] <- temp
DEST[191:160] <- temp
DEST[223:192] <- temp
DEST[255:224] <- temp
VBROADCASTSD (VEX.256 encoded version)
temp <- SRC[63:0]
DEST[63:0] <- temp
DEST[127:64] <- temp
DEST[191:128] <- temp
DEST[255:192] <- temp
VBROADCASTF128
temp <- SRC[127:0]
DEST[127:0] <- temp
DEST[VLMAX-1:128] <- temp

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                     
 ---  | --- | --- | --- | ---
 VEX.128.66.0F38.W0 18 /r VBROADCASTSS  | RM   | V/V           | AVX               | Broadcast single-precision floating-point       
 xmm1, m32                              |      |               |                   | element in mem to four locations in             
                                        |      |               |                   | xmm1.                                           
 VEX.256.66.0F38.W0 18 /r VBROADCASTSS  | RM   | V/V           | AVX               | Broadcast single-precision floating-point       
 ymm1, m32                              |      |               |                   | element in mem to eight locations in            
                                        |      |               |                   | ymm1.                                           
 VEX.256.66.0F38.W0 19 /r VBROADCASTSD  | RM   | V/V           | AVX               | Broadcast double-precision floating-point       
 ymm1, m64                              |      |               |                   | element in mem to four locations in             
                                        |      |               |                   | ymm1.                                           
 VEX.256.66.0F38.W0 1A /r VBROADCASTF128| RM   | V/V           | AVX               | Broadcast 128 bits of floating-point            
 ymm1, m128                             |      |               |                   | data in mem to low and high 128-bits            
                                        |      |               |                   | in ymm1.                                        
 VEX.128.66.0F38.W0 18/r VBROADCASTSS   | RM   | V/V           | AVX2              | Broadcast the low single-precision floatingpoint
 xmm1, xmm2                             |      |               |                   | element in the source operand to four           
                                        |      |               |                   | locations in xmm1.                              
 VEX.256.66.0F38.W0 18 /r VBROADCASTSS  | RM   | V/V           | AVX2              | Broadcast low single-precision floating-point   
 ymm1, xmm2                             |      |               |                   | element in the source operand to eight          
                                        |      |               |                   | locations in ymm1.                              
 VEX.256.66.0F38.W0 19 /r VBROADCASTSD  | RM   | V/V           | AVX2              | Broadcast low double-precision floating-point   
 ymm1, xmm2                             |      |               |                   | element in the source operand to four           
                                        |      |               |                   | locations in ymm1.                              

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
Load floating point values from the source operand (second operand) and broadcast
to all elements of the destination operand (first operand). VBROADCASTSD and
VBROADCASTF128 are only supported as 256-bit wide versions. VBROADCASTSS is
supported in both 128-bit and 256-bit wide versions. Memory and register source
operand syntax support of 256-bit instructions depend on the processor's enumeration
of the following conditions with respect to CPUID.1:ECX.AVX[bit 28] and CPUID.(EAX=07H,
### ECX=0H):EBX.AVX2[bit 5]

 - If CPUID.1:ECX.AVX = 1 and CPUID.(EAX=07H, ECX=0H):EBX.AVX2 = 0: the destination
operand is a YMM register. The source operand support can be either a 32-bit,
64-bit, or 128-bit memory location. Register source encodings are reserved and
will #UD.
 - If CPUID.1:ECX.AVX = 1 and CPUID.(EAX=07H, ECX=0H):EBX.AVX2 = 1: the destination
operand is a YMM register. The source operand support can be a register or memory
location.

<aside class="notification">
In VEX-encoded versions, VEX.vvvv is reserved and must be 1111b otherwise
instructions will #UD. An attempt to execute VBROADCASTSD or VBROADCASTF128
encoded with VEX.L= 0 will cause an #UD exception. Attempts to execute any VBROADCAST\*
instruction with VEX.W = 1 will cause #UD.
</aside>

   | |  
---- | -----
 m32                                     | X0                
 X0                                      | X0                
 VBROADCASTSS Operation (VEX.256 encoded | Figure 4-27. X0   
 version)                                |                   
 X0                                      | X0                
 VBROADCASTSS Operation (128-bit version)| Figure 4-28.      
 X0                                      | m64               
 X0                                      | X0                
 VBROADCASTSD Operation                  | Figure 4-29. X0 X0
 VBROADCASTF128 Operation                | Figure 4-30.      


### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 VBROADCASTSS:  | __m128 _mm_broadcast_ss(float \*a);     
 VBROADCASTSS:  | __m256 _mm256_broadcast_ss(float \*a);  
 VBROADCASTSD:  | __m256d _mm256_broadcast_sd(double \*a);
 VBROADCASTF128:| __m256 _mm256_broadcast_ps(__m128 \*    
                | a);                                    
 VBROADCASTF128:| __m256d _mm256_broadcast_pd(__m128d    
                | \* a);                                  

### Flags Affected
None.


### Other Exceptions
See Exceptions Type 6; additionally

   | |  
---- | -----
 #UD| If VEX.L = 0 for VBROADCASTSD, If VEX.L
    | = 0 for VBROADCASTF128, If VEX.W = 1.  
