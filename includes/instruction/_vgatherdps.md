## VGATHERDPS/VGATHERQPS  -  Gather Packed SP FP values Using Signed Dword/Qword Indices

> Operation

``` slim
DEST <- SRC1;
BASE_ADDR: base register encoded in VSIB addressing;
VINDEX: the vector index register encoded by VSIB addressing;
SCALE: scale factor encoded by SIB:[7:6];
DISP: optional 1, 4 byte displacement;
MASK <- SRC3;
VGATHERDPS (VEX.128 version)
FOR j<- 0 to 3
  i <- j \* 32;
  IF MASK[31+i] THEN
     MASK[i +31:i] <- 0xFFFFFFFF; // extend from most significant bit
  ELSE
     MASK[i +31:i] <- 0;
  FI;
ENDFOR
MASK[VLMAX-1:128] <- 0;
FOR j<- 0 to 3
  i <- j \* 32;
  DATA_ADDR <- BASE_ADDR + (SignExtend(VINDEX[i+31:i])\*SCALE + DISP;
  IF MASK[31+i] THEN
     DEST[i +31:i] <- FETCH_32BITS(DATA_ADDR); // a fault exits the instruction
  FI;
  MASK[i +31:i] <- 0;
ENDFOR
DEST[VLMAX-1:128] <- 0;
(non-masked elements of the mask register have the content of respective element
VGATHERQPS (VEX.128 version)
FOR j<- 0 to 3
  i <- j \* 32;
  IF MASK[31+i] THEN
     MASK[i +31:i] <- 0xFFFFFFFF; // extend from most significant bit
  ELSE
     MASK[i +31:i] <- 0;
  FI;
ENDFOR
MASK[VLMAX-1:128] <- 0;
FOR j<- 0 to 1
  k <- j \* 64;
  i <- j \* 32;
  DATA_ADDR <- BASE_ADDR + (SignExtend(VINDEX1[k+63:k])\*SCALE + DISP;
  IF MASK[31+i] THEN
     DEST[i +31:i] <- FETCH_32BITS(DATA_ADDR); // a fault exits the instruction
  FI;
  MASK[i +31:i] <- 0;
ENDFOR
MASK[127:64] <- 0;
DEST[VLMAX-1:64] <- 0;
(non-masked elements of the mask register have the content of respective element
VGATHERDPS (VEX.256 version)
FOR j<- 0 to 7
  i <- j \* 32;
  IF MASK[31+i] THEN
     MASK[i +31:i] <- 0xFFFFFFFF; // extend from most significant bit
  ELSE
     MASK[i +31:i] <- 0;
  FI;
ENDFOR
FOR j<- 0 to 7
  i <- j \* 32;
  DATA_ADDR <- BASE_ADDR + (SignExtend(VINDEX1[i+31:i])\*SCALE + DISP;
  IF MASK[31+i] THEN
     DEST[i +31:i] <- FETCH_32BITS(DATA_ADDR); // a fault exits the instruction
  FI;
  MASK[i +31:i] <- 0;
ENDFOR
(non-masked elements of the mask register have the content of respective element
VGATHERQPS (VEX.256 version)
FOR j<- 0 to 7
  i <- j \* 32;
  IF MASK[31+i] THEN
     MASK[i +31:i] <- 0xFFFFFFFF; // extend from most significant bit
  ELSE
     MASK[i +31:i] <- 0;
  FI;
ENDFOR
FOR j<- 0 to 3
  k <- j \* 64;
  i <- j \* 32;
  DATA_ADDR <- BASE_ADDR + (SignExtend(VINDEX1[k+63:k])\*SCALE + DISP;
  IF MASK[31+i] THEN
     DEST[i +31:i] <- FETCH_32BITS(DATA_ADDR); // a fault exits the instruction
  FI;
  MASK[i +31:i] <- 0;
ENDFOR
MASK[VLMAX-1:128] <- 0;
DEST[VLMAX-1:128] <- 0;
(non-masked elements of the mask register have the content of respective element

> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 VGATHERDPS:        | __m128 _mm_i32gather_ps (float const    
                    | \* base, __m128i index, const int scale);
 VGATHERDPS:        | __m128 _mm_mask_i32gather_ps (__m128    
                    | src, float const \* base, __m128i index, 
                    | __m128 mask, const int scale);          
 VGATHERDPS:        | __m256 _mm256_i32gather_ps (float const 
                    | \* base, __m256i index, const int scale);
 VGATHERDPS: scale);| __m256 _mm256_mask_i32gather_ps (__m256 
                    | src, float const \* base, __m256i index, 
                    | __m256 mask, const int                  
 VGATHERQPS:        | __m128 _mm_i64gather_ps (float const    
                    | \* base, __m128i index, const int scale);
 VGATHERQPS:        | __m128 _mm_mask_i64gather_ps (__m128    
                    | src, float const \* base, __m128i index, 
                    | __m128 mask, const int scale);          
 VGATHERQPS:        | __m128 _mm256_i64gather_ps (float const 
                    | \* base, __m256i index, const int scale);
 VGATHERQPS: scale);| __m128 _mm256_mask_i64gather_ps (__m128 
                    | src, float const \* base, __m256i index, 
                    | __m128 mask, const int                  

```

 Opcode/Instruction                     | Op/En| 64/32 -bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 VEX.DDS.128.66.0F38.W0 92 /r VGATHERDPS| RMV  | V/V            | AVX2              | Using dword indices specified in vm32x, 
 xmm1, vm32x, xmm2                      |      |                |                   | gather single-precision FP values from  
                                        |      |                |                   | memory conditioned on mask specified    
                                        |      |                |                   | by xmm2. Conditionally gathered elements
                                        |      |                |                   | are merged into xmm1.                   
 VEX.DDS.128.66.0F38.W0 93 /r VGATHERQPS| RMV  | V/V            | AVX2              | Using qword indices specified in vm64x, 
 xmm1, vm64x, xmm2                      |      |                |                   | gather single-precision FP values from  
                                        |      |                |                   | memory conditioned on mask specified    
                                        |      |                |                   | by xmm2. Conditionally gathered elements
                                        |      |                |                   | are merged into xmm1.                   
 VEX.DDS.256.66.0F38.W0 92 /r VGATHERDPS| RMV  | V/V            | AVX2              | Using dword indices specified in vm32y, 
 ymm1, vm32y, ymm2                      |      |                |                   | gather single-precision FP values from  
                                        |      |                |                   | memory conditioned on mask specified    
                                        |      |                |                   | by ymm2. Conditionally gathered elements
                                        |      |                |                   | are merged into ymm1.                   
 VEX.DDS.256.66.0F38.W0 93 /r VGATHERQPS| RMV  | V/V            | AVX2              | Using qword indices specified in vm64y, 
 xmm1, vm64y, xmm2                      |      |                |                   | gather single-precision FP values from  
                                        |      |                |                   | memory conditioned on mask specified    
                                        |      |                |                   | by xmm2. Conditionally gathered elements
                                        |      |                |                   | are merged into xmm1.                   

