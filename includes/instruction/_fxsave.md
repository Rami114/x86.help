## FXSAVE - Save x87 FPU, MMX Technology, and SSE State

> Operation

``` slim
IF 64-Bit Mode
  THEN
     IF REX.W = 1
       THEN
          DEST <- Save64BitPromotedFxsave(x87 FPU, MMX, XMM7-XMM0,
          MXCSR);
       ELSE
          DEST <- Save64BitDefaultFxsave(x87 FPU, MMX, XMM7-XMM0, MXCSR);
     FI;
  ELSE
     DEST <- SaveLegacyFxsave(x87 FPU, MMX, XMM7-XMM0, MXCSR);
FI;

```

 Opcode/Instruction               | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                          
 ---  | --- | --- | --- | ---
 0F AE /0 FXSAVE m512byte         | M    | Valid      | Valid          | Save the x87 FPU, MMX, XMM, and MXCSR
                                  |      |            |                | register state to m512byte.          
 REX.W+ 0F AE /0 FXSAVE64 m512byte| M    | Valid      | N.E.           | Save the x87 FPU, MMX, XMM, and MXCSR
                                  |      |            |                | register state to m512byte.          

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (w)| NA       | NA       | NA       

### Description
Saves the current state of the x87 FPU, MMX technology, XMM, and MXCSR registers
to a 512-byte memory location specified in the destination operand. The content
layout of the 512 byte region depends on whether the processor is operating
in non-64-bit operating modes or 64-bit sub-mode of IA-32e mode.

Bytes 464:511 are available to software use. The processor does not write to
bytes 464:511 of an FXSAVE area.

The operation of FXSAVE in non-64-bit modes is described first.


### Non-64-Bit Mode Operation
Table 3-53 shows the layout of the state information in memory when the processor
is operating in legacy modes.


### Table 3-53. Non-64-bit-Mode Layout of FXSAVE and FXRSTOR Memory Region
   | |  
---- | -----
 15 Rsvd| 14 MXCSR_MASK| 13 FPU CS Reserved Reserved Reserved| 12| 11 Table 3-53.| 10 FPU IP MXCSR| 9 Non-64-bit-Mode Layout of FXSAVE and| 8 XMM0 XMM1 XMM2 XMM3 XMM4 XMM5 XMM6| 7 FOP Rsrvd| 6| 5 Rsvd FPU DS ST0/MM0 ST1/MM1 ST2/MM2  | 4 FTW| 3 FSW| 2 FPU DP| 1 FCW| 0 0 16 32 48 64 80 96 112 128 144 160
        |              | Reserved Reserved Reserved Reserved |   |               |                | FXRSTOR                               | XMM7                                |            |  | ST3/MM3 ST4/MM4 ST5/MM5 ST6/MM6 ST7/MM7|      |      |         |      | 176 192 208 224 240 256 272          
        |              | Reserved                            |   |               |                |                                       |                                     |            |  |                                        |      |      |         |      |                                      

### Memory Region (Contd.)
   | |  
---- | -----
 15| 14| 13| 12| 11| 10| 9| 8 Reserved Reserved Reserved Reserved| 7| 6| 5| 4| 3| 2| 1| 0 288 304 320 336 352 368 384 400 416
   |   |   |   |   |   |  | Reserved Reserved Reserved Reserved  |  |  |  |  |  |  |  | 432 448 464 480 496                  
   |   |   |   |   |   |  | Reserved Reserved Reserved Available |  |  |  |  |  |  |  |                                      
   |   |   |   |   |   |  | Available Available                  |  |  |  |  |  |  |  |                                      
