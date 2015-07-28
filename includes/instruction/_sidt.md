## SIDT - Store Interrupt Descriptor Table Register

> Operation

``` slim
IF instruction is SIDT
  THEN
     IF OperandSize = 16
       THEN
          DEST[0:15] <- IDTR(Limit);
          DEST[16:39] <- IDTR(Base); (\* 24 bits of base address stored; \*)
          DEST[40:47] <- 0;
       ELSE IF (32-bit Operand Size)
          DEST[0:15] <- IDTR(Limit);
          DEST[16:47] <- IDTR(Base); FI; (\* Full 32-bit base address stored \*)
       ELSE (\* 64-bit Operand Size \*)
          DEST[0:15] <- IDTR(Limit);
          DEST[16:79] <- IDTR(Base); (\* Full 64-bit base address stored \*)
     FI;
FI;

```

 Opcode\* | Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description     
 ---  | --- | --- | --- | --- | ---
 0F 01 /1| SIDT m     | M    | Valid      | Valid          | Store IDTR to m.

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (w)| NA       | NA       | NA       

### Description
Stores the content the interrupt descriptor table register (IDTR) in the destination
operand. The destination operand specifies a 6-byte memory location.

In non-64-bit modes, if the operand-size attribute is 32 bits, the 16-bit limit
field of the register is stored in the low 2 bytes of the memory location and
the 32-bit base address is stored in the high 4 bytes. If the operand-size attribute
is 16 bits, the limit is stored in the low 2 bytes and the 24-bit base address
is stored in the third, fourth, and fifth byte, with the sixth byte filled with
0s.

In 64-bit mode, the operand size fixed at 8+2 bytes. The instruction stores
8-byte base and 2-byte limit values.

SIDT is only useful in operating-system software; however, it can be used in
application programs without causing an exception to be generated. See “LGDT/LIDT - Load
Global/Interrupt Descriptor Table Register” in Chapter 3, Intel® 64 and IA-32
Architectures Software Developer's Manual, Volume 2A, for information on loading
the GDTR and IDTR.


### IA-32 Architecture Compatibility
The 16-bit form of SIDT is compatible with the Intel 286 processor if the upper
8 bits are not referenced. The Intel 286 processor fills these bits with 1s;
the Pentium 4, Intel Xeon, P6 processor family, Pentium, Intel486, and Intel386
processors fill these bits with 0s.



### Flags Affected
None.


### Protected Mode Exceptions
   | |  
---- | -----
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
 **``#UD``**            | If the LOCK prefix is used.                    

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#GP``**| If a memory operand effective address
    | is outside the CS, DS, ES, FS, or GS 
    | segment limit.                       
 **``#SS``**| If a memory operand effective address
    | is outside the SS segment limit.     
 **``#UD``**| If the LOCK prefix is used.          

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#GP(0)``**         | If a memory operand effective address 
                | is outside the CS, DS, ES, FS, or GS  
                | segment limit.                        
 **``#SS(0)``**         | If a memory operand effective address 
                | is outside the SS segment limit.      
 **``#PF(fault-code)``**| If a page fault occurs.               
 **``#AC(0)``**         | If alignment checking is enabled and  
                | an unaligned memory reference is made.
 **``#UD``**            | If the LOCK prefix is used.           

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