### Instruction Operand Encoding
 Op/En| Operand 1      | Operand 2                            | Operand 3      | Operand 4
 ---  | --- | --- | --- | ---
 A    | ModRM:reg (r,w)| BaseReg (R): VSIB:base, VectorReg(R):| VEX.vvvv (r, w)| NA       
      |                | VSIB:index                           |                |          

### Description
The instruction conditionally loads up to 4 or 8 single-precision floating-point
values from memory addresses specified by the memory operand (the second operand)
and using dword indices. The memory operand uses the VSIB form of the SIB byte
to specify a general purpose register operand as the common base, a vector register
for an array of indices relative to the base and a constant scale factor. The
mask operand (the third operand) specifies the conditional load operation from
each memory address and the corresponding update of each data element of the
destination operand (the first operand). Conditionality is specified by the
most significant bit of each data element of the mask register. If an element's
mask bit is not set, the corresponding element of the destination register is
left unchanged. The width of data element in the destination register and mask
register are identical. The entire mask register will be set to zero by this
instruction unless the instruction causes an exception. Using qword indices,
the instruction conditionally loads up to 2 or 4 single-precision floating-point
values from the VSIB addressing memory operand, and updates the lower half of
the destination register. The upper 128 or 256 bits of the destination register
are zero'ed with qword indices. This instruction can be suspended by an exception
if at least one element is already gathered (i.e., if the exception

   | |  
---- | -----
 is triggered by an element other than            | When this happens, the destination If 
 the rightmost one with its mask bit              | any traps or interrupts are pending   
 set). register and the mask operand              | from already gathIt may do this to one
 are partially updated; those elements            | or both                               
 that have been gathered are placed into          |                                       
 the destination register and have their          |                                       
 mask bits set to zero. ered elements,            |                                       
 they will be delivered in lieu of the            |                                       
 exception; in this case, EFLAG.RF is             |                                       
 set to one so an instruction breakpoint          |                                       
 is not re-triggered when the instruction         |                                       
 is continued. If the data size and index         |                                       
 size are different, part of the destination      |                                       
 register and part of the mask register           |                                       
 do not correspond to any elements being          |                                       
 gathered. of those registers even if             |                                       
 the instruction triggers an exception,           |                                       
 and even if the instruction triggers             |                                       
 the exception before gathering any elements.     |                                       
 VEX.128 version: For dword indices,              | For                                   
 the instruction will gather four single-precision|                                       
 floating-point values.                           |                                       
