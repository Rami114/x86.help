## CPUID - CPU Identification

> Operation

``` slim
IA32_BIOS_SIGN_ID MSR <- Update with installed microcode revision number;
CASE (EAX) OF
```

 Opcode| Instruction| Op/En| 64-Bit Mode| Compat/Leg Mode| Description                            
 ---  | --- | --- | --- | --- | ---
 0F A2 | CPUID      | NP   | Valid      | Valid          | Returns processor identification and   
       |            |      |            |                | feature information to the EAX, EBX,   
       |            |      |            |                | ECX, and EDX registers, as determined  
       |            |      |            |                | by input entered in EAX (in some cases,
       |            |      |            |                | ECX as well).                          

### Instruction Operand Encoding
 Op/En| Operand 1| Operand 2| Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 NP   | NA       | NA       | NA       | NA       

### Description
The ID flag (bit 21) in the EFLAGS register indicates support for the CPUID
instruction. If a software procedure can set and clear this flag, the processor
executing the procedure supports the CPUID instruction. This instruction operates
the same in non-64-bit modes and 64-bit mode. CPUID returns processor identification
and feature information in the EAX, EBX, ECX, and EDX registers.1 The instruction's
output is dependent on the contents of the EAX register upon execution (in some
cases, ECX as well). For example, the following pseudocode loads EAX with 00H
and causes CPUID to return a Maximum Return Value and the Vendor Identification
### String in the appropriate registers

MOV EAX, 00H CPUID

Table 3-17 shows information returned, depending on the initial value loaded
into the EAX register. Table 3-18 shows the maximum CPUID input value recognized
for each family of IA-32 processors on which CPUID is implemented.

Two types of information are returned: basic and extended function information.
If a value entered for CPUID.EAX is higher than the maximum input value for
basic or extended function for that processor then the data for the highest
basic information leaf is returned. For example, using the Intel Core i7 processor,
the following is true: CPUID.EAX = 05H (\* Returns MONITOR/MWAIT leaf. \*) CPUID.EAX
= 0AH (\* Returns Architectural Performance Monitoring leaf. \*) CPUID.EAX = 0BH
### (\* Returns Extended Topology Enumeration leaf. \*) CPUID.EAX = 0CH (\* INVALID
Returns the same information as CPUID.EAX = 0BH. \*) CPUID.EAX = 80000008H (\*
### Returns linear/physical address size data. \*) CPUID.EAX = 8000000AH (\* INVALID
Returns same information as CPUID.EAX = 0BH. \*)

If a value entered for CPUID.EAX is less than or equal to the maximum input
value and the leaf is not supported on that processor then 0 is returned in
all the registers. For example, using the Intel Core i7 processor, the following
is true: CPUID.EAX = 07H (\*Returns EAX=EBX=ECX=EDX=0. \*)

When CPUID returns the highest basic leaf information as a result of an invalid
input EAX value, any dependence on input ECX value in the basic leaf is honored.

CPUID can be executed at any privilege level to serialize instruction execution.
Serializing instruction execution guarantees that any modifications to flags,
registers, and memory for previous instructions are completed before the next
instruction is fetched and executed.

### See also

“Serializing Instructions” in Chapter 8, “Multiple-Processor Management,” in
the Intel® 64 and IA-32 Architectures Software Developer's Manual, Volume 3A.

   | |  
---- | -----
 1.| On Intel 64 processors, CPUID clears   
   | the high 32 bits of the RAX/RBX/RCX/RDX
   | registers in all modes.                
“Caching Translation Information” in Chapter 4, “Paging,” in the Intel® 64 and
IA-32 Architectures Software Developer's Manual, Volume 3A.


### Table 3-17. Information Returned by CPUID Instruction
Initial EAX

   | |  
---- | -----
 Value Basic CPUID Information Maximum     | Information Provided about the Processor
 Input Value for Basic CPUID Information   |                                         
 (see Table 3-18) “Genu”“ntel”“ineI”Version|                                         
 Information: Type, Family, Model, and     |                                         
 Stepping ID (see Figure 3-5) Bits 07-00:  |                                         
 Brand Index Bits 15-08: CLFLUSH line      |                                         
 size (Value \* 8 = cache line size in      |                                         
 bytes) Bits 23-16: Maximum number of      |                                         
 addressable IDs for logical processors    |                                         
 in this physical package\*. Bits 31-24:    |                                         
 Initial APIC ID Feature Information       |                                         
 (see Figure 3-6 and Table 3-20) Feature   |                                         
 Information (see Figure 3-7 and Table     |                                         
 3-21)                                     |                                         
<aside class="notification">
\* The nearest power-of-2 integer that is not smaller than EBX[23:16]
is the number of unique initial APIC IDs reserved for addressing different logical
processors in a physical package. This field is only valid if CPUID.1.EDX.HTT[bit
28]= 1.
</aside>

   | |  
---- | -----
 02H| EAX EBX ECX EDX| Cache and TLB Information (see Table         
    |                | 3-22) Cache and TLB Information Cache        
    |                | and TLB Information Cache and TLB Information
 03H| EAX EBX ECX EDX| Reserved. Reserved. Bits 00-31 of 96         
    |                | bit processor serial number. (Available      
    |                | in Pentium III processor only; otherwise,    
    |                | the value in this register is reserved.)     
    |                | Bits 32-63 of 96 bit processor serial        
    |                | number. (Available in Pentium III processor  
    |                | only; otherwise, the value in this register  
    |                | is reserved.)                                
<aside class="notification">
Processor serial number (PSN) is not supported in the Pentium 4 processor
or later. On all models, use the PSN flag (returned using CPUID) to check for
PSN support before accessing the feature.
</aside>

See AP-485, Intel Processor Identification and the CPUID Instruction (Order
Number 241618) for more information on PSN.

CPUID leaves > 3 < 80000000 are visible only when IA32_MISC_ENABLE.BOOT_NT4[bit
22] = 0 (default).

Deterministic Cache Parameters Leaf

   | |  
---- | -----
 04H| NOTES: Leaf 04H output depends on the       
    | initial value in ECX.\*See also: “INPUT      
    | EAX = 4: Returns Deterministic Cache        
    | Parameters for each level on page 3-178.    
 EAX| Bits 04-00: Cache Type Field 0 = Null       
    | - No more caches 1 = Data Cache 2 =         
    | Instruction Cache 3 = Unified Cache         
    | 4-31 = Reserved Information Returned        
    | by CPUID Instruction (Contd.) Initial       
    | EAX Information Provided about the Processor
    | Bits 07-05: Cache Level (starts at 1)       
    | Bit 08: Self Initializing cache level       
    | (does not need SW initialization) Bit       
    | 09: Fully Associative cache Bits 13-10:     
    | Reserved Bits 25-14: Maximum number         
    | of addressable IDs for logical processors   
    | sharing this cache\*\*, \*\*\*Bits 31-26:        
    | Maximum number of addressable IDs for       
    | processor cores in the physical package\*\*,  
    | \*\*\*\*, \*\*\*\*\*                                 
 EBX| Bits 11-00: L = System Coherency Line       
    | Size\*\*Bits 21-12: P = Physical Line         
    | partitions\*\*Bits 31-22: W = Ways of         
    | associativity\*\*                             
 ECX| Bits 31-00: S = Number of Sets\*\*            
 EDX| Bit 0: Write-Back Invalidate/Invalidate     
    | 0 = WBINVD/INVD from threads sharing        
    | this cache acts upon lower level caches     
    | for threads sharing this cache. 1 =         
    | WBINVD/INVD is not guaranteed to act        
    | upon lower level caches of non-originating  
    | threads sharing this cache. Bit 1: Cache    
    | Inclusiveness 0 = Cache is not inclusive    
    | of lower cache levels. 1 = Cache is         
    | inclusive of lower cache levels. Bit        
    | 2: Complex Cache Indexing 0 = Direct        
    | mapped cache. 1 = A complex function        
    | is used to index the cache, potentially     
    | using all address bits. Bits 31-03:         
    | Reserved = 0                                
<aside class="notification">
\* If ECX contains an invalid sub leaf index, EAX/EBX/ECX/EDX return 0.
Invalid sub-leaves of EAX = 04H: ECX = n, n > 3. \*\* Add one to the return value
to get the result. \*\*\*The nearest power-of-2 integer that is not smaller than
(1 + EAX[25:14]) is the number of unique initial APIC IDs reserved for addressing
different logical processors sharing this cache \*\*\*\* The nearest power-of-2
integer that is not smaller than (1 + EAX[31:26]) is the number of unique Core_IDs
reserved for addressing different processor cores in a physical package. Core
ID is a subset of bits of the initial APIC ID. \*\*\*\*\* The returned value is constant
for valid initial values in ECX. Valid ECX values start from 0.
</aside>

MONITOR/MWAIT Leaf

   | |  
---- | -----
 05H| EAX EBX ECX EDX| Bits 15-00: Smallest monitor-line size      
    |                | in bytes (default is processor's monitor    
    |                | granularity) Bits 31-16: Reserved =         
    |                | 0 Bits 15-00: Largest monitor-line size     
    |                | in bytes (default is processor's monitor    
    |                | granularity) Bits 31-16: Reserved =         
    |                | 0 Bit 00: Enumeration of Monitor-Mwait      
    |                | extensions (beyond EAX and EBX registers)   
    |                | supported Bit 01: Supports treating         
    |                | interrupts as break-event for MWAIT,        
    |                | even when interrupts disabled Bits 31       
    |                | - 02: Reserved Information Returned         
    |                | by CPUID Instruction (Contd.) Initial       
    |                | EAX Information Provided about the Processor
    |                | Bits 03 - 00: Number of C0\* sub C-states    
    |                | supported using MWAIT Bits 07 - 04:         
    |                | Number of C1\* sub C-states supported        
    |                | using MWAIT Bits 11 - 08: Number of         
    |                | C2\* sub C-states supported using MWAIT      
    |                | Bits 15 - 12: Number of C3\* sub C-states    
    |                | supported using MWAIT Bits 19 - 16:         
    |                | Number of C4\* sub C-states supported        
    |                | using MWAIT Bits 23 - 20: Number of         
    |                | C5\* sub C-states supported using MWAIT      
    |                | Bits 27 - 24: Number of C6\* sub C-states    
    |                | supported using MWAIT Bits 31 - 28:         
    |                | Number of C7\* sub C-states supported        
    |                | using MWAIT                                 

<aside class="notification">
NOTE::
   | |  
