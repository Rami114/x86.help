## XRSTOR - Restore Processor Extended States

> Operation

``` slim
RFBM <- XCR0 AND EDX:EAX;
COMPMASK <- XCOMP_BV field from XSAVE header;
RSTORMASK <- XSTATE_BV field from XSAVE header;
IF in VMX non-root operation
  THEN VMXNR <- 1;
1.
  even though MXCSR is part of state component 1  -  SSE. The compacted form of XRSTOR does not make this exception.
  ELSE VMXNR <- 0;
FI;
LAXA <- linear address of XSAVE area;
IF COMPMASK[63] = 0
  THEN
     /* Standard form of XRSTOR */
     If RFBM[0] = 1
       THEN
          IF RSTORMASK[0] = 1
             THEN load x87 state from legacy region of XSAVE area;
             ELSE initialize x87 state;
          FI;
     FI;
     If RFBM[1] = 1
       THEN
          IF RSTORMASK[1] = 1
             THEN load XMM registers from legacy region of XSAVE area;
             ELSE set all XMM registers to 0;
          FI;
     FI;
     If RFBM[2] = 1
       THEN
          IF RSTORMASK[2] = 1
             THEN load AVX state from extended region (standard format) of XSAVE area;
             ELSE initialize AVX state;
          FI;
     FI;
     If RFBM[1] = 1 or RFBM[2] = 1
       THEN load MXCSR from legacy region of XSAVE area;
     FI;
FI;
  ELSE
     /* Compacted form of XRSTOR */
     IF CPUID.(EAX=0DH,ECX=1):EAX.XSAVEC[bit 1] = 0
       THEN
          #GP(0);
     FI;
     If RFBM[0] = 1
       THEN
          IF RSTORMASK[0] = 1
             THEN load x87 state from legacy region of XSAVE area;
             ELSE initialize x87 state;
          FI;
     FI;
     If RFBM[1] = 1
       THEN
          IF RSTORMASK[1] = 1
             THEN load SSE state from legacy region of XSAVE area;
             ELSE initialize SSE state;
          FI;
     FI;
     If RFBM[2] = 1
       THEN
          IF RSTORMASK[2] = 1
             THEN load AVX state from extended region (compacted format) of XSAVE area;
             ELSE initialize AVX state;
          FI;
     FI;
FI;
XRSTOR_INFO <- CPL,VMXNR,LAXA,COMPMASK;```

### Flags Affected
None.


> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 XRSTOR:| void _xrstor( void * , unsigned __int64);  
 XRSTOR:| void _xrstor64( void * , unsigned __int64);

