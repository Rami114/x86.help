## FCLEX/FNCLEX - Clear Exceptions

> Operation

``` slim
FPUStatusWord[0:7] <- 0;
FPUStatusWord[15] <- 0;

```

 Opcode\* | Instruction| 64-Bit Mode| Compat/Leg Mode| Description                          
 ---  | --- | --- | --- | ---
 9B DB E2| FCLEX      | Valid      | Valid          | Clear floating-point exception flags 
         |            |            |                | after checking for pending unmasked  
         |            |            |                | floating-point exceptions.           
 DB E2   | FNCLEX\*    | Valid      | Valid          | Clear floating-point exception flags 
         |            |            |                | without checking for pending unmasked
         |            |            |                | floating-point exceptions.           
<aside class="notification">
\* See IA-32 Architecture Compatibility section below.
</aside>


### Description
Clears the floating-point exception flags (PE, UE, OE, ZE, DE, and IE), the
exception summary status flag (ES), the stack fault flag (SF), and the busy
flag (B) in the FPU status word. The FCLEX instruction checks for and handles
any pending unmasked floating-point exceptions before clearing the exception
flags; the FNCLEX instruction does not.

The assembler issues two instructions for the FCLEX instruction (an FWAIT instruction
followed by an FNCLEX instruction), and the processor executes each of these
instructions separately. If an exception is generated for either of these instructions,
the save EIP points to the instruction that caused the exception.


### IA-32 Architecture Compatibility
When operating a Pentium or Intel486 processor in MS-DOS\* compatibility mode,
it is possible (under unusual circumstances) for an FNCLEX instruction to be
interrupted prior to being executed to handle a pending FPU exception. See the
section titled “No-Wait FPU Instructions Can Get FPU Interrupt in Window” in
Appendix D of the Intel®64 and IA-32 Architectures Software Developer's Manual,
Volume 1, for a description of these circumstances. An FNCLEX instruction cannot
be interrupted in this way on a Pentium 4, Intel Xeon, or P6 family processor.

This instruction affects only the x87 FPU floating-point exception flags. It
does not affect the SIMD floating-point exception flags in the MXCRS register.

This instruction's operation is the same in non-64-bit modes and 64-bit mode.



### FPU Flags Affected
The PE, UE, OE, ZE, DE, IE, ES, SF, and B flags in the FPU status word are cleared.
The C0, C1, C2, and C3 flags are undefined.


### Floating-Point Exceptions
None.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#NM``**| CR0.EM[bit 2] or CR0.TS[bit 3] = 1.
 **``#UD``**| If the LOCK prefix is used.        

### Real-Address Mode Exceptions
Same exceptions as in protected mode.


### Virtual-8086 Mode Exceptions
Same exceptions as in protected mode.


### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
Same exceptions as in protected mode.
