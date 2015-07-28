## EXTRACTPS  -  Extract Packed Single Precision Floating-Point Value

> Operation

``` slim
EXTRACTPS (128-bit Legacy SSE version)
SRC_OFFSET <- IMM8[1:0]
IF ( 64-Bit Mode and DEST is register)
  DEST[31:0] <- (SRC[127:0] » (SRC_OFFET\*32)) AND 0FFFFFFFFh
  DEST[63:32] <- 0
ELSE
  DEST[31:0] <- (SRC[127:0] » (SRC_OFFET\*32)) AND 0FFFFFFFFh
FI
VEXTRACTPS (VEX.128 encoded version)
SRC_OFFSET <- IMM8[1:0]
IF ( 64-Bit Mode and DEST is register)
  DEST[31:0] <- (SRC[127:0] » (SRC_OFFET\*32)) AND 0FFFFFFFFh
  DEST[63:32] <- 0
ELSE
  DEST[31:0] <- (SRC[127:0] » (SRC_OFFET\*32)) AND 0FFFFFFFFh
FI

```

 Opcode/Instruction                     | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 66 0F 3A 17 /r ib EXTRACTPS reg/m32,   | MRI  | V/V           | SSE4_1            | Extract a single-precision floating-point  
 xmm2, imm8                             |      |               |                   | value from xmm2 at the source offset       
                                        |      |               |                   | specified by imm8 and store the result     
                                        |      |               |                   | to reg or m32. The upper 32 bits of        
                                        |      |               |                   | r64 is zeroed if reg is r64.               
 VEX.128.66.0F3A.WIG 17 /r ib VEXTRACTPS| MRI  | V/V           | AVX               | Extract one single-precision floating-point
 r/m32, xmm1, imm8                      |      |               |                   | value from xmm1 at the offset specified    
                                        |      |               |                   | by imm8 and store the result in reg        
                                        |      |               |                   | or m32. Zero extend the results in 64-bit  
                                        |      |               |                   | register if applicable.                    

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 MRI  | ModRM:r/m (w)| ModRM:reg (r)| imm8     | NA       

### Description
Extracts a single-precision floating-point value from the source operand (second
operand) at the 32-bit offset specified from imm8. Immediate bits higher than
the most significant offset for the vector length are ignored. The extracted
single-precision floating-point value is stored in the low 32-bits of the destination
operand In 64-bit mode, destination register operand has default operand size
of 64 bits. The upper 32-bits of the register are filled with zero. REX.W is
ignored. 128-bit Legacy SSE version: When a REX.W prefix is used in 64-bit mode
with a general purpose register (GPR) as a destination operand, the packed single
quantity is zero extended to 64 bits. VEX.128 encoded version: When VEX.128.66.0F3A.W1
17 form is used in 64-bit mode with a general purpose register (GPR) as a destination
operand, the packed single quantity is zero extended to 64 bits. VEX.vvvv is
reserved and must be 1111b otherwise instructions will #UD. The source register
is an XMM register. Imm8[1:0] determine the starting DWORD offset from which
to extract the 32-bit floating-point value. If VEXTRACTPS is encoded with VEX.L=
1, an attempt to execute the instruction encoded with VEX.L= 1 will cause an
#UD exception.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 EXTRACTPS:| _mm_extractmem_ps (float \*dest, __m128
           | a, const int nidx);                   
 EXTRACTPS:| __m128 _mm_extract_ps (__m128 a, const
           | int nidx);                            

### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L= 1.
