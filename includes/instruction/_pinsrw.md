## PINSRW - Insert Word

> Operation

``` slim
PINSRW (with 64-bit source operand)
  SEL <- COUNT AND 3H;
     CASE (Determine word position) OF```

###        SEL <- 0
###        SEL <- 1
###        SEL <- 2
###        SEL <- 3
  DEST <- (DEST AND NOT MASK) OR (((SRC << (SEL * 16)) AND MASK);
PINSRW (with 128-bit source operand)
  SEL <- COUNT AND 7H;
     CASE (Determine word position) OF
###        SEL <- 0
###        SEL <- 1
###        SEL <- 2
###        SEL <- 3
###        SEL <- 4
###        SEL <- 5
###        SEL <- 6
###        SEL <- 7
  DEST <- (DEST AND NOT MASK) OR (((SRC << (SEL * 16)) AND MASK);
VPINSRW (VEX.128 encoded version)
SEL <- imm8[2:0]
DEST[127:0] <- write_w_element(SEL, SRC2, SRC1)
DEST[VLMAX-1:128] <- 0

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PINSRW:| __m64 _mm_insert_pi16 (__m64 a, int  
        | d, int n)                            
 PINSRW:| __m128i _mm_insert_epi16 ( __m128i a,
        | int b, int imm)                      

```

 Opcode/Instruction                   | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                               
 ---  | --- | --- | --- | ---
 0F C4 /r ib1 PINSRW mm, r32/m16, imm8| RMI  | V/V                   | SSE               | Insert the low word from r32 or from      
                                      |      |                       |                   | m16 into mm at the word position specified
                                      |      |                       |                   | by imm8.                                  
 66 0F C4 /r ib PINSRW xmm, r32/m16,  | RMI  | V/V                   | SSE2              | Move the low word of r32 or from m16      
 imm8                                 |      |                       |                   | into xmm at the word position specified   
                                      |      |                       |                   | by imm8.                                  
 VEX.NDS.128.66.0F.W0 C4 /r ib VPINSRW| RVMI | V2/V                  | AVX               | Insert a word integer value from r32/m16  
 xmm1, xmm2, r32/m16, imm8            |      |                       |                   | and rest from xmm2 into xmm1 at the       
                                      |      |                       |                   | word offset in imm8.                      
<aside class="notification">
1. See note in Section 2.4, “Instruction Exception Specification” in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 2A
and Section 22.25.3, “Exception Conditions of Legacy SIMD Instructions Operating
on MMX Registers” in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 3A. 2. In 64-bit mode, VEX.W1 is ignored for VPINSRW (similar
to legacy REX.W=1 prefix in PINSRW).
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3    | Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8         | NA       
 RVMI | ModRM:reg (w)| VEX.vvvv (r) | ModRM:r/m (r)| imm8     

### Description
Copies a word from the source operand (second operand) and inserts it in the
destination operand (first operand) at the location specified with the count
operand (third operand). (The other words in the destination register are left
untouched.) The source operand can be a general-purpose register or a 16-bit
memory location. (When the source operand is a general-purpose register, the
low word of the register is copied.) The destination operand can be an MMX technology
register or an XMM register. The count operand is an 8-bit immediate. When specifying
a word location in an MMX technology register, the 2 least-significant bits
of the count operand specify the location; for an XMM register, the 3 least-significant
bits specify the location.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
### to access additional registers (XMM8-XMM15, R8-15). 128-bit Legacy SSE version
Bits (VLMAX-1:128) of the corresponding YMM destination register remain unchanged.
VEX.128 encoded version: Bits (VLMAX-1:128) of the destination YMM register
are zeroed. VEX.L must be 0, otherwise the instruction will **``#UD.``**



### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Exceptions Type 5; additionally

   | |  
---- | -----
 **``#UD``**| If VEX.L = 1. If VPINSRW in non-64-bit
    | mode with VEX.W=1.                    
