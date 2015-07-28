
 Opcode/Instruction                     | Op/En| 64/32 bit Mode Support| CPUID Feature Flag| Description                           
 ---  | --- | --- | --- | ---
 66 0F 3A 63 /r imm8 PCMPISTRI xmm1,    | RM   | V/V                   | SSE4_2            | Perform a packed comparison of string 
 xmm2/m128, imm8                        |      |                       |                   | data with implicit lengths, generating
                                        |      |                       |                   | an index, and storing the result in   
                                        |      |                       |                   | ECX.                                  
 VEX.128.66.0F3A.WIG 63 /r ib VPCMPISTRI| RM   | V/V                   | AVX               | Perform a packed comparison of string 
 xmm1, xmm2/m128, imm8                  |      |                       |                   | data with implicit lengths, generating
                                        |      |                       |                   | an index, and storing the result in   
                                        |      |                       |                   | ECX.                                  

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 RM   | ModRM:reg (r)| ModRM:r/m (r)| imm8     | NA       

### Description
The instruction compares data from two strings based on the encoded value in
the Imm8 Control Byte (see Section 4.1, “Imm8 Control Byte Operation for PCMPESTRI
/ PCMPESTRM / PCMPISTRI / PCMPISTRM”), and generates an index stored to ECX.

   | |  
---- | -----
 Each string is represented by a single        | The value is an xmm (or possibly m128     
 value. contains the data elements of          | for the second operand) which Each input  
 the string (byte or word data). valid/invalid | byte/word is augmented with a A byte/word 
 tag. byte/word. The comparison and aggregation| is considered valid only if it has a      
 operations are performed according to         | lower index than the least significant    
 ---  | ---
 the encoded value of Imm8 bit fields          | null (The least significant null byte/word
 (see Section 4.1). The index of the           | is also considered invalid.)              
 first (or last, according to imm8[6])         |                                           
 set bit of IntRes2 is returned in ECX.        |                                           
 If no bits are set in IntRes2, ECX is         |                                           
 set to 16 (8). Note that the Arithmetic       |                                           
 Flags are written in a non-standard           |                                           
 manner in order to supply the most relevant   |                                           
 information:                                  |                                           
CFlag - Reset if IntRes2 is equal to zero, set otherwise ZFlag - Set if any
byte/word of xmm2/mem128 is null, reset otherwise SFlag - Set if any byte/word
of xmm1 is null, reset otherwise OFlag -IntRes2[0]AFlag - Reset PFlag - Reset
<aside class="notification">
In VEX.128 encoded version, VEX.vvvv is reserved and must be 1111b, VEX.L
must be 0, otherwise the instruction will #UD.
</aside>


### Effective Operand Size
 Operating mode/size| Operand1| Operand 2| Result
 ---  | --- | --- | ---
 16 bit             | xmm     | xmm/m128 | ECX   
 32 bit             | xmm     | xmm/m128 | ECX   
 64 bit             | xmm     | xmm/m128 | ECX   
 64 bit + REX.W     | xmm     | xmm/m128 | RCX   

### Intel C/C++ Compiler Intrinsic Equivalent For Returning Index
   | |  
---- | -----
 int| _mm_cmpistri (__m128i a, __m128i b,
    | const int mode);                   

### Intel C/C++ Compiler Intrinsics For Reading EFlag Results
   | |  
---- | -----
 int| _mm_cmpistra (__m128i a, __m128i b,
    | const int mode);                   
 int| _mm_cmpistrc (__m128i a, __m128i b,
    | const int mode);                   
 int| _mm_cmpistro (__m128i a, __m128i b,
    | const int mode);                   
 int| _mm_cmpistrs (__m128i a, __m128i b,
    | const int mode);                   
 int| _mm_cmpistrz (__m128i a, __m128i b,
    | const int mode);                   

### SIMD Floating-Point Exceptions
None.


### Other Exceptions
See Exceptions Type 4; additionally, this instruction does not cause #GP if
the memory operand is not aligned to 16 Byte boundary, and

   | |  
---- | -----
 #UD| If VEX.L = 1. If VEX.vvvv != 1111B.