---- | -----
 \*                                                             | The definition of C0 through C7 states    
                                                               | for MWAIT extension are processor-specific
                                                               | C-states, not ACPI Cstates. Thermal       
                                                               | and Power Management Leaf                 
 Bit 00: Digital temperature sensor is                         | EAX EBX                                   
 supported if set Bit 01: Intel Turbo                          |                                           
 Boost Technology Available (see description                   |                                           
 of IA32_MISC_ENABLE[38]). Bit 02: ARAT.                       |                                           
 APIC-Timer-always-running feature is                          |                                           
 supported if set. Bit 03: Reserved Bit                        |                                           
 04: PLN. Power limit notification controls                    |                                           
 are supported if set. Bit 05: ECMD.                           |                                           
 Clock modulation duty cycle extension                         |                                           
 is supported if set. Bit 06: PTM. Package                     |                                           
 thermal management is supported if set.                       |                                           
 Bit 07: HWP. HWP base registers (IA32_PM_ENALBE[bit           |                                           
 0], IA32_HWP_CAPABILITIES, IA32_HWP_REQUEST,                  |                                           
 IA32_HWP_STATUS) are supported if set.                        |                                           
 Bit 08: HWP_Notification. IA32_HWP_INTERRUPT                  |                                           
 MSR is supported if set. Bit 09: HWP_Activity_Window.         |                                           
 IA32_HWP_REQUEST[bits 41:32] is supported                     |                                           
 if set. Bit 10: HWP_Energy_Performance_Preference.            |                                           
 IA32_HWP_REQUEST[bits 31:24] is supported                     |                                           
 if set. Bit 11: HWP_Package_Level_Request.                    |                                           
 IA32_HWP_REQUEST_PKG MSR is supported                         |                                           
 if set. Bit 12: Reserved. Bit 13: HDC.                        |                                           
 HDC base registers IA32_PKG_HDC_CTL,                          |                                           
 IA32_PM_CTL1, IA32_THREAD_STALL MSRs                          |                                           
 are supported if set. Bits 31 - 15:                           |                                           
 Reserved Bits 03 - 00: Number of Interrupt                    |                                           
 Thresholds in Digital Thermal Sensor                          |                                           
 Bits 31 - 04: Reserved                                        |                                           
 Bit 00: Hardware Coordination Feedback                        | ECX                                       
 Capability (Presence of IA32_MPERF and                        |                                           
 IA32_APERF). The capability to provide                        |                                           
 a measure of delivered processor performance                  |                                           
 (since last reset of the counters),                           |                                           
 as a percentage of expected processor                         |                                           
 performance at frequency specified in                         |                                           
 CPUID Brand String Bits 02 - 01: Reserved                     |                                           
 = 0 Bit 03: The processor supports performance-energy         |                                           
 bias preference if CPUID.06H:ECX.SETBH[bit                    |                                           
 3] is set and it also implies the presence                    |                                           
 of a new architectural MSR called IA32_ENERGY_PERF_BIAS       |                                           
 (1B0H) Bits 31 - 04: Reserved = 0                             |                                           
 Reserved = 0                                                  | EDX Structured Extended Feature Flags     
                                                               | Enumeration Leaf (Output depends on       
                                                               | ECX input value) Sub-leaf 0 (Input ECX    
                                                               | = 0). \*                                   
 Bits 31-00: Reports the maximum input                         | EAX                                       
 value for supported leaf 7 sub-leaves.                        |                                           
 Table 3-17.                                                   | Information Returned by CPUID Instruction 
                                                               | (Contd.) Initial EAX Information Provided 
                                                               | about the Processor                       
 Bit 00: FSGSBASE. Supports RDFSBASE/RDGSBASE/WRFSBASE/WRGSBASE| EBX                                       
 if 1. Bit 01: IA32_TSC_ADJUST MSR is                          |                                           
 supported if 1. Bit 02: Reserved Bit                          |                                           
 03: BMI1 Bit 04: HLE Bit 05: AVX2 Bit                         |                                           
 06: Reserved Bit 07: SMEP. Supports                           |                                           
 Supervisor-Mode Execution Prevention                          |                                           
 if 1. Bit 08: BMI2 Bit 09: Supports                           |                                           
 Enhanced REP MOVSB/STOSB if 1. Bit 10:                        |                                           
 INVPCID. If 1, supports INVPCID instruction                   |                                           
 for system software that manages process-context              |                                           
 identifiers. Bit 11: RTM Bit 12: Supports                     |                                           
 Platform Quality of Service Monitoring                        |                                           
 (PQM) capability if 1. Bit 13: Deprecates                     |                                           
 FPU CS and FPU DS values if 1. Bit 14:                        |                                           
 Reserved. Bit 15: Supports Platform                           |                                           
 Quality of Service Enforcement (PQE)                          |                                           
 capability if 1. Bits 31:16: Reserved                         |                                           
 Reserved                                                      | ECX                                       
 Reserved                                                      | EDX                                       
</aside>

<aside class="notification">
NOTE::
\* If ECX contains an invalid sub-leaf index, EAX/EBX/ECX/EDX return 0. Invalid
sub-leaves of EAX = 07H: ECX = n, n > 0.
</aside>

Direct Cache Access Information Leaf

   | |  
---- | -----
 09H    | EAX EBX ECX EDX Architectural Performance    | Value of bits [31:0] of IA32_PLATFORM_DCA_CAP
        | Monitoring Leaf                              | MSR (address 1F8H) Reserved Reserved         
        |                                              | Reserved                                     
 0AH 0BH| EAX EBX ECX EDX Extended Topology Enumeration| Bits 07 - 00: Version ID of architectural    
        | Leaf EAX EBX ECX EDX                         | performance monitoring Bits 15- 08:          
        |                                              | Number of general-purpose performance        
        |                                              | monitoring counter per logical processor     
        |                                              | Bits 23 - 16: Bit width of general-purpose,  
        |                                              | performance monitoring counter Bits          
        |                                              | 31 - 24: Length of EBX bit vector to         
        |                                              | enumerate architectural performance          
        |                                              | monitoring events Bit 00: Core cycle         
        |                                              | event not available if 1 Bit 01: Instruction 
        |                                              | retired event not available if 1 Bit         
        |                                              | 02: Reference cycles event not available     
        |                                              | if 1 Bit 03: Last-level cache reference      
        |                                              | event not available if 1 Bit 04: Last-level  
        |                                              | cache misses event not available if          
        |                                              | 1 Bit 05: Branch instruction retired         
        |                                              | event not available if 1 Bit 06: Branch      
        |                                              | mispredict retired event not available       
        |                                              | if 1 Bits 31- 07: Reserved = 0 Reserved      
        |                                              | = 0 Bits 04 - 00: Number of fixed-function   
        |                                              | performance counters (if Version ID          
        |                                              | > 1) Bits 12- 05: Bit width of fixed-function
        |                                              | performance counters (if Version ID          
        |                                              | > 1) Reserved = 0 Information Returned       
        |                                              | by CPUID Instruction (Contd.) Initial        
        |                                              | EAX Information Provided about the Processor 
        |                                              | NOTES: Most of Leaf 0BH output depends       
        |                                              | on the initial value in ECX. The EDX         
        |                                              | output of leaf 0BH is always valid and       
        |                                              | does not vary with input value in ECX.       
        |                                              | Output value in ECX[7:0] always equals       
        |                                              | input value in ECX[7:0]. For sub-leaves      
        |                                              | that return an invalid level-type of         
        |                                              | 0 in ECX[15:8]; EAX and EBX will return      
        |                                              | 0. If an input value n in ECX returns        
        |                                              | the invalid level-type of 0 in ECX[15:8],    
        |                                              | other input values with ECX >n also          
        |                                              | return 0 in ECX[15:8]. Bits 04-00: Number    
        |                                              | of bits to shift right on x2APIC ID          
        |                                              | to get a unique topology ID of the next      
        |                                              | level type\*. All logical processors          
        |                                              | with the same next level ID share current    
        |                                              | level. Bits 31-05: Reserved. Bits 15         
        |                                              | - 00: Number of logical processors at        
        |                                              | this level type. The number reflects         
        |                                              | configuration as shipped by Intel\*\*.         
        |                                              | Bits 31- 16: Reserved. Bits 07 - 00:         
        |                                              | Level number. Same value in ECX input        
        |                                              | Bits 15 - 08: Level type\*\*\*. Bits 31         
        |                                              | - 16:: Reserved. Bits 31- 00: x2APIC         
        |                                              | ID the current logical processor.            
<aside class="notification">
\* Software should use this field (EAX[4:0]) to enumerate processor topology
of the system.
</aside>

\*\* Software must not use EBX[15:0] to enumerate processor topology of the system.
This value in this field (EBX[15:0]) is only intended for display/diagnostic
purposes. The actual number of logical processors available to BIOS/OS/Applications
may be different from the value of EBX[15:0], depending on software and platform
hardware configurations.

\*\*\* The value of the “level type” field is not related to level numbers in any
way, higher “level type” values do not mean higher levels. Level type field
has the following encoding: 0 : invalid 1 : SMT 2 : Core 3-255 : Reserved

Processor Extended State Enumeration Main Leaf (EAX = 0DH, ECX = 0)

   | |  