qword indices, the instruction will gather two values and zeroes the upper 64
bits of the destination.

   | |  
---- | -----
 VEX.256 version: For dword indices,               | For If any pair of the index, mask,        
 the instruction will gather eight single-precision| or destination registers are the same,     
 floating-point values. qword indices,             | this instruction results a UD fault.       
 the instruction will gather four values           |                                            
 and zeroes the upper 128 bits of the              |                                            
 destination. Note that: •                         |                                            
 •64 memory-ordering model. •                      | Memory ordering with other instructions    
                                                   | follows the IntelThat is, if a fault       
                                                   | is triggered by an element and delivered,  
                                                   | all                                        
 elements closer to the LSB of the destination     | Individual elements closer If a given      
 will be completed (and non-faulting).             | element triggers multiple faults, they     
 to the MSB may or may not be completed.           | are delivered in the Elements may be       
 conventional order. •                             | gathered in any order, but faults must     
                                                   | be delivered in a right-to-left order;     
                                                   | thus, elements to                          
 the left of a faulting one may be gathered        | A given implementation of this This        
 before the fault is delivered. instruction        | instruction does not perform AC checks,    
 is repeatable - given the same input              | and so will never deliver an AC fault.     
 values and architectural state, the               |                                            
 same set of elements to the left of               |                                            
 the faulting one will be gathered. •              |                                            
 •                                                 | This instruction will cause a **``#UD``** if       
                                                   | the address size attribute is 16-bit.      
 •                                                 | This instruction will cause a **``#UD``** if       
                                                   | the memory operand is encoded without      
                                                   | the SIB byte.                              
 •is implementation specific, and some             | This instruction should not be used        
 implementations may use loads larger              | to access memory mapped I/O as the ordering
 than the data element size or load elements       | of the individual loads it does The        
 an indeterminate number of times. •               | scaled index may require more bits to      
                                                   | represent than the address bits used       
                                                   | by the processor (e.g., in 32-             
 bit mode, if the scale is greater than            | In this case, the most significant bits    
 one). bits are ignored.                           | beyond the number of address               


### SIMD Floating-Point Exceptions
None


### Other Exceptions
See Exceptions Type 12