```

 Opcode         | Instruction | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                          
 ---  | --- | --- | --- | --- | ---
 0F AE /5       | XRSTOR mem  | M    | Valid      | Valid          | Restore state components specified by
                |             |      |            |                | EDX:EAX from mem.                    
 REX.W+ 0F AE /5| XRSTOR64 mem| M    | Valid      | N.E.           | Restore state components specified by
                |             |      |            |                | EDX:EAX from mem.                    

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (r)| NA       | NA       | NA       

### Description
Performs a full or partial restore of processor state components from the XSAVE
area located at the memory address specified by the source operand. The implicit
EDX:EAX register pair specifies a 64-bit instruction mask. The specific state
components restored correspond to the bits set in the requested-feature bitmap
(RFBM), which is the logical-AND of EDX:EAX and XCR0.

The format of the XSAVE area is detailed in Section 13.4, “XSAVE Area,” of Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1.

Section 13.7, “Operation of XRSTOR,” of Intel® 64 and IA-32 Architectures Software
Developer's Manual, Volume 1 provides a detailed description of the operation
### of the XRSTOR instruction. The following items provide a highlevel outline

 - Execution of XRSTOR may take one of two forms: standard and compacted. Bit 63
of the XCOMP_BV field in the XSAVE header determines which form is used: value
0 specifies the standard form, while value 1 specifies the compacted form.
 - If RFBM[i] = 0, XRSTOR does not update state component i.1
 - If RFBM[i] = 1 and bit i is clear in the XSTATE_BV field in the XSAVE header,
XRSTOR initializes state component i.
 - If RFBM[i] = 1 and XSTATE_BV[i] = 1, XRSTOR loads state component i from the
XSAVE area.
 - The standard form of XRSTOR treats MXCSR (which is part of state component 1
 -  SSE) differently from the XMM registers. If either form attempts to load MXCSR
with an illegal value, a general-protection exception (**``#GP)``** occurs.
 - XRSTOR loads the internal value XRSTOR_INFO, which may be used to optimize a
subsequent execution of XSAVEOPT or XSAVES.
 - Immediately following an execution of XRSTOR, the processor tracks as in-use
(not in initial configuration) any state component i for which RFBM[i] = 1 and
XSTATE_BV[i] = 1; it tracks as modified any state component i for which RFBM[i]
= 0.

Use of a source operand not aligned to 64-byte boundary (for 64-bit and 32-bit
modes) results in a general-protection (**``#GP)``** exception. In 64-bit mode, the
upper 32 bits of RDX and RAX are ignored.



### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory operand effective address                                  
                | is outside the CS, DS, ES, FS, or GS                                   
                | segment limit. If a memory operand is                                  
                | not aligned on a 64-byte boundary, regardless                          
                | of segment. If bit 63 of the XCOMP_BV                                  
                | field of the XSAVE header is 1 and CPUID.(EAX=0DH,ECX=1):EAX.XSAVEC[bit
                | 1] = 0. If the standard form is executed                               
                | and a bit in XCR0 is 0 and the corresponding                           
                | bit in the XSTATE_BV field of the XSAVE                                
                | header is 1. If the standard form is                                   
                | executed and bytes 23:8 of the XSAVE                                   
                | header are not all zero. If the compacted                              
                | form is executed and a bit in XCR0 is                                  
                | 0 and the corresponding bit in the XCOMP_BV                            
                | field of the XSAVE header is 1. If the                                 
                | compacted form is executed and a bit                                   
                | in the XCOMP_BV field in the XSAVE header                              
                | is 0 and the corresponding bit in the                                  
                | XSTATE_BV field is 1. If the compacted                                 
                | form is executed and bytes 63:16 of                                    
                | the XSAVE header are not all zero. If                                  
                | attempting to write any reserved bits                                  
                | of the MXCSR register with 1.                                          
 **``#SS(0)``**         | If a memory operand effective address                                  
                | is outside the SS segment limit.                                       
 **``#PF(fault-code)``**| If a page fault occurs.                                                
 **``#NM``**            | If CR0.TS[bit 3] = 1.                                                  
 **``#UD``**            | If CPUID.01H:ECX.XSAVE[bit 26] = 0.                                    
                | If CR4.OSXSAVE[bit 18] = 0. If any of                                  
                | the LOCK, 66H, F3H or F2H prefixes is                                  
                | used.                                                                  
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
    | a 64-byte boundary, regardless of segment.                       
    | If any part of the operand lies outside                          
    | the effective address space from 0 to                            
    | FFFFH. If bit 63 of the XCOMP_BV field                           
    | of the XSAVE header is 1 and CPUID.(EAX=0DH,ECX=1):EAX.XSAVEC[bit
    | 1] = 0.                                                          
If the standard form is executed and a bit in XCR0 is 0 and the corresponding
bit in the XSTATE_BV field of the XSAVE header is 1. If the standard form is
executed and bytes 23:8 of the XSAVE header are not all zero. If the compacted
form is executed and a bit in XCR0 is 0 and the corresponding bit in the XCOMP_BV
field of the XSAVE header is 1. If the compacted form is executed and a bit
in the XCOMP_BV field in the XSAVE header is 0 and the corresponding bit in
the XSTATE_BV field is 1. If the compacted form is executed and bytes 63:16
of the XSAVE header are not all zero. If attempting to write any reserved bits
of the MXCSR register with 1.

   | |  
---- | -----
 **``#NM``**| If CR0.TS[bit 3] = 1.                
 **``#UD``**| If CPUID.01H:ECX.XSAVE[bit 26] = 0.  
    | If CR4.OSXSAVE[bit 18] = 0. If any of
    | the LOCK, 66H, F3H or F2H prefixes is
    | used.                                

### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory address is in a non-canonical                        
                | form. If a memory operand is not aligned                         
                | on a 64-byte boundary, regardless of                             
                | segment. If bit 63 of the XCOMP_BV field                         
                | of the XSAVE header is 1 and CPUID.(EAX=0DH,ECX=1):EAX.XSAVEC[bit
                | 1] = 0. If the standard form is executed                         
                | and a bit in XCR0 is 0 and the corresponding                     
                | bit in the XSTATE_BV field of the XSAVE                          
                | header is 1. If the standard form is                             
                | executed and bytes 23:8 of the XSAVE                             
                | header are not all zero. If the compacted                        
                | form is executed and a bit in XCR0 is                            
                | 0 and the corresponding bit in the XCOMP_BV                      
                | field of the XSAVE header is 1. If the                           
                | compacted form is executed and a bit                             
                | in the XCOMP_BV field in the XSAVE header                        
                | is 0 and the corresponding bit in the                            
                | XSTATE_BV field is 1. If the compacted                           
                | form is executed and bytes 63:16 of                              
                | the XSAVE header are not all zero. If                            
                | attempting to write any reserved bits                            
                | of the MXCSR register with 1.                                    
 **``#SS(0)``**         | If a memory address referencing the                              
                | SS segment is in a non-canonical form.                           
 **``#PF(fault-code)``**| If a page fault occurs.                                          
 **``#NM``**            | If CR0.TS[bit 3] = 1.                                            
 **``#UD``**            | If CPUID.01H:ECX.XSAVE[bit 26] = 0.                              
                | If CR4.OSXSAVE[bit 18] = 0. If any of                            
                | the LOCK, 66H, F3H or F2H prefixes is                            
                | used.                                                            
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
