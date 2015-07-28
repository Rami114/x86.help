## PSHUFW - Shuffle Packed Words

> Operation

``` slim
DEST[15:0] <- (SRC >> (ORDER[1:0] \* 16))[15:0];
DEST[31:16] <- (SRC >> (ORDER[3:2] \* 16))[15:0];
DEST[47:32] <- (SRC >> (ORDER[5:4] \* 16))[15:0];
DEST[63:48] <- (SRC >> (ORDER[7:6] \* 16))[15:0];

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 PSHUFW:| __m64 _mm_shuffle_pi16(__m64 a, int
        | n)                                 

```

 Opcode/Instruction                   | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                              
 ---  | --- | --- | --- | ---
 0F 70 /r ib PSHUFW mm1, mm2/m64, imm8| RMI  | Valid      | Valid          | Shuffle the words in mm2/m64 based on    
                                      |      |            |                | the encoding in imm8 and store the result
                                      |      |            |                | in mm1.                                  

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RMI  | ModRM:reg (w)| ModRM:r/m (r)| imm8     | NA       

### Description
Copies words from the source operand (second operand) and inserts them in the
destination operand (first operand) at word locations selected with the order
operand (third operand). This operation is similar to the operation used by
the PSHUFD instruction, which is illustrated in Figure 4-12. For the PSHUFW
instruction, each 2-bit field in the order operand selects the contents of one
word location in the destination operand. The encodings of the order operand
fields select words from the source operand to be copied to the destination
operand.

The source operand can be an MMX technology register or a 64-bit memory location.
The destination operand is an MMX technology register. The order operand is
an 8-bit immediate. Note that this instruction permits a word in the source
operand to be copied to more than one word location in the destination operand.

In 64-bit mode, using a REX prefix in the form of REX.R permits this instruction
to access additional registers (XMM8-XMM15).



### Flags Affected
None.


### Numeric Exceptions
None.


### Other Exceptions
See Table 22-7, “Exception Conditions for SIMD/MMX Instructions with Memory
Reference,” in the Intel® 64 and IA-32 Architectures Software Developer's Manual,
Volume 3A.