---- | -----
 0DH                                     | NOTES: Leaf 0DH main leaf (ECX = 0).               
 EAX                                     | Bits 31-00: Reports the valid bit fields           
                                         | of the lower 32 bits of XCR0. If a bit             
                                         | is 0, the corresponding bit field in               
                                         | XCR0 is reserved. Bit 00: legacy x87               
                                         | Bit 01: 128-bit SSE Bit 02: 256-bit                
                                         | AVX Bits 31- 03: Reserved                          
 EBX                                     | Bits 31-00: Maximum size (bytes, from              
                                         | the beginning of the XSAVE/XRSTOR save             
                                         | area) required by enabled features in              
                                         | XCR0. May be different than ECX if some            
                                         | features at the end of the XSAVE save              
                                         | area are not enabled.                              
 ECX                                     | Bit 31-00: Maximum size (bytes, from               
                                         | the beginning of the XSAVE/XRSTOR save             
                                         | area) of the XSAVE/XRSTOR save area                
                                         | required by all supported features in              
                                         | the processor, i.e all the valid bit               
                                         | fields in XCR0.                                    
 EDX Processor Extended State Enumeration| Bit 31-00: Reports the valid bit fields            
 Sub-leaf (EAX = 0DH, ECX = 1)           | of the upper 32 bits of XCR0. If a bit             
                                         | is 0, the corresponding bit field in               
                                         | XCR0 is reserved. Information Returned             
                                         | by CPUID Instruction (Contd.) Initial              
                                         | EAX Information Provided about the Processor       
 EAX                                     | Bits 31-04: Reserved Bit 00: XSAVEOPT              
                                         | is available Bit 01: Supports XSAVEC               
                                         | and the compacted form of XRSTOR if                
                                         | set Bit 02: Supports XGETBV with ECX               
                                         | = 1 if set Bit 03: Supports XSAVES/XRSTORS         
                                         | and IA32_XSS if set                                
 EBX                                     | Bits 31-00: The size in bytes of the               
                                         | XSAVE area containing all states enabled           
                                         | by XCRO | IA32_XSS.                                
 ECX                                     | Bits 31-00: Reports the valid bit fields           
                                         | of the lower 32 bits of IA32_XSS. If               
                                         | a bit is 0, the corresponding bit field            
                                         | in IA32_XSS is reserved. Bits 07-00:               
                                         | Reserved Bit 08: IA32_XSS[bit 8] is                
                                         | supported if 1 Bits 31-09: Reserved                
 EDX Processor Extended State Enumeration| Bits 31-00: Reports the valid bit fields           
 Sub-leaves (EAX = 0DH, ECX = n, n >     | of the upper 32 bits of IA32_XSS. If               
 1)                                      | a bit is 0, the corresponding bit field            
                                         | in IA32_XSS is reserved. Bits 31-00:               
                                         | Reserved                                           
 0DH                                     | NOTES: Leaf 0DH output depends on the              
                                         | initial value in ECX. Each valid sub-leaf          
                                         | index maps to a valid bit in either                
                                         | the XCR0 register or the IA32_XSS MSR              
                                         | starting at bit position 2. \* If ECX               
                                         | contains an invalid sub-leaf index,                
                                         | EAX/EBX/ECX/EDX return 0. Invalid sub-leaves       
                                         | of EAX = 0DH: ECX = n, n > 2.                      
 EAX                                     | Bits 31-0: The size in bytes (from the             
                                         | offset specified in EBX) of the save               
                                         | area for an extended state feature associated      
                                         | with a valid sub-leaf index, n. This               
                                         | field reports 0 if the sub-leaf index,             
                                         | n, does not map to a valid bit in the              
                                         | XCR0 register\*.                                    
 EBX                                     | Bits 31-0: The offset in bytes of this             
                                         | extended state component's save area               
                                         | from the beginning of the XSAVE/XRSTOR             
                                         | area. This field reports 0 if the sub-leaf         
                                         | index, n, is invalid\*.                             
 ECX                                     | This field reports 0 if the sub-leaf               
                                         | index, n, is invalid\*; otherwise, bit              
                                         | 0 is set if the sub-leaf index, n, maps            
                                         | to a valid bit in the IA32_XSS MSR,                
                                         | and bits 31-1 are reserved.                        
 EDX Platform QoS Monitoring Enumeration | This field reports 0 if the sub-leaf               
 Sub-leaf (EAX = 0FH, ECX = 0)           | index, n, is invalid\*; otherwise it                
                                         | is reserved.                                       
 0FH                                     | NOTES: Leaf 0FH output depends on the              
                                         | initial value in ECX. Sub-leaf index               
                                         | 0 reports valid resource type starting             
                                         | at bit position 1 of EDX                           
 EAX                                     | Reserved.                                          
 EBX                                     | Bits 31-0: Maximum range (zero-based)              
                                         | of RMID within this physical processor             
                                         | of all types.                                      
 ECX                                     | Reserved.                                          
 EDX L3 Cache QoS Monitoring Capability  | Bit 00: Reserved. Bit 01: Supports L3              
 Enumeration Sub-leaf (EAX = 0FH, ECX    | Cache QoS Monitoring if 1. Bits 31:02:             
 = 1)                                    | Reserved Information Returned by CPUID             
                                         | Instruction (Contd.) Initial EAX Information       
                                         | Provided about the Processor                       
 0FH                                     | NOTES: Leaf 0FH output depends on the              
                                         | initial value in ECX.                              
 EAX                                     | Reserved.                                          
 EBX                                     | Bits 31-0: Conversion factor from reported         
                                         | IA32_QM_CTR value to occupancy metric              
                                         | (bytes).                                           
 ECX                                     | Maximum range (zero-based) of RMID of              
                                         | this resource type.                                
 EDX Platform QoS Enforcement Enumeration| Bit 00: Supports L3 occupancy monitoring           
 Sub-leaf (EAX = 10H, ECX = 0)           | if 1. Bits 31:01: Reserved                         
 10H                                     | NOTES: Leaf 10H output depends on the              
                                         | initial value in ECX. Sub-leaf index               
                                         | 0 reports valid resource identification            
                                         | (ResID) starting at bit position 1 of              
                                         | EDX                                                
 EAX                                     | Reserved.                                          
 EBX                                     | Bit 00: Reserved. Bit 01: Supports L3              
                                         | Cache QoS Enforcement if 1. Bits 31:02:            
                                         | Reserved                                           
 ECX                                     | Reserved.                                          
 EDX L3 Cache QoS Enforcement Enumeration| Reserved.                                          
 Sub-leaf (EAX = 10H, ECX = ResID =1)    |                                                    
 10H                                     | NOTES: Leaf 10H output depends on the              
                                         | initial value in ECX.                              
 EAX                                     | Bits 4:0: Length of the capacity bit               
                                         | mask for the corresponding ResID. Bits             
                                         | 31:05: Reserved                                    
 EBX                                     | Bits 31-0: Bit-granular map of isolation/contention
                                         | of allocation units.                               
 ECX                                     | Bit 00: Reserved. Bit 01: Updates of               
                                         | COS should be infrequent if 1. Bits                
                                         | 31:02: Reserved                                    
 EDX Unimplemented CPUID Leaf Functions  | Bits 15:0: Highest COS number supported            
 Extended Function CPUID Information     | for this ResID. Bits 31:16: Reserved               
                                         | Invalid. No existing or future CPU will            
                                         | return processor identification or feature         
                                         | information if the initial EAX value               
                                         | is in the range 40000000H to 4FFFFFFFH.            
                                         | 4FFFFFFFH                                          
 EAX                                     | Maximum Input Value for Extended Function          
                                         | CPUID Information (see Table 3-18).                
 EBX ECX EDX                             | Reserved Reserved Reserved Information             
                                         | Returned by CPUID Instruction (Contd.)             
                                         | Initial EAX Information Provided about             
                                         | the Processor                                      
 EAX                                     | Extended Processor Signature and Feature           
                                         | Bits.                                              
 EBX                                     | Reserved                                           
 ECX                                     | Bit 00: LAHF/SAHF available in 64-bit              
                                         | mode Bits 04-01 Reserved Bit 05: LZCNT             
                                         | Bits 07-06 Reserved Bit 08: PREFETCHW              
                                         | Bits 31-09 Reserved                                
 EDX                                     | Bits 10-00: Reserved Bit 11: SYSCALL/SYSRET        
                                         | available in 64-bit mode Bits 19-12:               
                                         | Reserved = 0 Bit 20: Execute Disable               
                                         | Bit available Bits 25-21: Reserved =               
                                         | 0 Bit 26: 1-GByte pages are available              
                                         | if 1 Bit 27: RDTSCP and IA32_TSC_AUX               
                                         | are available if 1 Bits 28: Reserved               
                                         | = 0 Bit 29: Intel® 64 Architecture available       
                                         | if 1 Bits 31-30: Reserved = 0                      
 EAX EBX ECX EDX                         | Processor Brand String Processor Brand             
                                         | String Continued Processor Brand String            
                                         | Continued Processor Brand String Continued         
 EAX EBX ECX EDX                         | Processor Brand String Continued Processor         
                                         | Brand String Continued Processor Brand             
                                         | String Continued Processor Brand String            
                                         | Continued                                          
 EAX EBX ECX EDX                         | Processor Brand String Continued Processor         
                                         | Brand String Continued Processor Brand             
                                         | String Continued Processor Brand String            
                                         | Continued                                          
 EAX EBX ECX EDX                         | Reserved = 0 Reserved = 0 Reserved =               
                                         | 0 Reserved = 0                                     
 EAX EBX                                 | Reserved = 0 Reserved = 0                          
 ECX EDX                                 | Bits 07-00: Cache Line size in bytes               
                                         | Bits 11-08: Reserved Bits 15-12: L2                
                                         | Associativity field \*Bits 31-16: Cache             
                                         | size in 1K units Reserved = 0 Information          
                                         | Returned by CPUID Instruction (Contd.)             
                                         | Initial EAX Information Provided about             
                                         | the Processor                                      
<aside class="notification">
\* L2 associativity field encodings: 00H - Disabled 01H - Direct mapped
02H - 2-way 04H - 4-way 06H - 8-way 08H - 16-way 0FH - Fully associative
</aside>

   | |  
---- | -----
 80000007H| EAX EBX ECX EDX| Reserved = 0 Reserved = 0 Reserved =        
          |                | 0 Bits 07-00: Reserved = 0 Bit 08: Invariant
          |                | TSC available if 1 Bits 31-09: Reserved     
          |                | = 0                                         
 80000008H| EAX EBX ECX EDX| Linear/Physical Address size Bits 07-00:    
          |                | **``#Physical``** Address Bits\*Bits 15-8: **``#Linear``**   
          |                | Address Bits Bits 31-16: Reserved =         
          |                | 0 Reserved = 0 Reserved = 0 Reserved        
          |                | = 0                                         
<aside class="notification">

</aside>

   | |  
---- | -----
 \*| If CPUID.80000008H:EAX[7:0] is supported,
  | the maximum physical address number      
  | supported should come from this field.   

### INPUT EAX = 0: Returns CPUID's Highest Value for Basic Processor Information and the Vendor Identification String
When CPUID executes with EAX set to 0, the processor returns the highest value
the CPUID recognizes for returning basic processor information. The value is
returned in the EAX register (see Table 3-18) and is processor specific. A vendor
identification string is also returned in EBX, EDX, and ECX. For Intel processors,
the string is “GenuineIntel” and is expressed: EBX ← 756e6547h (\* \"Genu\", with
G in the low eight bits of BL \*) EDX ← 49656e69h (\* \"ineI\", with i in the low
eight bits of DL \*) ECX ← 6c65746eh (\* \"ntel\", with n in the low eight bits
of CL \*)


### INPUT EAX = 80000000H: Returns CPUID's Highest Value for Extended Processor Information
When CPUID executes with EAX set to 80000000H, the processor returns the highest
value the processor recognizes for returning extended processor information.
The value is returned in the EAX register (see Table 3-18) and is processor
specific.


### Table 3-18. Highest CPUID Source Operand for Intel 64 and IA-32 Processors
Highest Value in EAX Intel 64 or IA-32 Processors

   | |  
---- | -----
 Basic Information                        | Extended Function Information CPUID     
                                          | Not Implemented                         
 01H                                      | Not Implemented                         
 Highest CPUID Source Operand for Intel   | (Contd.) Highest Value in EAX Intel     
 64 and IA-32 Processors Basic Information| 64 or IA-32 Processors Extended Function
                                          | Information                             
 02H                                      | Not Implemented Processors              
 03H                                      | Not Implemented                         
 02H                                      | 80000004H                               
 02H                                      | 80000004H                               
 02H                                      | 80000004H                               
 05H                                      | 80000008H Technology                    
 05H                                      | 80000008H                               
 06H                                      | 80000008H                               
 0AH                                      | 80000008H                               
 0AH                                      | 80000008H                               
 0AH                                      | 80000008H Series                        
 0DH                                      | 80000008H                               
 0AH                                      | 80000008H                               
 0AH                                      | 80000008H                               
 0BH                                      | 80000008H                               

