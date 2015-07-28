## INVLPG - Invalidate TLB Entry

> Operation
``` slim

Flush(RelevantTLBEntries);
Continue; (\* Continue execution \*)

```

 Opcode | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                                
 ---  | --- | --- | --- | --- | ---
 0F 01/7| INVLPG m   | M    | Valid      | Valid          | Invalidate TLB Entry for page that contains
        |            |      |            |                | m.                                         
<aside class="notification">
\* See the IA-32 Architecture Compatibility section below.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (r)| NA       | NA       | NA       

### Description
Invalidates (flushes) the translation lookaside buffer (TLB) entry specified
with the source operand. The source operand is a memory address. The processor
determines the page that contains that address and flushes the TLB entry for
that page.

The INVLPG instruction is a privileged instruction. When the processor is running
in protected mode, the CPL must be 0 to execute this instruction.

The INVLPG instruction normally flushes the TLB entry only for the specified
page; however, in some cases, it may flush more entries, even the entire TLB.
The instruction is guaranteed to invalidates only TLB entries associated with
the current PCID. (If PCIDs are disabled  -  CR4.PCIDE = 0  -  the current PCID
is 000H.) The instruction also invalidates any global TLB entries for the specified
page, regardless of PCID.

For more details on operations that flush the TLB, see “MOV - Move to/from Control
Registers” and Section 4.10.4.1, “Operations that Invalidate TLBs and Paging-Structure
Caches,” of the Intel® 64 and IA-32 Architectures Software Developer's Manual,
Volume 3A.

This instruction's operation is the same in all non-64-bit modes. It also operates
the same in 64-bit mode, except if the memory address is in non-canonical form.
In this case, INVLPG is the same as a NOP.


### IA-32 Architecture Compatibility
The INVLPG instruction is implementation dependent, and its function may be
implemented differently on different families of Intel 64 or IA-32 processors.
This instruction is not supported on IA-32 processors earlier than the Intel486
processor.



### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
 #GP(0)| If the current privilege level is not    
       | 0.                                       
 #UD   | Operand is a register. If the LOCK prefix
       | is used.                                 

### Real-Address Mode Exceptions
   | |  
---- | -----
 #UD| Operand is a register. If the LOCK prefix
    | is used.                                 

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 #GP(0)| The INVLPG instruction cannot be executed
       | at the virtual-8086 mode.                

### 64-Bit Mode Exceptions
   | |  
---- | -----
 #GP(0)| If the current privilege level is not    
       | 0.                                       
 #UD   | Operand is a register. If the LOCK prefix
       | is used.                                 
