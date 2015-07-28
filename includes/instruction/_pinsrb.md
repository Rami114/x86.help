## PINSRB/PINSRD/PINSRQ  -  Insert Byte/Dword/Qword

> Operation
``` slim

CASE OF
  PINSRB: SEL <- COUNT[3:0];
       MASK <- (0FFH << (SEL \* 8));
       TEMP <- (((SRC[7:0] << (SEL \*8)) AND MASK);
  PINSRD: SEL <- COUNT[1:0];
       MASK <- (0FFFFFFFFH << (SEL \* 32));
       TEMP <- (((SRC << (SEL \*32)) AND MASK)
  PINSRQ: SEL <- COUNT[0]
       MASK <- (0FFFFFFFFFFFFFFFFH << (SEL \* 64));
       TEMP <- (((SRC << (SEL \*32)) AND MASK)
ESAC;
     DEST <- ((DEST AND NOT MASK) OR TEMP);
VPINSRB (VEX.128 encoded version)
SEL <- imm8[3:0]
DEST[127:0] <- write_b_element(SEL, SRC2, SRC1)
DEST[VLMAX-1:128] <- 0
VPINSRD (VEX.128 encoded version)
SEL <- imm8[1:0]
DEST[127:0] <- write_d_element(SEL, SRC2, SRC1)
DEST[VLMAX-1:128] <- 0
VPINSRQ (VEX.128 encoded version)
SEL <- imm8[0]
DEST[127:0] <- write_q_element(SEL, SRC2, SRC1)
DEST[VLMAX-1:128] <- 0

```

 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                              
 ---  | --- | --- | --- | ---
 66 0F 3A 20 /r ib PINSRB xmm1, r32/m8, | RMI  | V/V                   | SSE4_1            | Insert a byte integer value from r32/m8  
 imm8                                   |      |                       |                   | into xmm1 at the destination element     
                                        |      |                       |                   | in xmm1 specified by imm8.               
 66 0F 3A 22 /r ib PINSRD xmm1, r/m32,  | RMI  | V/V                   | SSE4_1            | Insert a dword integer value from r/m32  
 imm8                                   |      |                       |                   | into the xmm1 at the destination element 
                                        |      |                       |                   | specified by imm8.                       
 66 REX.W 0F 3A 22 /r ib PINSRQ xmm1,   | RMI  | V/N. E.               | SSE4_1            | Insert a qword integer value from r/m64  
 r/m64, imm8                            |      |                       |                   | into the xmm1 at the destination element 
                                        |      |                       |                   | specified by imm8.                       
 VEX.NDS.128.66.0F3A.W0 20 /r ib VPINSRB| RVMI | V1/V                  | AVX               | Merge a byte integer value from r32/m8   
 xmm1, xmm2, r32/m8, imm8               |      |                       |                   | and rest from xmm2 into xmm1 at the      
                                        |      |                       |                   | byte offset in imm8.                     
 VEX.NDS.128.66.0F3A.W0 22 /r ib VPINSRD| RVMI | V/V                   | AVX               | Insert a dword integer value from r32/m32
 xmm1, xmm2, r/m32, imm8                |      |                       |                   | and rest from xmm2 into xmm1 at the      
                                        |      |                       |                   | dword offset in imm8.                    
 VEX.NDS.128.66.0F3A.W1 22 /r ib VPINSRQ| RVMI | V/I                   | AVX               | Insert a qword integer value from r64/m64
 xmm1, xmm2, r/m64, imm8                |      |                       |                   | and rest from xmm2 into xmm1 at the      
                                        |      |                       |                   | qword offset in imm8.                    
<aside class="notification">
1. In 64-bit mode, VEX.W1 is ignored for VPINSRB (similar to legacy REX.W=1
prefix with PINSRB).
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
Copies a byte/dword/qword from the source operand (second operand) and inserts
it in the destination operand (first operand) at the location specified with
the count operand (third operand). (The other elements in the destination register
are left untouched.) The source operand can be a general-purpose register or
a memory location. (When the source operand is a general-purpose register, PINSRB
copies the low byte of the register.) The destination operand is an XMM register.
The count operand is an 8-bit immediate. When specifying a qword[dword, byte]location
in an an XMM register, the [2, 4] least-significant bit(s) of the count operand
specify the location. In 64-bit mode, using a REX prefix in the form of REX.R
permits this instruction to access additional registers (XMM8-XMM15, R8-15).
Use of REX.W permits the use of 64 bit general purpose registers. 128-bit Legacy
SSE version: Bits (VLMAX-1:128) of the corresponding YMM destination register
remain unchanged. VEX.128 encoded version: Bits (VLMAX-1:128) of the destination
YMM register are zeroed. VEX.L must be 0, otherwise the instruction will #UD.
Attempt to execute VPINSRQ in non-64-bit mode will cause #UD.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 PINSRB:| __m128i _mm_insert_epi8 (__m128i s1, 
        | int s2, const int ndx);              
 PINSRD:| __m128i _mm_insert_epi32 (__m128i s2,
        | int s, const int ndx);               
 PINSRQ:| __m128i _mm_insert_epi64(__m128i s2, 
        | __int64 s, const int ndx);           

### Flags Affected
None.


### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 #UD| If VEX.L = 1. If VPINSRQ in non-64-bit
    | mode with VEX.W=1.                    