### IA32_BIOS_SIGN_ID Returns Microcode Update Signature
For processors that support the microcode update facility, the IA32_BIOS_SIGN_ID
MSR is loaded with the update signature whenever CPUID executes. The signature
is returned in the upper DWORD. For details, see Chapter 9 in the Intel® 64
and IA-32 Architectures Software Developer's Manual, Volume 3A.


### INPUT EAX = 1: Returns Model, Family, Stepping Information
When CPUID executes with EAX set to 1, version information is returned in EAX
(see Figure 3-5). For example: model, family, and processor type for the Intel
### Xeon processor 5100 series is as follows

 - Model  -  1111B
 - Family  -  0101B
 - Processor Type  -  00B

See Table 3-19 for available processor type values. Stepping IDs are provided
as needed.

   | |  
---- | -----
 31| 28 27 Extended Family ID| 20 19 Extended Model ID| 16 15 14 13 12 11| 8 Family ID| 7 Model| 4| 3 Stepping ID| 0 EAX
Extended Family ID (0) Extended Model ID (0) Processor Type Family (0FH for
the Pentium 4 Processor Family) Model

Reserved

OM16525

   | |  
---- | -----
 Figure 3-5.| Version Information Returned by CPUID
            | in EAX Processor Type Field Encoding 
            | 00B 01B 10B 11B                      
<aside class="notification">
NOTE See Chapter 17 in the Intel® 64 and IA-32 Architectures Software Developer's
Manual, Volume 1, for information on identifying earlier IA-32 processors.
</aside>

The Extended Family ID needs to be examined only when the Family ID is 0FH.
### Integrate the fields into a display using the following rule

IF Family_ID != 0FH THEN DisplayFamily = Family_ID; ELSE DisplayFamily = Extended_Family_ID
+ Family_ID; (\* Right justify and zero-extend 4-bit field. \*) FI; (\* Show DisplayFamily
as HEX field. \*)

The Extended Model ID needs to be examined only when the Family ID is 06H or
### 0FH. Integrate the field into a display using the following rule

IF (Family_ID = 06H or Family_ID = 0FH) THEN DisplayModel = (Extended_Model_ID
« 4) + Model_ID; (\* Right justify and zero-extend 4-bit field; display Model_ID
as HEX field.\*) ELSE DisplayModel = Model_ID; FI; (\* Show DisplayModel as HEX
field. \*)


### INPUT EAX = 1: Returns Additional Information in EBX
When CPUID executes with EAX set to 1, additional information is returned to
### the EBX register

 - Brand index (low byte of EBX)  -  this number provides an entry into a brand string
table that contains brand strings for IA-32 processors. More information about
this field is provided later in this section.
 - CLFLUSH instruction cache line size (second byte of EBX)  -  this number indicates
the size of the cache line flushed with CLFLUSH instruction in 8-byte increments.
This field was introduced in the Pentium 4 processor.
 - Local APIC ID (high byte of EBX)  -  this number is the 8-bit ID that is assigned
to the local APIC on the processor during power up. This field was introduced
in the Pentium 4 processor.


### INPUT EAX = 1: Returns Feature Information in ECX and EDX
When CPUID executes with EAX set to 1, feature information is returned in ECX
and EDX.

 - Figure 3-6 and Table 3-20 show encodings for ECX.
 - Figure 3-7 and Table 3-21 show encodings for EDX.

For all feature flags, a 1 indicates that the feature is supported. Use Intel
to properly interpret feature flags.


<aside class="notification">
NOTE:
Software must confirm that a processor feature is present using feature flags
returned by CPUID prior to using the feature. Software should not depend on
future offerings retaining all features.
</aside>

   | |  
---- | -----
 31 30 29 28 27 26 25 24 23 22 21 20| 17| 16| 15| 14| 13| 12| 11| 10| 9| 8| 7| 6| 5| 4| 3| 2| 1| 0
 19 18                              |   |   |   |   |   |   |   |   |  |  |  |  |  |  |  |  |  |  
ECX 0

RDRAND F16C AVX OSXSAVE XSAVE AES TSC-Deadline POPCNT MOVBE x2APIC

   | |  
---- | -----
 SSE4_2  - SSE4_1  - DCA  - PCID  - PDCM  - | SSE4.2 SSE4.1 Direct Cache Access Process-context
                                  | Identifiers Perf/Debug Capability MSR            
xTPR Update Control CMPXCHG16B

   | |  
---- | -----
 FMA  - SDBG CNXT-ID  -  L1 Context ID SSSE3  | Fused Multiply Add SSSE3 Extensions                 
  - TM2  -  Thermal Monitor 2 EST  - SMX  -       | Technology 64-bit DS Area Carryless                 
 Safer Mode Extensions VMX  -  Virtual      | Multiplication SSE3 Extensions OM16524b             
 Machine Extensions DS-CPL  -  CPL Qualified| Feature Information Returned in the                 
 Debug Store MONITOR  -  MONITOR/MWAIT      | ECX Register Feature Information Returned           
 DTES64 PCLMULQDQ SSE3 Reserved           | in the ECX Register Description Streaming           
                                          | SIMD Extensions 3 (SSE3). A value of                
                                          | 1 indicates the processor supports this             
                                          | technology. PCLMULQDQ. A value of 1                 
                                          | indicates the processor supports the                
                                          | PCLMULQDQ instruction 64-bit DS Area.               
                                          | A value of 1 indicates the processor                
                                          | supports DS area using 64-bit layout                
                                          | MONITOR/MWAIT. A value of 1 indicates               
                                          | the processor supports this feature.                
                                          | CPL Qualified Debug Store. A value of               
                                          | 1 indicates the processor supports the              
                                          | extensions to the Debug Store feature               
                                          | to allow for branch message storage                 
                                          | qualified by CPL. Virtual Machine Extensions.       
                                          | A value of 1 indicates that the processor           
                                          | supports this technology Safer Mode                 
                                          | Extensions. A value of 1 indicates that             
                                          | the processor supports this technology.             
                                          | See Chapter 5, “Safer Mode Extensions               
                                          | Reference”. Enhanced Intel SpeedStep®               
                                          | technology. A value of 1 indicates that             
                                          | the processor supports this technology.             
                                          | Thermal Monitor 2. A value of 1 indicates           
                                          | whether the processor supports this                 
                                          | technology. A value of 1 indicates the              
                                          | presence of the Supplemental Streaming              
                                          | SIMD Extensions 3 (SSSE3). A value of               
                                          | 0 indicates the instruction extensions              
                                          | are not present in the processor (Contd.)           
                                          | Description L1 Context ID. A value of               
                                          | 1 indicates the L1 data cache mode can              
                                          | be set to either adaptive mode or shared            
                                          | mode. A value of 0 indicates this feature           
                                          | is not supported. See definition of                 
                                          | the IA32_MISC_ENABLE MSR Bit 24 (L1                 
                                          | Data Cache Context Mode) for details.               
                                          | A value of 1 indicates the processor                
                                          | supports IA32_DEBUG_INTERFACE MSR for               
                                          | silicon debug. A value of 1 indicates               
                                          | the processor supports FMA extensions               
                                          | using YMM state. CMPXCHG16B Available.              
                                          | A value of 1 indicates that the feature             
                                          | is available. See the “CMPXCHG8B/CMPXCHG16B - Compare 
                                          | and Exchange Bytes” section in this                 
                                          | chapter for a description. xTPR Update              
                                          | Control. A value of 1 indicates that                
                                          | the processor supports changing IA32_MISC_ENABLE[bit
                                          | 23]. Perfmon and Debug Capability: A                
                                          | value of 1 indicates the processor supports         
                                          | the performance and debug feature indication        
                                          | MSR IA32_PERF_CAPABILITIES. Reserved                
                                          | Process-context identifiers. A value                
                                          | of 1 indicates that the processor supports          
                                          | PCIDs and that software may set CR4.PCIDE           
                                          | to 1. A value of 1 indicates the processor          
                                          | supports the ability to prefetch data               
                                          | from a memory mapped device. A value                
                                          | of 1 indicates that the processor supports          
                                          | SSE4.1. A value of 1 indicates that                 
                                          | the processor supports SSE4.2. A value              
                                          | of 1 indicates that the processor supports          
                                          | x2APIC feature. A value of 1 indicates              
                                          | that the processor supports MOVBE instruction.      
                                          | A value of 1 indicates that the processor           
                                          | supports the POPCNT instruction. A value            
                                          | of 1 indicates that the processor's                 
                                          | local APIC timer supports one-shot operation        
                                          | using a TSC deadline value. A value                 
                                          | of 1 indicates that the processor supports          
                                          | the AESNI instruction extensions. A                 
                                          | value of 1 indicates that the processor             
                                          | supports the XSAVE/XRSTOR processor                 
                                          | extended states feature, the XSETBV/XGETBV          
                                          | instructions, and XCR0. A value of 1                
                                          | indicates that the OS has set CR4.OSXSAVE[bit       
                                          | 18] to enable the XSAVE feature set.                
                                          | A value of 1 indicates the processor                
                                          | supports the AVX instruction extensions.            
                                          | A value of 1 indicates that processor               
                                          | supports 16-bit floating-point conversion           
                                          | instructions. A value of 1 indicates                
                                          | that processor supports RDRAND instruction.         
                                          | Always returns 0. 0                                 
EDX

PBE-Pend. Brk. EN. TM-Therm. Monitor HTT-Multi-threading SS-Self Snoop SSE2-SSE2
Extensions SSE-SSE Extensions FXSR-FXSAVE/FXRSTOR MMX-MMX Technology ACPI-Thermal
Monitor and Clock Ctrl DS-Debug Store CLFSH-CFLUSH instruction PSN-Processor
Serial Number PSE-36 - Page Size Extension PAT-Page Attribute Table CMOV-Conditional
Move/Compare Instruction MCA-Machine Check Architecture PGE-PTE Global Bit MTRR-Memory
Type Range Registers SEP-SYSENTER and SYSEXIT APIC-APIC on Chip CX8-CMPXCHG8B
Inst. MCE-Machine Check Exception PAE-Physical Address Extensions MSR-RDMSR
and WRMSR Support TSC-Time Stamp Counter PSE-Page Size Extensions DE-Debugging
Extensions VME-Virtual-8086 Mode Enhancement FPU-x87 FPU on Chip

Reserved

OM16523

   | |  
