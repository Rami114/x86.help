## MOVSHDUP - Move Packed Single-FP High and Duplicate

> Operation

``` slim
MOVSHDUP (128-bit Legacy SSE version)
DEST[31:0] <- SRC[63:32]
DEST[63:32] <- SRC[63:32]
DEST[95:64] <- SRC[127:96]
DEST[127:96] <- SRC[127:96]
DEST[VLMAX-1:128] (Unmodified)
VMOVSHDUP (VEX.128 encoded version)
DEST[31:0] <- SRC[63:32]
DEST[63:32] <- SRC[63:32]
DEST[95:64] <- SRC[127:96]
DEST[127:96] <- SRC[127:96]
DEST[VLMAX-1:128] <- 0
VMOVSHDUP (VEX.256 encoded version)
DEST[31:0] <- SRC[63:32]
DEST[63:32] <- SRC[63:32]
DEST[95:64] <- SRC[127:96]
DEST[127:96] <- SRC[127:96]
DEST[159:128] <- SRC[191:160]
DEST[191:160] <- SRC[191:160]
DEST[223:192] <- SRC[255:224]
DEST[255:224] <- SRC[255:224]

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                   
 ---  | --- | --- | --- | ---
 F3 0F 16 /r MOVSHDUP xmm1, xmm2/m128   | RM   | V/V           | SSE3              | Move two single-precision floating-point      
                                        |      |               |                   | values from the higher 32-bit operand         
                                        |      |               |                   | of each qword in xmm2/m128 to xmm1 and        
                                        |      |               |                   | duplicate each 32-bit operand to the          
                                        |      |               |                   | lower 32-bits of each qword.                  
 VEX.128.F3.0F.WIG 16 /r VMOVSHDUP xmm1,| RM   | V/V           | AVX               | Move odd index single-precision floating-point
 xmm2/m128                              |      |               |                   | values from xmm2/mem and duplicate each       
                                        |      |               |                   | element into xmm1.                            
 VEX.256.F3.0F.WIG 16 /r VMOVSHDUP ymm1,| RM   | V/V           | AVX               | Move odd index single-precision floating-point
 ymm2/m256                              |      |               |                   | values from ymm2/mem and duplicate each       
                                        |      |               |                   | element into ymm1.                            

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (w)| ModRM:r/m (r)| NA       | NA       

### Description
The linear address corresponds to the address of the least-significant byte
of the referenced memory data. When a memory address is indicated, the 16 bytes
of data at memory location m128 are loaded and the single-precision elements
in positions 1 and 3 are duplicated. When the register-register form of this
operation is used, the same operation is performed but with data coming from
the 128-bit source register. See Figure 3-25.

MOVSHDUP xmm1, xmm2/m128

xmm2/

   | |  
---- | -----
 [127:96]xmm2/| [95:64]xmm1[95:64]xmm2/m128[127:96]| [63:32]xmm1[63:32]xmm2/m128[63:32]| [31:0]m128 xmm1[31:0]RESULT: xmm2/xmm1
              |                                    |                                   | m128[63:32]                           
 [127:96]     | [95:64]                            | [63:32]                           | [31:0]                                
OM15998

   | |  
---- | -----
 Figure 3-25.| MOVSHDUP - Move Packed Single-FP High
             | and Duplicate                      
In 64-bit mode, use of the REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15). 128-bit Legacy SSE version: Bits
(VLMAX-1:128) of the corresponding YMM destination register remain unchanged.
VEX.128 encoded version: Bits (VLMAX-1:128) of the destination YMM register
are zeroed. Note: In VEX-encoded versions, VEX.vvvv is reserved and must be
1111b otherwise instructions will #UD.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 (V)MOVSHDUP:| __m128 _mm_movehdup_ps(__m128 a)     
 VMOVSHDUP:  | __m256 _mm256_movehdup_ps (__m256 a);

### Exceptions
General protection exception if not aligned on 16-byte boundary, regardless
of segment.


### Numeric Exceptions
None


### Other Exceptions
See Exceptions Type 2.