The destination operand contains the first byte of the memory image, and it
must be aligned on a 16-byte boundary. A misaligned destination operand will
result in a general-protection (**``#GP)``** exception being generated (or in some cases,
an alignment check exception [**``#AC]).``**

The FXSAVE instruction is used when an operating system needs to perform a context
switch or when an exception handler needs to save and examine the current state
of the x87 FPU, MMX technology, and/or XMM and MXCSR registers.

The fields in Table 3-53 are defined in Table 3-54.


### Table 3-54. Field Definitions
   | |  
---- | -----
 Field                  | Definition                                           
 FCW                    | x87 FPU Control Word (16 bits). See                  
                        | Figure 8-6 in the Intel® 64 and IA-32                
                        | Architectures Software Developer's Manual,           
                        | Volume 1, for the layout of the x87                  
                        | FPU control word.                                    
 FSW                    | x87 FPU Status Word (16 bits). See Figure            
                        | 8-4 in the Intel® 64 and IA-32 Architectures         
                        | Software Developer's Manual, Volume                  
                        | 1, for the layout of the x87 FPU status              
                        | word.                                                
 Abridged FTW           | x87 FPU Tag Word (8 bits). The tag information       
                        | saved here is abridged, as described                 
                        | in the following paragraphs.                         
 FOP                    | x87 FPU Opcode (16 bits). The lower                  
                        | 11 bits of this field contain the opcode,            
                        | upper 5 bits are reserved. See Figure                
                        | 8-8 in the Intel® 64 and IA-32 Architectures         
                        | Software Developer's Manual, Volume                  
                        | 1, for the layout of the x87 FPU opcode              
                        | field.                                               
 FPU IP                 | x87 FPU Instruction Pointer Offset (32               
                        | bits). The contents of this field differ             
                        | depending on the current addressing                  
                        | mode (32-bit or 16-bit) of the processor             
                        | when the FXSAVE instruction was executed:            
                        | 32-bit mode  -  32-bit IP offset. 16-bit               
                        | mode  -  low 16 bits are IP offset; high               
                        | 16 bits are reserved. See “x87 FPU Instruction       
                        | and Operand (Data) Pointers” in Chapter              
                        | 8 of the Intel® 64 and IA-32 Architectures           
                        | Software Developer's Manual, Volume                  
                        | 1, for a description of the x87 FPU                  
                        | instruction pointer.                                 
 FPU CS                 | x87 FPU Instruction Pointer Selector                 
                        | (16 bits). If CPUID.(EAX=07H,ECX=0H):EBX[bit         
                        | 13] = 1, the processor deprecates the                
                        | FPU CS and FPU DS values, and this field             
                        | is saved as 0000H. (Contd.)                          
 Field                  | Definition                                           
 FPU DP                 | x87 FPU Instruction Operand (Data) Pointer           
                        | Offset (32 bits). The contents of this               
                        | field differ depending on the current                
                        | addressing mode (32-bit or 16-bit) of                
                        | the processor when the FXSAVE instruction            
                        | was executed: 32-bit mode  -  32-bit DP                
                        | offset. 16-bit mode  -  low 16 bits are                
                        | DP offset; high 16 bits are reserved.                
                        | See “x87 FPU Instruction and Operand                 
                        | (Data) Pointers” in Chapter 8 of the                 
                        | Intel® 64 and IA-32 Architectures Software           
                        | Developer's Manual, Volume 1, for a                  
                        | description of the x87 FPU operand pointer.          
 FPU DS                 | x87 FPU Instruction Operand (Data) Pointer           
                        | Selector (16 bits). If CPUID.(EAX=07H,ECX=0H):EBX[bit
                        | 13] = 1, the processor deprecates the                
                        | FPU CS and FPU DS values, and this field             
                        | is saved as 0000H.                                   
 MXCSR                  | MXCSR Register State (32 bits). See                  
                        | Figure 10-3 in the Intel® 64 and IA-32               
                        | Architectures Software Developer's Manual,           
                        | Volume 1, for the layout of the MXCSR                
                        | register. If the OSFXSR bit in control               
                        | register CR4 is not set, the FXSAVE                  
                        | instruction may not save this register.              
                        | This behavior is implementation dependent.           
 MXCSR_MASK             | MXCSR_MASK (32 bits). This mask can                  
                        | be used to adjust values written to                  
                        | the MXCSR register, ensuring that reserved           
                        | bits are set to 0. Set the mask bits                 
                        | and flags in MXCSR to the mode of operation          
                        | desired for SSE and SSE2 SIMD floating-point         
                        | instructions. See “Guidelines for Writing            
                        | to the MXCSR Register” in Chapter 11                 
                        | of the Intel® 64 and IA-32 Architectures             
                        | Software Developer's Manual, Volume                  
                        | 1, for instructions for how to determine             
                        | and use the MXCSR_MASK value.                        
 ST0/MM0 through ST7/MM7| x87 FPU or MMX technology registers.                 
                        | These 80-bit fields contain the x87                  
                        | FPU data registers or the MMX technology             
                        | registers, depending on the state of                 
                        | the processor prior to the execution                 
                        | of the FXSAVE instruction. If the processor          
                        | had been executing x87 FPU instruction               
                        | prior to the FXSAVE instruction, the                 
                        | x87 FPU data registers are saved; if                 
                        | it had been executing MMX instructions               
                        | (or SSE or SSE2 instructions that operated           
                        | on the MMX technology registers), the                
                        | MMX technology registers are saved.                  
                        | When the MMX technology registers are                
                        | saved, the high 16 bits of the field                 
                        | are reserved.                                        
 XMM0 through XMM7      | XMM registers (128 bits per field).                  
                        | If the OSFXSR bit in control register                
                        | CR4 is not set, the FXSAVE instruction               
                        | may not save these registers. This behavior          
                        | is implementation dependent.                         
The FXSAVE instruction saves an abridged version of the x87 FPU tag word in
the FTW field (unlike the FSAVE instruction, which saves the complete tag word).
The tag information is saved in physical register order (R0 through R7), rather
than in top-of-stack (TOS) order. With the FXSAVE instruction, however, only
a single bit (1 for valid or 0 for empty) is saved for each tag. For example,
### assume that the tag word is currently set as follows

   | |  
---- | -----
 R7                                        | R6| R5| R4| R3| R2| R1| R0                                         
 11                                        | xx| xx| xx| 11| 11| 11| 11 Here, 11B indicates empty stack elements
                                           |   |   |   |   |   |   | and “xx” indicates valid (00B), zero       
                                           |   |   |   |   |   |   | (01B), or special (10B). For this example, 
                                           |   |   |   |   |   |   | the FXSAVE instruction saves only the      
                                           |   |   |   |   |   |   | following 8 bits of information:           
 R7                                        | R6| R5| R4| R3| R2| R1| R0                                         
 0 FXSAVE instruction does not check       | 1 | 1 | 1 | 0 | 0 | 0 | 0 Here, a 1 is saved for any valid,        
 for pending unmasked floating-point       |   |   |   |   |   |   | zero, or special tag, and a 0 is saved     
 exceptions. (The FXSAVE operation in      |   |   |   |   |   |   | for any empty tag. The operation of        
 this regard is similar to the operation   |   |   |   |   |   |   | the FXSAVE instruction differs from        
 of the FNSAVE instruction). After the     |   |   |   |   |   |   | that of the FSAVE instruction, the as      
 FXSAVE instruction has saved the state    |   |   |   |   |   |   | follows: •••                               
 of the x87 FPU, MMX technology, XMM,      |   |   |   |   |   |   |                                            
 and MXCSR registers, the processor retains|   |   |   |   |   |   |                                            
 the contents of the registers. Because    |   |   |   |   |   |   |                                            
 of this behavior, the FXSAVE instruction  |   |   |   |   |   |   |                                            
 cannot be used by an application program  |   |   |   |   |   |   |                                            
 to pass a “clean” x87 FPU state to a      |   |   |   |   |   |   |                                            
 procedure, since it retains the current   |   |   |   |   |   |   |                                            
 state. To clean the x87 FPU state, an     |   |   |   |   |   |   |                                            
 application must explicitly execute       |   |   |   |   |   |   |                                            
 an FINIT instruction after an FXSAVE      |   |   |   |   |   |   |                                            
 instruction to reinitialize the x87       |   |   |   |   |   |   |                                            
 FPU state. The format of the memory       |   |   |   |   |   |   |                                            
 image saved with the FXSAVE instruction   |   |   |   |   |   |   |                                            
 is the same regardless of the current     |   |   |   |   |   |   |                                            
 addressing mode (32-bit or 16-bit) and    |   |   |   |   |   |   |                                            
 operating mode (protected, real address,  |   |   |   |   |   |   |                                            
 ---  | --- | --- | --- | --- | --- | --- | ---
 or system management).                    |   |   |   |   |   |   |                                            
This behavior differs from the FSAVE instructions, where the memory image format
is different depending on the addressing mode and operating mode. Because of
the different image formats, the memory image saved with the FXSAVE instruction
cannot be restored correctly with the FRSTOR instruction, and likewise the state
saved with the FSAVE instruction cannot be restored correctly with the FXRSTOR
instruction.

The FSAVE format for FTW can be recreated from the FTW valid bits and the stored
80-bit FP data (assuming the stored data was not the contents of MMX technology
registers) using Table 3-55.


### Table 3-55. Recreating FSAVE Format
   | |  
---- | -----
 Exponent all 1's| Exponent all 0's| Fraction all 0's| J and M bits| FTW valid bit x87 FTW
 0               | 0               | 0               | 0x          | 10                   
 0               | 0               | 0               | 1x          | 00                   
 0               | 0               | 1               | 00          | 10                   
 0               | 0               | 1               | 10          | 00                   
 0               | 1               | 0               | 0x          | 10                   
 0               | 1               | 0               | 1x          | 10                   
 0               | 1               | 1               | 00          | 01                   
 0               | 1               | 1               | 10          | 10                   
 1               | 0               | 0               | 1x          | 10                   
 1               | 0               | 0               | 1x          | 10                   
 1               | 0               | 1               | 00          | 10                   
 1               | 0               | 1               | 10          | 10 11                
The J-bit is defined to be the 1-bit binary integer to the left of the decimal
place in the significand. The M-bit is defined to be the most significant bit
of the fractional portion of the significand (i.e., the bit immediately to the
right of the decimal place).

When the M-bit is the most significant bit of the fractional portion of the
significand, it must be 0 if the fraction is all 0's.


### IA-32e Mode Operation
In compatibility sub-mode of IA-32e mode, legacy SSE registers, XMM0 through
XMM7, are saved according to the legacy FXSAVE map. In 64-bit mode, all of the
SSE registers, XMM0 through XMM15, are saved. Additionally, there are two different
layouts of the FXSAVE map in 64-bit mode, corresponding to FXSAVE64 (which requires
REX.W=1) and FXSAVE (REX.W=0). In the FXSAVE64 map (Table 3-56), the FPU IP
and FPU DP pointers are 64-bit wide. In the FXSAVE map for 64-bit mode (Table
3-57), the FPU IP and FPU DP pointers are 32-bits.


### Table 3-56. Layout of the 64-bit-mode FXSAVE64 Map (requires REX.W = 1)
   | |  
---- | -----
 15 MXCSR_MASK| 14| 13 Reserved Reserved Reserved Reserved| 12 FPU IP| 11| 10 MXCSR Table 3-56.| 9| 8 Layout of the 64-bit-mode FXSAVE64| 7 FOP| 6| 5 Reserved ST0/MM0 ST1/MM1 ST2/MM2 ST3/MM3| 4 FTW FPU DP| 3 FSW| 2| 1 FCW| 0 0 16 32 48 64 80 96 112
              |   | Reserved Reserved                     |          |   |                     |  | Map                                 |      |  | ST4/MM4 ST5/MM5                           |             |      |  |      |                          

### (requires REX.W = 1) (Contd.)
   | |  
---- | -----
 15         | 14           | 13 Reserved Reserved                | 12| 11 Table 3-57.| 10                                       | 9 Layout of the 64-bit-mode FXSAVE Map| 8| 7 XMM0 XMM1 XMM2 XMM3 XMM4 XMM5 XMM6      | 6| 5 ST6/MM6 ST7/MM7                        | 4    | 3    | 2       | 1    | 0 128 144 160 176 192 208 224 240 256
            |              |                                     |   |               |                                          | (REX.W = 0)                           |  | XMM7 XMM8 XMM9 XMM10 XMM11 XMM12 XMM13    |  |                                          |      |      |         |      | 272 288 304 320 336 352 368 384 400  
            |              |                                     |   |               |                                          |                                       |  | XMM14 XMM15 Reserved Reserved Reserved    |  |                                          |      |      |         |      | 416 432 448 464 480 496              
            |              |                                     |   |               |                                          |                                       |  | Available Available Available             |  |                                          |      |      |         |      |                                      
 15 Reserved| 14 MXCSR_MASK| 13 FPU CS Reserved Reserved Reserved| 12| 11            | 10 FPU IP MXCSR Layout of the 64-bit-mode| 9                                     | 8| 7 FOP Reserved XMM0                       | 6| 5 Reserved FPU DS ST0/MM0 ST1/MM1 ST2/MM2| 4 FTW| 3 FSW| 2 FPU DP| 1 FCW| 0 0 16 32 48 64 80 96 112 128 144 160
            |              | Reserved Reserved Reserved Reserved |   |               | FXSAVE Map (REX.W = 0) (Contd.) (Contd.) |                                       |  |                                           |  | ST3/MM3 ST4/MM4 ST5/MM5 ST6/MM6 ST7/MM7  |      |      |         |      |                                      
            |              | Reserved Table 3-57.                |   |               |                                          |                                       |  |                                           |  |                                          |      |      |         |      |                                      
 15         | 14           | 13                                  | 12| 11            | 10                                       | 9                                     | 8| 7 XMM1 XMM2 XMM3 XMM4 XMM5 XMM6 XMM7      | 6| 5                                        | 4    | 3    | 2       | 1    | 0 176 192 208 224 240 256 272 288 304
            |              |                                     |   |               |                                          |                                       |  | XMM8 XMM9 XMM10 XMM11 XMM12 XMM13 XMM14   |  |                                          |      |      |         |      | 320 336 352 368 384 400 416 432 448  
            |              |                                     |   |               |                                          |                                       |  | XMM15 Reserved Reserved Reserved Available|  |                                          |      |      |         |      | 464 480 496                          
            |              |                                     |   |               |                                          |                                       |  | Available Available                       |  |                                          |      |      |         |      |                                      


### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | For an illegal memory operand effective     
                | address in the CS, DS, ES, FS or GS         
                | segments. If a memory operand is not        
                | aligned on a 16-byte boundary, regardless   
                | of segment. (See the description of         
                | the alignment check exception [**``#AC]``**         
                | below.)                                     
 **``#SS(0)``**         | For an illegal address in the SS segment.   
 **``#PF(fault-code)``**| For a page fault.                           
 **``#NM``**            | If CR0.TS[bit 3] = 1. If CR0.EM[bit         
                | 2] = 1.                                     
 **``#UD``**            | If CPUID.01H:EDX.FXSR[bit 24] = 0.          
 **``#UD``**            | If the LOCK prefix is used.                 
 **``#AC``**            | If this exception is disabled a general     
                | protection exception (**``#GP)``** is signaled      
                | if the memory operand is not aligned        
                | on a 16-byte boundary, as described         
                | above. If the alignment check exception     
                | (**``#AC)``** is enabled (and the CPL is 3),        
                | signaling of **``#AC``** is not guaranteed and      
                | may vary with implementation, as follows.   
                | In all implementations where **``#AC``** is         
                | not signaled, a general protection exception
                | is signaled in its place. In addition,      
                | the width of the alignment check may        
                | also vary with implementation. For instance,
                | for a given implementation, an alignment    
                | check exception might be signaled for       
                | a 2-byte misalignment, whereas a general    
                | protection exception might be signaled      
                | for all other misalignments (4-, 8-,        
                | or 16-byte misalignments).                  

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#GP``**| If a memory operand is not aligned on     
    | a 16-byte boundary, regardless of segment.
    | If any part of the operand lies outside   
    | the effective address space from 0 to     
    | FFFFH.                                    
 **``#NM``**| If CR0.TS[bit 3] = 1. If CR0.EM[bit       
    | 2] = 1.                                   
 **``#UD``**| If CPUID.01H:EDX.FXSR[bit 24] = 0. If     
    | the LOCK prefix is used.                  

### Virtual-8086 Mode Exceptions
Same exceptions as in real address mode.

   | |  
---- | -----
 **``#PF(fault-code)``**| For a page fault.              
 **``#AC``**            | For unaligned memory reference.
 **``#UD``**            | If the LOCK prefix is used.    

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If a memory address referencing the         
                | SS segment is in a non-canonical form.      
 **``#GP(0)``**         | If the memory address is in a non-canonical 
                | form. If memory operand is not aligned      
                | on a 16-byte boundary, regardless of        
                | segment.                                    
 **``#PF(fault-code)``**| For a page fault.                           
 **``#NM``**            | If CR0.TS[bit 3] = 1. If CR0.EM[bit         
                | 2] = 1.                                     
 **``#UD``**            | If CPUID.01H:EDX.FXSR[bit 24] = 0. If       
                | the LOCK prefix is used.                    
 **``#AC``**            | If this exception is disabled a general     
                | protection exception (**``#GP)``** is signaled      
                | if the memory operand is not aligned        
                | on a 16-byte boundary, as described         
                | above. If the alignment check exception     
                | (**``#AC)``** is enabled (and the CPL is 3),        
                | signaling of **``#AC``** is not guaranteed and      
                | may vary with implementation, as follows.   
                | In all implementations where **``#AC``** is         
                | not signaled, a general protection exception
                | is signaled in its place. In addition,      
                | the width of the alignment check may        
                | also vary with implementation. For instance,
                | for a given implementation, an alignment    
                | check exception might be signaled for       
                | a 2-byte misalignment, whereas a general    
                | protection exception might be signaled      
                | for all other misalignments (4-, 8-,        
                | or 16-byte misalignments).                  

### Implementation Note
The order in which the processor signals general-protection (**``#GP)``** and page-fault
(**``#PF)``** exceptions when they both occur on an instruction boundary is given in
Table 5-2 in the Intel® 64 and IA-32 Architectures Software Developer's Manual,
Volume 3B. This order vary for FXSAVE for different processor implementations.