---- | -----
 Figure 3-7.                         | Feature Information Returned in the               
                                     | EDX Register More on Feature Information          
                                     | Returned in the EDX Register Description          
                                     | Floating Point Unit On-Chip. The processor        
                                     | contains an x87 FPU. Virtual 8086 Mode            
                                     | Enhancements. Virtual 8086 mode enhancements,     
                                     | including CR4.VME for controlling the             
                                     | feature, CR4.PVI for protected mode               
                                     | virtual interrupts, software interrupt            
                                     | indirection, expansion of the TSS with            
                                     | the software indirection bitmap, and              
                                     | EFLAGS.VIF and EFLAGS.VIP flags. Debugging        
                                     | Extensions. Support for I/O breakpoints,          
                                     | including CR4.DE for controlling the              
                                     | feature, and optional trapping of accesses        
                                     | to DR4 and DR5. Page Size Extension.              
                                     | Large pages of size 4 MByte are supported,        
                                     | including CR4.PSE for controlling the             
                                     | feature, the defined dirty bit in PDE             
                                     | (Page Directory Entries), optional reserved       
                                     | bit trapping in CR3, PDEs, and PTEs.              
                                     | Time Stamp Counter. The RDTSC instruction         
                                     | is supported, including CR4.TSD for               
                                     | controlling privilege. Model Specific             
                                     | Registers RDMSR and WRMSR Instructions.           
                                     | The RDMSR and WRMSR instructions are              
                                     | supported. Some of the MSRs are implementation    
                                     | dependent. Physical Address Extension.            
                                     | Physical addresses greater than 32 bits           
                                     | are supported: extended page table entry          
                                     | formats, an extra level in the page               
                                     | translation tables is defined, 2-MByte            
                                     | pages are supported instead of 4 Mbyte            
                                     | pages if PAE bit is 1. Machine Check              
                                     | Exception. Exception 18 is defined for            
                                     | Machine Checks, including CR4.MCE for             
                                     | controlling the feature. This feature             
                                     | does not define the model-specific implementations
                                     | of machine-check error logging, reporting,        
                                     | and processor shutdowns. Machine Check            
                                     | exception handlers may have to depend             
                                     | on processor version to do model specific         
                                     | processing of the exception, or test              
                                     | for the presence of the Machine Check             
                                     | feature. CMPXCHG8B Instruction. The               
                                     | compare-and-exchange 8 bytes (64 bits)            
                                     | instruction is supported (implicitly              
                                     | locked and atomic). APIC On-Chip. The             
                                     | processor contains an Advanced Programmable       
                                     | Interrupt Controller (APIC), responding           
                                     | to memory mapped commands in the physical         
                                     | address range FFFE0000H to FFFE0FFFH              
                                     | (by default - some processors permit              
                                     | the APIC to be relocated). Reserved               
                                     | SYSENTER and SYSEXIT Instructions. The            
                                     | SYSENTER and SYSEXIT and associated               
                                     | MSRs are supported. Memory Type Range             
                                     | Registers. MTRRs are supported. The               
                                     | MTRRcap MSR contains feature bits that            
                                     | describe what memory types are supported,         
                                     | how many variable MTRRs are supported,            
                                     | and whether fixed MTRRs are supported.            
                                     | Page Global Bit. The global bit is supported      
                                     | in paging-structure entries that map              
                                     | a page, indicating TLB entries that               
                                     | are common to different processes and             
                                     | need not be flushed. The CR4.PGE bit              
                                     | controls this feature. Machine Check              
                                     | Architecture. The Machine Check Architecture,     
                                     | which provides a compatible mechanism             
                                     | for error reporting in P6 family, Pentium         
                                     | 4, Intel Xeon processors, and future              
                                     | processors, is supported. The MCG_CAP             
                                     | MSR contains feature bits describing              
                                     | how many banks of error reporting MSRs            
                                     | are supported. Conditional Move Instructions.     
                                     | The conditional move instruction CMOV             
                                     | is supported. In addition, if x87 FPU             
                                     | is present as indicated by the CPUID.FPU          
                                     | feature bit, then the FCOMI and FCMOV             
                                     | instructions are supported Page Attribute         
                                     | Table. Page Attribute Table is supported.         
                                     | This feature augments the Memory Type             
                                     | Range Registers (MTRRs), allowing an              
                                     | operating system to specify attributes            
                                     | of memory accessed through a linear               
                                     | address on a 4KB granularity. 36-Bit              
                                     | Page Size Extension. 4-MByte pages addressing     
                                     | physical memory beyond 4 GBytes are               
                                     | supported with 32-bit paging. This feature        
                                     | indicates that upper bits of the physical         
                                     | address of a 4-MByte page are encoded             
                                     | in bits 20:13 of the page-directory               
                                     | entry. Such physical addresses are limited        
                                     | by MAXPHYADDR and may be up to 40 bits            
                                     | in size. Processor Serial Number. The             
                                     | processor supports the 96-bit processor           
                                     | identification number feature and the             
                                     | feature is enabled. CLFLUSH Instruction.          
                                     | CLFLUSH Instruction is supported. Reserved        
 More on Feature Information Returned| Table 3-21. Description Debug Store.              
 in the EDX Register (Contd.)        | The processor supports the ability to             
                                     | write debug information into a memory             
                                     | resident buffer. This feature is used             
                                     | by the branch trace store (BTS) and               
                                     | precise event-based sampling (PEBS)               
                                     | facilities (see Chapter 23, “Introduction         
                                     | to Virtual-Machine Extensions,” in the            
                                     | Intel® 64 and IA-32 Architectures Software        
                                     | Developer's Manual, Volume 3C). Thermal           
                                     | Monitor and Software Controlled Clock             
                                     | Facilities. The processor implements              
                                     | internal MSRs that allow processor temperature    
                                     | to be monitored and processor performance         
                                     | to be modulated in predefined duty cycles         
                                     | under software control. Intel MMX Technology.     
                                     | The processor supports the Intel MMX              
                                     | technology. FXSAVE and FXRSTOR Instructions.      
                                     | The FXSAVE and FXRSTOR instructions               
                                     | are supported for fast save and restore           
                                     | of the floating point context. Presence           
                                     | of this bit also indicates that CR4.OSFXSR        
                                     | is available for an operating system              
                                     | to indicate that it supports the FXSAVE           
                                     | and FXRSTOR instructions. SSE. The processor      
                                     | supports the SSE extensions. SSE2. The            
                                     | processor supports the SSE2 extensions.           
                                     | Self Snoop. The processor supports the            
                                     | management of conflicting memory types            
                                     | by performing a snoop of its own cache            
                                     | structure for transactions issued to              
                                     | the bus. Max APIC IDs reserved field              
                                     | is Valid. A value of 0 for HTT indicates          
                                     | there is only a single logical processor          
                                     | in A value of 1 for HTT indicates the             
                                     | value in CPUID.1.EBX[23:16] (the Maximum          
                                     | number of addressable IDs for logical             
                                     | processors in this package) is valid              
                                     | for the package. Thermal Monitor. The             
                                     | processor implements the thermal monitor          
                                     | automatic thermal control circuitry               
                                     | (TCC). Reserved Pending Break Enable.             
                                     | The processor supports the use of the             
                                     | FERR**``#/PBE#``** pin when the processor is              
                                     | in the stop-clock state (STPCLK# is               
                                     | asserted) to signal the processor that            
                                     | an interrupt is pending and that the              
                                     | processor should return to normal operation       
                                     | to handle the interrupt. Bit 10 (PBE              
                                     | enable) in the IA32_MISC_ENABLE MSR               
                                     | enables this capability.                          

### INPUT EAX = 2: TLB/Cache/Prefetch Information Returned in EAX, EBX, ECX, EDX
When CPUID executes with EAX set to 2, the processor returns information about
the processor's internal TLBs, cache and prefetch hardware in the EAX, EBX,
ECX, and EDX registers. The information is reported in encoded form and fall
### into the following categories

 - The least-significant byte in register EAX (register AL) indicates the number
of times the CPUID instruction must be executed with an input value of 2 to
get a complete description of the processor's TLB/Cache/Prefetch hardware. The
Intel Xeon processor 7400 series will return a 1.
 - The most significant bit (bit 31) of each register indicates whether the register
contains valid information (set to 0) or is reserved (set to 1).
 - If a register contains valid information, the information is contained in 1
byte descriptors. There are four types of encoding values for the byte descriptor,
the encoding type is noted in the second column of Table 3-22. Table 3-22 lists
the encoding of these descriptors. Note that the order of descriptors in the
EAX, EBX, ECX, and EDX registers is not defined; that is, specific bytes are
not designated to contain descriptors for specific cache, prefetch, or TLB types.
The descriptors may appear in any order. Note also a processor may report a
general descriptor type (FFH) and not report any byte descriptor of “cache type”
via CPUID leaf 2. Table 3-22. Encoding of CPUID Leaf 2 Descriptors

   | |  
---- | -----
 Value 00H 01H 02H 03H 04H 05H 06H 08H     | Type Null descriptor, this byte contains       | Description (Contd.)                     
 09H 0AH 0BH 0CH 0DH 0EH 1DH 21H 22H       | no information Instruction TLB: 4 KByte        |                                          
 23H 24H 25H 29H 2CH 30H 40H 41H 42H       | pages, 4-way set associative, 32 entries       |                                          
 43H 44H 45H 46H 47H 48H 49H 4AH 4BH       | Instruction TLB: 4 MByte pages, fully          |                                          
 4CH 4DH 4EH 4FH                           | associative, 2 entries Data TLB: 4 KByte       |                                          
                                           | pages, 4-way set associative, 64 entries       |                                          
                                           | Data TLB: 4 MByte pages, 4-way set associative,|                                          
                                           | 8 entries Data TLB1: 4 MByte pages,            |                                          
                                           | 4-way set associative, 32 entries 1st-level    |                                          
                                           | instruction cache: 8 KBytes, 4-way set         |                                          
                                           | associative, 32 byte line size 1st-level       |                                          
                                           | instruction cache: 16 KBytes, 4-way            |                                          
                                           | set associative, 32 byte line size 1st-level   |                                          
                                           | instruction cache: 32KBytes, 4-way set         |                                          
                                           | associative, 64 byte line size 1st-level       |                                          
                                           | data cache: 8 KBytes, 2-way set associative,   |                                          
                                           | 32 byte line size Instruction TLB: 4           |                                          
                                           | MByte pages, 4-way set associative,            |                                          
                                           | 4 entries 1st-level data cache: 16 KBytes,     |                                          
                                           | 4-way set associative, 32 byte line            |                                          
                                           | size 1st-level data cache: 16 KBytes,          |                                          
                                           | 4-way set associative, 64 byte line            |                                          
                                           | size 1st-level data cache: 24 KBytes,          |                                          
                                           | 6-way set associative, 64 byte line            |                                          
                                           | size 2nd-level cache: 128 KBytes, 2-way        |                                          
                                           | set associative, 64 byte line size 2nd-level   |                                          
                                           | cache: 256 KBytes, 8-way set associative,      |                                          
                                           | 64 byte line size 3rd-level cache: 512         |                                          
                                           | KBytes, 4-way set associative, 64 byte         |                                          
                                           | line size, 2 lines per sector 3rd-level        |                                          
                                           | cache: 1 MBytes, 8-way set associative,        |                                          
                                           | 64 byte line size, 2 lines per sector          |                                          
                                           | 2nd-level cache: 1 MBytes, 16-way set          |                                          
                                           | associative, 64 byte line size 3rd-level       |                                          
                                           | cache: 2 MBytes, 8-way set associative,        |                                          
                                           | 64 byte line size, 2 lines per sector          |                                          
                                           | 3rd-level cache: 4 MBytes, 8-way set           |                                          
                                           | associative, 64 byte line size, 2 lines        |                                          
                                           | per sector 1st-level data cache: 32            |                                          
                                           | KBytes, 8-way set associative, 64 byte         |                                          
                                           | line size 1st-level instruction cache:         |                                          
                                           | 32 KBytes, 8-way set associative, 64           |                                          
                                           | byte line size No 2nd-level cache or,          |                                          
                                           | if processor contains a valid 2nd-level        |                                          
                                           | cache, no 3rd-level cache 2nd-level            |                                          
                                           | cache: 128 KBytes, 4-way set associative,      |                                          
                                           | 32 byte line size 2nd-level cache: 256         |                                          
                                           | KBytes, 4-way set associative, 32 byte         |                                          
                                           | line size 2nd-level cache: 512 KBytes,         |                                          
                                           | 4-way set associative, 32 byte line            |                                          
                                           | size 2nd-level cache: 1 MByte, 4-way           |                                          
                                           | set associative, 32 byte line size 2nd-level   |                                          
                                           | cache: 2 MByte, 4-way set associative,         |                                          
                                           | 32 byte line size 3rd-level cache: 4           |                                          
                                           | MByte, 4-way set associative, 64 byte          |                                          
                                           | line size 3rd-level cache: 8 MByte,            |                                          
                                           | 8-way set associative, 64 byte line            |                                          
                                           | size 2nd-level cache: 3MByte, 12-way           |                                          
                                           | set associative, 64 byte line size 3rd-level   |                                          
                                           | cache: 4MB, 16-way set associative,            |                                          
                                           | 64-byte line size (Intel Xeon processor        |                                          
                                           | MP, Family 0FH, Model 06H); 2nd-level          |                                          
                                           | cache: 4 MByte, 16-way set associative,        |                                          
                                           | 64 byte line size 3rd-level cache: 6MByte,     |                                          
                                           | 12-way set associative, 64 byte line           |                                          
                                           | size 3rd-level cache: 8MByte, 16-way           |                                          
                                           | set associative, 64 byte line size 3rd-level   |                                          
                                           | cache: 12MByte, 12-way set associative,        |                                          
                                           | 64 byte line size 3rd-level cache: 16MByte,    |                                          
                                           | 16-way set associative, 64 byte line           |                                          
                                           | size 2nd-level cache: 6MByte, 24-way           |                                          
                                           | set associative, 64 byte line size Instruction |                                          
                                           | TLB: 4 KByte pages, 32 entries Table           |                                          
                                           | 3-22.                                          |                                          
 Value 50H 51H 52H 55H 56H 57H 59H 5AH     | Type Instruction TLB: 4 KByte and 2-MByte      | Description (Contd.)                     
 5BH 5CH 5DH 60H 61H 63H 66H 67H 68H       | or 4-MByte pages, 64 entries Instruction       |                                          
 70H 71H 72H 76H 78H 79H 7AH 7BH 7CH       | TLB: 4 KByte and 2-MByte or 4-MByte            |                                          
 7DH 7FH 80H 82H 83H 84H 85H 86H 87H       | pages, 128 entries Instruction TLB:            |                                          
 A0H B0H B1H B2H B3H B4H                   | 4 KByte and 2-MByte or 4-MByte pages,          |                                          
                                           | 256 entries Instruction TLB: 2-MByte           |                                          
                                           | or 4-MByte pages, fully associative,           |                                          
                                           | 7 entries Data TLB0: 4 MByte pages,            |                                          
                                           | 4-way set associative, 16 entries Data         |                                          
                                           | TLB0: 4 KByte pages, 4-way associative,        |                                          
                                           | 16 entries Data TLB0: 4 KByte pages,           |                                          
                                           | fully associative, 16 entries Data TLB0:       |                                          
                                           | 2-MByte or 4 MByte pages, 4-way set            |                                          
                                           | associative, 32 entries Data TLB: 4            |                                          
                                           | KByte and 4 MByte pages, 64 entries            |                                          
                                           | Data TLB: 4 KByte and 4 MByte pages,128        |                                          
                                           | entries Data TLB: 4 KByte and 4 MByte          |                                          
                                           | pages,256 entries 1st-level data cache:        |                                          
                                           | 16 KByte, 8-way set associative, 64            |                                          
                                           | byte line size Instruction TLB: 4 KByte        |                                          
                                           | pages, fully associative, 48 entries           |                                          
                                           | Data TLB: 1 GByte pages, 4-way set associative,|                                          
                                           | 4 entries 1st-level data cache: 8 KByte,       |                                          
                                           | 4-way set associative, 64 byte line            |                                          
                                           | size 1st-level data cache: 16 KByte,           |                                          
                                           | 4-way set associative, 64 byte line            |                                          
                                           | size 1st-level data cache: 32 KByte,           |                                          
                                           | 4-way set associative, 64 byte line            |                                          
                                           | size Trace cache: 12 K-μop, 8-way set          |                                          
                                           | associative Trace cache: 16 K-μop, 8-way       |                                          
                                           | set associative Trace cache: 32 K-μop,         |                                          
                                           | 8-way set associative Instruction TLB:         |                                          
                                           | 2M/4M pages, fully associative, 8 entries      |                                          
                                           | 2nd-level cache: 1 MByte, 4-way set            |                                          
                                           | associative, 64byte line size 2nd-level        |                                          
                                           | cache: 128 KByte, 8-way set associative,       |                                          
                                           | 64 byte line size, 2 lines per sector          |                                          
                                           | 2nd-level cache: 256 KByte, 8-way set          |                                          
                                           | associative, 64 byte line size, 2 lines        |                                          
                                           | per sector 2nd-level cache: 512 KByte,         |                                          
                                           | 8-way set associative, 64 byte line            |                                          
                                           | size, 2 lines per sector 2nd-level cache:      |                                          
                                           | 1 MByte, 8-way set associative, 64 byte        |                                          
                                           | line size, 2 lines per sector 2nd-level        |                                          
                                           | cache: 2 MByte, 8-way set associative,         |                                          
                                           | 64byte line size 2nd-level cache: 512          |                                          
                                           | KByte, 2-way set associative, 64-byte          |                                          
                                           | line size 2nd-level cache: 512 KByte,          |                                          
                                           | 8-way set associative, 64-byte line            |                                          
                                           | size 2nd-level cache: 256 KByte, 8-way         |                                          
                                           | set associative, 32 byte line size 2nd-level   |                                          
                                           | cache: 512 KByte, 8-way set associative,       |                                          
                                           | 32 byte line size 2nd-level cache: 1           |                                          
                                           | MByte, 8-way set associative, 32 byte          |                                          
                                           | line size 2nd-level cache: 2 MByte,            |                                          
                                           | 8-way set associative, 32 byte line            |                                          
                                           | size 2nd-level cache: 512 KByte, 4-way         |                                          
                                           | set associative, 64 byte line size 2nd-level   |                                          
                                           | cache: 1 MByte, 8-way set associative,         |                                          
                                           | 64 byte line size DTLB: 4k pages, fully        |                                          
                                           | associative, 32 entries Instruction            |                                          
                                           | TLB: 4 KByte pages, 4-way set associative,     |                                          
                                           | 128 entries Instruction TLB: 2M pages,         |                                          
                                           | 4-way, 8 entries or 4M pages, 4-way,           |                                          
                                           | 4 entries Instruction TLB: 4KByte pages,       |                                          
                                           | 4-way set associative, 64 entries Data         |                                          
                                           | TLB: 4 KByte pages, 4-way set associative,     |                                          
                                           | 128 entries Data TLB1: 4 KByte pages,          |                                          
                                           | 4-way associative, 256 entries Table           |                                          
                                           | 3-22.                                          |                                          
 Value B5H B6H BAH C0H C1H C2H CAH D0H     | Type Instruction TLB: 4KByte pages,            | Description Example 3-1. The first member
 D1H D2H D6H D7H D8H DCH DDH DEH E2H       | 8-way set associative, 64 entries Instruction  | of the family of Pentium 4 processors    
 E3H E4H EAH EBH ECH F0H F1H FFH EAX       | TLB: 4KByte pages, 8-way set associative,      | returns the following information about  
 EBX ECX EDX The least-significant byte    | 128 entries Data TLB1: 4 KByte pages,          | caches and TLBs when the CPUID executes  
 (byte 0) of register EAX is set to 01H.   | 4-way associative, 64 entries Data TLB:        | with an input value of 2: Which means:   
 This indicates that CPUID needs to be     | 4 KByte and 4 MByte pages, 4-way associative,  | •••••                                    
 executed once with an input value of      | 8 entries Shared 2nd-Level TLB: 4 KByte/2MByte |                                          
 2 to retrieve complete information about  | pages, 8-way associative, 1024 entries         |                                          
 caches and TLBs. The most-significant     | DTLB: 4 KByte/2 MByte pages, 4-way associative,|                                          
 bit of all four registers (EAX, EBX,      | 16 entries Shared 2nd-Level TLB: 4 KByte       |                                          
 ECX, and EDX) is set to 0, indicating     | pages, 4-way associative, 512 entries          |                                          
 that each register contains valid 1-byte  | 3rd-level cache: 512 KByte, 4-way set          |                                          
 descriptors. Bytes 1, 2, and 3 of register| associative, 64 byte line size 3rd-level       |                                          
 EAX indicate that the processor has:      | cache: 1 MByte, 4-way set associative,         |                                          
  -  -  - The descriptors in registers EBX       | 64 byte line size 3rd-level cache: 2           |                                          
 and ECX are valid, but contain NULL       | MByte, 4-way set associative, 64 byte          |                                          
 descriptors. Bytes 0, 1, 2, and 3 of      | line size 3rd-level cache: 1 MByte,            |                                          
 register EDX indicate that the processor  | 8-way set associative, 64 byte line            |                                          
 has:  -  -  -  -                                  | size 3rd-level cache: 2 MByte, 8-way           |                                          
                                           | set associative, 64 byte line size 3rd-level   |                                          
                                           | cache: 4 MByte, 8-way set associative,         |                                          
                                           | 64 byte line size 3rd-level cache: 1.5         |                                          
                                           | MByte, 12-way set associative, 64 byte         |                                          
                                           | line size 3rd-level cache: 3 MByte,            |                                          
                                           | 12-way set associative, 64 byte line           |                                          
                                           | size 3rd-level cache: 6 MByte, 12-way          |                                          
                                           | set associative, 64 byte line size 3rd-level   |                                          
                                           | cache: 2 MByte, 16-way set associative,        |                                          
                                           | 64 byte line size 3rd-level cache: 4           |                                          
                                           | MByte, 16-way set associative, 64 byte         |                                          
                                           | line size 3rd-level cache: 8 MByte,            |                                          
                                           | 16-way set associative, 64 byte line           |                                          
                                           | size 3rd-level cache: 12MByte, 24-way          |                                          
                                           | set associative, 64 byte line size 3rd-level   |                                          
                                           | cache: 18MByte, 24-way set associative,        |                                          
                                           | 64 byte line size 3rd-level cache: 24MByte,    |                                          
                                           | 24-way set associative, 64 byte line           |                                          
                                           | size 64-Byte prefetching 128-Byte prefetching  |                                          
                                           | CPUID leaf 2 does not report cache descriptor  |                                          
                                           | information, use CPUID leaf 4 to query         |                                          
                                           | cache parameters Example of Cache and          |                                          
                                           | TLB Interpretation 66 5B 50 01H 0H 0H          |                                          
                                           | 00 7A 70 00H 50H - a 64-entry instruction      |                                          
                                           | TLB, for mapping 4-KByte and 2-MByte           |                                          
                                           | or 4-MByte pages. 5BH - a 64-entry data        |                                          
                                           | TLB, for mapping 4-KByte and 4-MByte           |                                          
                                           | pages. 66H - an 8-KByte 1st level data         |                                          
                                           | cache, 4-way set associative, with a           |                                          
                                           | 64-Byte cache line size. 00H - NULL            |                                          
                                           | descriptor. 70H - Trace cache: 12 K-μop,       |                                          
                                           | 8-way set associative. 7AH - a 256-KByte       |                                          
                                           | 2nd level cache, 8-way set associative,        |                                          
                                           | with a sectored, 64-byte cache line            |                                          
                                           | size. 00H - NULL descriptor.                   |                                          

### INPUT EAX = 04H: Returns Deterministic Cache Parameters for Each Level
When CPUID executes with EAX set to 04H and ECX contains an index value, the
processor returns encoded data that describe a set of deterministic cache parameters
(for the cache level associated with the input in ECX). Valid index values start
from 0.

Software can enumerate the deterministic cache parameters for each level of
the cache hierarchy starting with an index value of 0, until the parameters
report the value associated with the cache type field is 0. The architecturally
defined fields reported by deterministic cache parameters are documented in
Table 3-17.

This Cache Size in Bytes

= (Ways + 1) \* (Partitions + 1) \* (Line_Size + 1) \* (Sets + 1)

= (EBX[31:22] + 1) \* (EBX[21:12] + 1) \* (EBX[11:0] + 1) \* (ECX + 1)

The CPUID leaf 04H also reports data that can be used to derive the topology
of processor cores in a physical package. This information is constant for all
valid index values. Software can query the raw data reported by executing CPUID
with EAX=04H and ECX=0 and use it as part of the topology enumeration algorithm
described in Chapter 8, “Multiple-Processor Management,” in the Intel® 64 and
IA-32 Architectures Software Developer's Manual, Volume 3A.


### INPUT EAX = 05H: Returns MONITOR and MWAIT Features
When CPUID executes with EAX set to 05H, the processor returns information about
features available to MONITOR/MWAIT instructions. The MONITOR instruction is
used for address-range monitoring in conjunction with MWAIT instruction. The
MWAIT instruction optionally provides additional extensions for advanced power
management. See Table 3-17.


### INPUT EAX = 06H: Returns Thermal and Power Management Features
When CPUID executes with EAX set to 06H, the processor returns information about
thermal and power management features. See Table 3-17.


### INPUT EAX = 07H: Returns Structured Extended Feature Enumeration Information
When CPUID executes with EAX set to 07H and ECX = 0, the processor returns information
about the maximum input value for sub-leaves that contain extended feature flags.
See Table 3-17.

When CPUID executes with EAX set to 07H and the input value of ECX is invalid
(see leaf 07H entry in Table 3-17), the processor returns 0 in EAX/EBX/ECX/EDX.
In subleaf 0, EAX returns the maximum input value of the highest leaf 7 sub-leaf,
and EBX, ECX & EDX contain information of extended feature flags.


### INPUT EAX = 09H: Returns Direct Cache Access Information
When CPUID executes with EAX set to 09H, the processor returns information about
Direct Cache Access capabilities. See Table 3-17.


### INPUT EAX = 0AH: Returns Architectural Performance Monitoring Features
When CPUID executes with EAX set to 0AH, the processor returns information about
support for architectural performance monitoring capabilities. Architectural
performance monitoring is supported if the version ID (see Table 3-17) is greater
than Pn 0. See Table 3-17.

For each version of architectural performance monitoring capability, software
must enumerate this leaf to discover the programming facilities and the architectural
performance events available in the processor. The details are described in
Chapter 23, “Introduction to Virtual-Machine Extensions,” in the Intel® 64 and
IA-32 Architectures Software Developer's Manual, Volume 3C.


### INPUT EAX = 0BH: Returns Extended Topology Information
When CPUID executes with EAX set to 0BH, the processor returns information about
extended topology enumeration data. Software must detect the presence of CPUID
leaf 0BH by verifying (a) the highest leaf index supported by CPUID is >= 0BH,
and (b) CPUID.0BH:EBX[15:0] reports a non-zero value. See Table 3-17.


### INPUT EAX = 0DH: Returns Processor Extended States Enumeration Information
When CPUID executes with EAX set to 0DH and ECX = 0, the processor returns information
about the bit-vector representation of all processor state extensions that are
supported in the processor and storage size requirements of the XSAVE/XRSTOR
area. See Table 3-17.

When CPUID executes with EAX set to 0DH and ECX = n (n > 1, and is a valid sub-leaf
index), the processor returns information about the size and offset of each
processor extended state save area within the XSAVE/XRSTOR area. See Table 3-17.
Software can use the forward-extendable technique depicted below to query the
valid sub-leaves and obtain size and offset information for each processor extended
### state save area

For i = 2 to 62 // sub-leaf 1 is reserved IF (CPUID.(EAX=0DH, ECX=0):VECTOR[i]
= 1 ) // VECTOR is the 64-bit value of EDX:EAX Execute CPUID.(EAX=0DH, ECX =
i) to examine size and offset for sub-leaf i; FI;


