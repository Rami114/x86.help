## WRFSBASE/WRGSBASE - Write FS/GS Segment Base

> Operation

``` slim
FS/GS segment base address <- SRC;

```

 Opcode/Instruction              | Op/En| 64/32bit Mode| CPUID Feature Flag| Description                             
 ---  | --- | --- | --- | ---
 F3 0F AE /2 WRFSBASE r32        | M    | V/I          | FSGSBASE          | Load the FS base address with the 32-bit
                                 |      |              |                   | value in the source register.           
 REX.W + F3 0F AE /2 WRFSBASE r64| M    | V/I          | FSGSBASE          | Load the FS base address with the 64-bit
                                 |      |              |                   | value in the source register.           
 F3 0F AE /3 WRGSBASE r32        | M    | V/I          | FSGSBASE          | Load the GS base address with the 32-bit
                                 |      |              |                   | value in the source register.           
 REX.W + F3 0F AE /3 WRGSBASE r64| M    | V/I          | FSGSBASE          | Load the GS base address with the 64-bit
                                 |      |              |                   | value in the source register.           

### Instruction Operand Encoding
 Op/En| Operand 1    | Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 M    | ModRM:r/m (r)| NA       | NA       | NA       

### Description
Loads the FS or GS segment base address with the general-purpose register indicated
by the modR/M:r/m field.

The source operand may be either a 32-bit or a 64-bit general-purpose register.
The REX.W prefix indicates the operand size is 64 bits. If no REX.W prefix is
used, the operand size is 32 bits; the upper 32 bits of the source register
are ignored and upper 32 bits of the base address (for FS or GS) are cleared.
This instruction is supported only in 64-bit mode.



### Flags Affected
None


### C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 WRFSBASE:| void _writefsbase_u32( unsigned int  
          | );                                   
 WRFSBASE:| _writefsbase_u64( unsigned __int64 );
 WRGSBASE:| void _writegsbase_u32( unsigned int  
          | );                                   
 WRGSBASE:| _writegsbase_u64( unsigned __int64 );

### Protected Mode Exceptions
   | |  
---- | -----
 **``#UD``**| The WRFSBASE and WRGSBASE instructions
    | are not recognized in protected mode. 

### Real-Address Mode Exceptions
   | |  
---- | -----
 **``#UD``**| The WRFSBASE and WRGSBASE instructions  
    | are not recognized in real-address mode.

### Virtual-8086 Mode Exceptions
   | |  
---- | -----
 **``#UD``**| The WRFSBASE and WRGSBASE instructions  
    | are not recognized in virtual-8086 mode.

### Compatibility Mode Exceptions
   | |  
---- | -----
 **``#UD``**| The WRFSBASE and WRGSBASE instructions
    | are not recognized in compatibility   
    | mode.                                 

### 64-Bit Mode Exceptions
   | |  
---- | -----
 **``#UD``**   | If the LOCK prefix is used. If CR4.FSGSBASE[bit
       | 16] = 0. If CPUID.07H.0H:EBX.FSGSBASE[bit      
       | 0] = 0                                         
 **``#GP(0)``**| If the source register contains a non-canonical
       | address.                                       
