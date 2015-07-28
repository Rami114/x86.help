## XSAVEC - Save Processor Extended States with Compaction

> Operation

``` slim
RFBM <- XCR0 AND EDX:EAX;
COMPMASK <- RFBM OR 80000000_00000000H;
IF RFBM[0] = 1 and XINUSE[0] = 1
1.
  have its initial value of 1F80H. In this case, XSAVEC saves SSE state as long as RFBM[1] = 1.
2.
3.
  have its initial value of 1F80H. In this case, XSAVEC sets XSTATE_BV[1] to 1 as long as RFBM[1] = 1.
  THEN store x87 state into legacy region of XSAVE area;
FI;
IF RFBM[1] = 1 and (XINUSE[1] = 1 or MXCSR != 1F80H)
  THEN store SSE state into legacy region of XSAVE area;
FI;
IF RFBM[2] = 1 AND XINUSE[2] = 1
  THEN store AVX state into extended region of XSAVE area;
FI;
XSTATE_BV field in XSAVE header <- XINUSE AND RFBM;
XCOMP_BV field in XSAVE header <- COMPMASK;```

### Flags Affected
None.


> Intel C/C++ Compiler Intrinsic Equivalent

``` slim
   | |  
---- | -----
 XSAVEC:  | void _xsavec( void * , unsigned __int64);  
 XSAVEC64:| void _xsavec64( void * , unsigned __int64);

```

 Opcode         | Instruction | Op/En| 64-Bit Mode| Compat/Leg Mode| Description                               
 ---  | --- | --- | --- | --- | ---
 0F C7 /4       | XSAVEC mem  | M    | Valid      | Valid          | Save state components specified by EDX:EAX
                |             |      |            |                | to mem with compaction.                   
 REX.W+ 0F C7 /4| XSAVEC64 mem| M    | Valid      | N.E.           | Save state components specified by EDX:EAX
                |             |      |            |                | to mem with compaction.                   

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (w)| NA       | NA       | NA       

### Description
Performs a full or partial save of processor state components to the XSAVE area
located at the memory address specified by the destination operand. The implicit
EDX:EAX register pair specifies a 64-bit instruction mask. The specific state
components saved correspond to the bits set in the requested-feature bitmap
(RFBM), which is the logical-AND of EDX:EAX and XCR0.

The format of the XSAVE area is detailed in Section 13.4, “XSAVE Area,” of Intel®
64 and IA-32 Architectures Software Developer's Manual, Volume 1.

Section 13.9, “Operation of XSAVEC,” of Intel® 64 and IA-32 Architectures Software
Developer's Manual, Volume 1 provides a detailed description of the operation
### of the XSAVEC instruction. The following items provide a highlevel outline

 - Execution of XSAVEC is similar to that of XSAVE. XSAVEC differs from XSAVE in
that it uses compaction and that it may use the init optimization.
 - XSAVEC saves state component i if and only if RFBM[i] = 1 and XINUSE[i] = 1.1
(XINUSE is a bitmap by which the processor tracks the status of various state
components. See Section 13.5.4, “Processor Tracking of XSAVE-Managed State.”)
 - XSAVEC does not modify bytes 511:464 of the legacy region of the XSAVE area
(see Section 13.4.1, “Legacy Region of an XSAVE Area”).
 - XSAVEC writes the logical AND of RFBM and XINUSE to the XSTATE_BV field of the
XSAVE header.2,3 (See Section 13.4.2, “XSAVE Header.”) XSAVEC sets bit 63 of
the XCOMP_BV field and sets bits 62:0 of that field to RFBM[62:0]. XSAVEC does
not write to any parts of the XSAVE header other than the XSTATE_BV and XCOMP_BV
fields.
 - XSAVEC always uses the compacted format of the extended region of the XSAVE
area (see Section 13.4.3, “Extended Region of an XSAVE Area”).

Use of a destination operand not aligned to 64-byte boundary (in either 64-bit
or 32-bit modes) results in a general-protection (**``#GP)``** exception. In 64-bit
mode, the upper 32 bits of RDX and RAX are ignored.



### Protected Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory operand effective address        
                | is outside the CS, DS, ES, FS, or GS         
                | segment limit. If a memory operand is        
                | not aligned on a 64-byte boundary, regardless
                | of segment.                                  
 **``#SS(0)``**         | If a memory operand effective address        
                | is outside the SS segment limit.             
 **``#PF(fault-code)``**| If a page fault occurs.                      
 **``#NM``**            | If CR0.TS[bit 3] = 1.                        
 **``#UD``**            | If CPUID.01H:ECX.XSAVE[bit 26] = 0 or        
                | CPUID.(EAX=0DH,ECX=1):EAX.XSAVEC[bit         
                | 1] = 0. If CR4.OSXSAVE[bit 18] = 0.          
                | If any of the LOCK, 66H, F3H or F2H          
                | prefixes is used.                            
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
    | FFFFH.                                    
 **``#NM``**| If CR0.TS[bit 3] = 1.                     
 **``#UD``**| If CPUID.01H:ECX.XSAVE[bit 26] = 0 or     
    | CPUID.(EAX=0DH,ECX=1):EAX.XSAVEC[bit      
    | 1] = 0. If CR4.OSXSAVE[bit 18] = 0.       
    | If any of the LOCK, 66H, F3H or F2H       
    | prefixes is used.                         

### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If the memory address is in a non-canonical 
                | form. If a memory operand is not aligned    
                | on a 64-byte boundary, regardless of        
                | segment.                                    
 **``#SS(0)``**         | If a memory address referencing the         
                | SS segment is in a non-canonical form.      
 **``#PF(fault-code)``**| If a page fault occurs.                     
 **``#NM``**            | If CR0.TS[bit 3] = 1.                       
 **``#UD``**            | If CPUID.01H:ECX.XSAVE[bit 26] = 0 or       
                | CPUID.(EAX=0DH,ECX=1):EAX.XSAVEC[bit        
                | 1] = 0. If CR4.OSXSAVE[bit 18] = 0.         
                | If any of the LOCK, 66H, F3H or F2H         
                | prefixes is used.                           
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