### INPUT EAX = 0FH: Returns Platform Quality of Service (PQoS) Monitoring Enumeration Information
When CPUID executes with EAX set to 0FH and ECX = 0, the processor returns information
about the bit-vector representation of QoS monitoring resource types that are
supported in the processor and maximum range of RMID values the processor can
use to monitor of any supported resource types. Each bit, starting from bit
1, corresponds to a specific resource type if the bit is set. The bit position
corresponds to the sub-leaf index (or ResID) that software must use to query
QoS monitoring capability available for that type. See Table 3-17.

When CPUID executes with EAX set to 0FH and ECX = n (n >= 1, and is a valid
ResID), the processor returns information software can use to program IA32_PQR_ASSOC,
IA32_QM_EVTSEL MSRs before reading QoS data from the IA32_QM_CTR MSR.


### INPUT EAX = 10H: Returns Platform Quality of Service (PQoS) Enforcement Enumeration Information
When CPUID executes with EAX set to 10H and ECX = 0, the processor returns information
about the bit-vector representation of QoS Enforcement resource types that are
supported in the processor. Each bit, starting from bit 1, corresponds to a
specific resource type if the bit is set. The bit position corresponds to the
sub-leaf index (or ResID) that software must use to query QoS enforcement capability
available for that type. See Table 3-17.

