## SGDT - Store Global Descriptor Table Register

> Operation

``` slim
IF instruction is SGDT
     IF OperandSize = 16
       THEN
          DEST[0:15] <- GDTR(Limit);
          DEST[16:39] <- GDTR(Base); (* 24 bits of base address stored *)
          DEST[40:47] <- 0;
       ELSE IF (32-bit Operand Size)
          DEST[0:15] <- GDTR(Limit);
          DEST[16:47] <- GDTR(Base); (* Full 32-bit base address stored *)
          FI;
       ELSE (* 64-bit Operand Size *)
          DEST[0:15] <- GDTR(Limit);
          DEST[16:79] <- GDTR(Base); (* Full 64-bit base address stored *)
     FI;
FI;

```

 Opcode* | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description     
 ---  | --- | --- | --- | --- | ---
 0F 01 /0| SGDT m     | M    | Valid      | Valid          | Store GDTR to m.
<aside class="notification">
* See IA-32 Architecture Compatibility section below.
</aside>


### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (w)| NA       | NA       | NA       

### Description
Stores the content of the global descriptor table register (GDTR) in the destination
operand. The destination operand specifies a memory location.

In legacy or compatibility mode, the destination operand is a 6-byte memory
location. If the operand-size attribute is 16 bits, the limit is stored in the
low 2 bytes and the 24-bit base address is stored in bytes 3-5, and byte 6 is
zero-filled. If the operand-size attribute is 32 bits, the 16-bit limit field
of the register is stored in the low 2 bytes of the memory location and the
32-bit base address is stored in the high 4 bytes.

In IA-32e mode, the operand size is fixed at 8+2 bytes. The instruction stores
an 8-byte base and a 2-byte limit.

SGDT is useful only by operating-system software. However, it can be used in
application programs without causing an exception to be generated. See “LGDT/LIDT - Load
Global/Interrupt Descriptor Table Register” in Chapter 3, Intel® 64 and IA-32
Architectures Software Developer's Manual, Volume 2A, for information on loading
the GDTR and IDTR.


### IA-32 Architecture Compatibility
The 16-bit form of the SGDT is compatible with the Intel 286 processor if the
upper 8 bits are not referenced. The Intel 286 processor fills these bits with
1s; the Pentium 4, Intel Xeon, P6 processor family, Pentium, Intel486, and Intel386™
processors fill these bits with 0s.



### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
 **``#UD``**            | If the destination operand is a register.      
                | If the LOCK prefix is used.                    
 **``#GP(0)``**         | If the destination is located in a non-writable
                | segment. If a memory operand effective         
                | address is outside the CS, DS, ES, FS,         
                | or GS segment limit. If the DS, ES,            
                | FS, or GS register is used to access           
                | memory and it contains a NULL segment          
                | selector.                                      
 **``#SS(0)``**         | If a memory operand effective address          
                | is outside the SS segment limit.               
 **``#PF(fault-code)``**| If a page fault occurs.                        
 **``#AC(0)``**         | If alignment checking is enabled and           
                | an unaligned memory reference is made          
                | while the current privilege level is           
                | 3.                                             

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#UD``**| If the destination operand is a register.
    | If the LOCK prefix is used.              
 **``#GP``**| If a memory operand effective address    
    | is outside the CS, DS, ES, FS, or GS     
    | segment limit.                           
 **``#SS``**| If a memory operand effective address    
    | is outside the SS segment limit.         

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#UD``**            | If the destination operand is a register.
                | If the LOCK prefix is used.              
 **``#GP(0)``**         | If a memory operand effective address    
                | is outside the CS, DS, ES, FS, or GS     
                | segment limit.                           
 **``#SS(0)``**         | If a memory operand effective address    
                | is outside the SS segment limit.         
 **``#PF(fault-code)``**| If a page fault occurs.                  
 **``#AC(0)``**         | If alignment checking is enabled and     
                | an unaligned memory reference is made.   

### Compatibility Mode Exceptions
Same exceptions as in protected mode.


### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#SS(0)``**         | If a memory address referencing the        
                | SS segment is in a non-canonical form.     
 **``#UD``**            | If the destination operand is a register.  
                | If the LOCK prefix is used.                
 **``#GP(0)``**         | If the memory address is in a non-canonical
                | form.                                      
 **``#PF(fault-code)``**| If a page fault occurs.                    
 **``#AC(0)``**         | If alignment checking is enabled and       
                | an unaligned memory reference is made      
                | while the current privilege level is       
                | 3.                                         