When CPUID executes with EAX set to 10H and ECX = n (n >= 1, and is a valid
ResID), the processor returns information about available classes of service
and range of QoS mask MSRs that software can use to configure each class of
services using capability bit masks in the QoS Mask registers, IA32_resourceType_Mask_n.


### METHODS FOR RETURNING BRANDING INFORMATION
### Use the following techniques to access branding information

   | |  
---- | -----
 1.| Processor brand string method; this    
   | method also returns the processor's    
   | maximum operating frequency            
 2.| Processor brand index; this method uses
   | a software supplied brand string table.
These two methods are discussed in the following sections. For methods that
are available in early processors, see Section: “Identification of Earlier IA-32
Processors” in Chapter 17 of the Intel® 64 and IA-32 Architectures Software
Developer's Manual, Volume 1.


### The Processor Brand String Method
Figure 3-8 describes the algorithm used for detection of the brand string. Processor
brand identification software should execute this algorithm on all Intel 64
and IA-32 processors.

This method (introduced with Pentium 4 processors) returns an ASCII brand identification
string and the maximum operating frequency of the processor to the EAX, EBX,
ECX, and EDX registers.

Input: EAX=0x80000000

CPUID

   | |  
---- | -----
 False| Processor Brand String Not Supported
CPUID True ≥Function Extended Supported

EAX Return Value =Max. Extended CPUID Function Index

True

   | |  
---- | -----
 IF (EAX Return Value ≥ 0x80000004)| Processor Brand String Supported
OM15194

   | |  
---- | -----
 Figure 3-8.| Determination of Support for the Processor
            | Brand String                              

### How Brand Strings Work
To use the brand string method, execute CPUID with EAX input of 8000002H through
80000004H. For each input value, CPUID returns 16 ASCII characters using EAX,
EBX, ECX, and EDX. The returned string will be NULL-terminated.

Table 3-23 shows the brand string that is returned by the first processor in
the Pentium 4 processor family.


### Table 3-23. Processor Brand String Returned with Pentium 4 Processor
   | |  
---- | -----
 EAX Input Value| Return Values                              | ASCII Equivalent                      
 Table 3-23.    | Processor Brand String Returned with       | (Contd.) ””””“(let”“P )R”“itne”“R(mu”“
                | Pentium 4 Processor EAX = 20202020H        | 4 )”“ UPC”“0051”“\\0zHM”               
                | EBX = 20202020H ECX = 20202020H EDX        |                                       
                | = 6E492020H EAX = 286C6574H EBX = 50202952H|                                       
                | ECX = 69746E65H EDX = 52286D75H EAX        |                                       
                | = 20342029H EBX = 20555043H ECX = 30303531H|                                       
                | EDX = 007A484DH                            |                                       

### Extracting the Maximum Processor Frequency from Brand Strings
Figure 3-9 provides an algorithm which software can use to extract the maximum
processor operating frequency from the processor brand string.


<aside class="notification">
NOTE:
When a frequency is given in a brand string, it is the maximum qualified frequency
of the processor, not the frequency at which the processor is currently running.
</aside>

   | |  
---- | -----
 Figure 3-9.| Algorithm for Extracting Maximum Processor
            | Frequency                                 

### The Processor Brand Index Method
The brand index method (introduced with Pentium® III Xeon® processors) provides
an entry point into a brand identification table that is maintained in memory
by system software and is accessible from system- and user-level code. In this
table, each brand index is associate with an ASCII brand identification string
that identifies the official Intel family and model number of a processor.

When CPUID executes with EAX set to 1, the processor returns a brand index to
the low byte in EBX. Software can then use this index to locate the brand identification
string for the processor in the brand identification table. The first entry
(brand index 0) in this table is reserved, allowing for backward compatibility
with processors that do not support the brand identification feature. Starting
with processor signature family ID = 0FH, model = 03H, brand index method is
no longer supported. Use brand string method instead.

Table 3-24 shows brand indices that have identification strings associated with
them.


### Table 3-24. Mapping of Brand Indices; and Intel 64 and IA-32 Processor Brand Strings
   | |  
---- | -----
 Brand Index This processor does not         | Brand String                         
 support the brand identification feature    |                                      
 Intel(R) Celeron(R) processor1 Intel(R)     |                                      
 Pentium(R) III processor1                   |                                      
 Table 3-24. Intel(R) Pentium(R) III         | Mapping of Brand Indices; and Intel  
 Xeon(R) processor; If processor signature   | 64 and IA-32 Processor Brand Strings 
 = 000006B1h, then Intel(R) Celeron(R)       | NOTES: 1. Indicates versions of these
 processor Intel(R) Pentium(R) III processor | processors that were introduced after
 Mobile Intel(R) Pentium(R) III processor-M  | the Pentium III                      
 Mobile Intel(R) Celeron(R) processor1       |                                      
 Intel(R) Pentium(R) 4 processor Intel(R)    |                                      
 Pentium(R) 4 processor Intel(R) Celeron(R)  |                                      
 processor1 Intel(R) Xeon(R) processor;      |                                      
 If processor signature = 00000F13h,         |                                      
 then Intel(R) Xeon(R) processor MP Intel(R) |                                      
 Xeon(R) processor MP Mobile Intel(R)        |                                      
 Pentium(R) 4 processor-M; If processor      |                                      
 signature = 00000F13h, then Intel(R)        |                                      
 Xeon(R) processor Mobile Intel(R) Celeron(R)|                                      
 processor1 Mobile Genuine Intel(R) processor|                                      
 Intel(R) Celeron(R) M processor Mobile      |                                      
 Intel(R) Celeron(R) processor1 Intel(R)     |                                      
 Celeron(R) processor Mobile Genuine         |                                      
 Intel(R) processor Intel(R) Pentium(R)      |                                      
 M processor Mobile Intel(R) Celeron(R)      |                                      
 processor1 RESERVED                         |                                      

### IA-32 Architecture Compatibility
CPUID is not supported in early models of the Intel486 processor or in any IA-32
processor earlier than the Intel486 processor.



###   EAX = 0
     EAX <- Highest basic function input value understood by CPUID;
     EBX <- Vendor identification string;
     EDX <- Vendor identification string;
     ECX <- Vendor identification string;
  BREAK;
###   EAX = 1H
     EAX[3:0] <- Stepping ID;
     EAX[7:4] <- Model;
     EAX[11:8] <- Family;
     EAX[13:12] <- Processor type;
     EAX[15:14] <- Reserved;
     EAX[19:16] <- Extended Model;
     EAX[27:20] <- Extended Family;
     EAX[31:28] <- Reserved;
     EBX[7:0] <- Brand Index; (\* Reserved if the value is zero. \*)
     EBX[15:8] <- CLFLUSH Line Size;
     EBX[16:23] <- Reserved; (\* Number of threads enabled = 2 if MT enable fuse set. \*)
     EBX[24:31] <- Initial APIC ID;
     ECX <- Feature flags; (\* See Figure 3-6. \*)
     EDX <- Feature flags; (\* See Figure 3-7. \*)
  BREAK;
###   EAX = 2H
     EAX <- Cache and TLB information;
     EBX <- Cache and TLB information;
     ECX <- Cache and TLB information;
     EDX <- Cache and TLB information;
  BREAK;
###   EAX = 3H
     EAX <- Reserved;
     EBX <- Reserved;
     ECX <- ProcessorSerialNumber[31:0];
     (\* Pentium III processors only, otherwise reserved. \*)
     EDX <- ProcessorSerialNumber[63:32];
     (\* Pentium III processors only, otherwise reserved. \*
  BREAK
###   EAX = 4H
     EAX <- Deterministic Cache Parameters Leaf; (\* See Table 3-17. \*)
     EBX <- Deterministic Cache Parameters Leaf;
     ECX <- Deterministic Cache Parameters Leaf;
     EDX <- Deterministic Cache Parameters Leaf;
  BREAK;
###   EAX = 5H
     EAX <- MONITOR/MWAIT Leaf; (\* See Table 3-17. \*)
     EBX <- MONITOR/MWAIT Leaf;
     ECX <- MONITOR/MWAIT Leaf;
     EDX <- MONITOR/MWAIT Leaf;
  BREAK;
###   EAX = 6H
     EAX <- Thermal and Power Management Leaf; (\* See Table 3-17. \*)
     EBX <- Thermal and Power Management Leaf;
     ECX <- Thermal and Power Management Leaf;
     EDX <- Thermal and Power Management Leaf;
  BREAK;
###   EAX = 7H
     EAX <- Structured Extended Feature Flags Enumeration Leaf; (\* See Table 3-17. \*)
     EBX <- Structured Extended Feature Flags Enumeration Leaf;
     ECX <- Structured Extended Feature Flags Enumeration Leaf;
     EDX <- Structured Extended Feature Flags Enumeration Leaf;
  BREAK;
###   EAX = 8H
     EAX <- Reserved = 0;
     EBX <- Reserved = 0;
     ECX <- Reserved = 0;
     EDX <- Reserved = 0;
  BREAK;
###   EAX = 9H
     EAX <- Direct Cache Access Information Leaf; (\* See Table 3-17. \*)
     EBX <- Direct Cache Access Information Leaf;
     ECX <- Direct Cache Access Information Leaf;
     EDX <- Direct Cache Access Information Leaf;
  BREAK;
###   EAX = AH
     EAX <- Architectural Performance Monitoring Leaf; (\* See Table 3-17. \*)
     EBX <- Architectural Performance Monitoring Leaf;
     ECX <- Architectural Performance Monitoring Leaf;
     EDX <- Architectural Performance Monitoring Leaf;
     BREAK
###   EAX = BH
     EAX <- Extended Topology Enumeration Leaf; (\* See Table 3-17. \*)
     EBX <- Extended Topology Enumeration Leaf;
     ECX <- Extended Topology Enumeration Leaf;
     EDX <- Extended Topology Enumeration Leaf;
  BREAK;
###   EAX = CH
     EAX <- Reserved = 0;
     EBX <- Reserved = 0;
     ECX <- Reserved = 0;
     EDX <- Reserved = 0;
  BREAK;
###   EAX = DH
     EAX <- Processor Extended State Enumeration Leaf; (\* See Table 3-17. \*)
     EBX <- Processor Extended State Enumeration Leaf;
     ECX <- Processor Extended State Enumeration Leaf;
     EDX <- Processor Extended State Enumeration Leaf;
  BREAK;
###   EAX = EH
     EAX <- Reserved = 0;
     EBX <- Reserved = 0;
     ECX <- Reserved = 0;
     EDX <- Reserved = 0;
  BREAK;
###   EAX = FH
     EAX <- Platform Quality of Service Monitoring Enumeration Leaf; (\* See Table 3-17. \*)
     EBX <- Platform Quality of Service Monitoring Enumeration Leaf;
     ECX <- Platform Quality of Service Monitoring Enumeration Leaf;
     EDX <- Platform Quality of Service Monitoring Enumeration Leaf;
  BREAK;
###   EAX = 10H
     EAX <- Platform Quality of Service Enforcement Enumeration Leaf; (\* See Table 3-17. \*)
     EBX <- Platform Quality of Service Enforcement Enumeration Leaf;
     ECX <- Platform Quality of Service Enforcement Enumeration Leaf;
     EDX <- Platform Quality of Service Enforcement Enumeration Leaf;
  BREAK;
BREAK;
###   EAX = 80000000H
     EAX <- Highest extended function input value understood by CPUID;
     EBX <- Reserved;
     ECX <- Reserved;
     EDX <- Reserved;
  BREAK;
###   EAX = 80000001H
     EAX <- Reserved;
     EBX <- Reserved;
     ECX <- Extended Feature Bits (\* See Table 3-17.\*);
     EDX <- Extended Feature Bits (\* See Table 3-17. \*);
  BREAK;
###   EAX = 80000002H
     EAX <- Processor Brand String;
     EBX <- Processor Brand String, continued;
     ECX <- Processor Brand String, continued;
     EDX <- Processor Brand String, continued;
  BREAK;
###   EAX = 80000003H
     EAX <- Processor Brand String, continued;
     EBX <- Processor Brand String, continued;
     ECX <- Processor Brand String, continued;
     EDX <- Processor Brand String, continued;
  BREAK;
###   EAX = 80000004H
     EAX <- Processor Brand String, continued;
     EBX <- Processor Brand String, continued;
     ECX <- Processor Brand String, continued;
     EDX <- Processor Brand String, continued;
  BREAK;
###   EAX = 80000005H
     EAX <- Reserved = 0;
     EBX <- Reserved = 0;
     ECX <- Reserved = 0;
     EDX <- Reserved = 0;
  BREAK;
###   EAX = 80000006H
     EAX <- Reserved = 0;
     EBX <- Reserved = 0;
     ECX <- Cache information;
     EDX <- Reserved = 0;
  BREAK;
###   EAX = 80000007H
     EAX <- Reserved = 0;
     EBX <- Reserved = 0;
     ECX <- Reserved = 0;
     EDX <- Reserved = Misc Feature Flags;
  BREAK;
###   EAX = 80000008H
     EAX <- Reserved = Physical Address Size Information;
     EBX <- Reserved = Virtual Address Size Information;
     ECX <- Reserved = 0;
     EDX <- Reserved = 0;
  BREAK;
###   EAX >= 40000000H and EAX <= 4FFFFFFFH
  DEFAULT: (\* EAX = Value outside of recognized range for CPUID. \*)
     (\* If the highest basic information leaf data depend on ECX input value, ECX is honored.\*)
     EAX <- Reserved; (\* Information returned for highest basic information leaf. \*)
     EBX <- Reserved; (\* Information returned for highest basic information leaf. \*)
     ECX <- Reserved; (\* Information returned for highest basic information leaf. \*)
     EDX <- Reserved; (\* Information returned for highest basic information leaf. \*)
  BREAK;
ESAC;

### Flags Affected
None.


### Exceptions (All Operating Modes)
   | |  
---- | -----
 **``#UD``**| If the LOCK prefix is used. In earlier 
    | IA-32 processors that do not support   
    | the CPUID instruction, execution of    
    | the instruction results in an invalid  
    | opcode (**``#UD)``** exception being generated.
