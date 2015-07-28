## MOVNTPD - Store Packed Double-Precision Floating-Point Values Using Non-Temporal Hint

> Operation
``` slim

DEST <- SRC;

```

 Opcode/Instruction                    | Op/En| 64/32-bit Mode| CPUID Feature Flag| Description                                
 ---  | --- | --- | --- | ---
 66 0F 2B /r MOVNTPD m128, xmm         | MR   | V/V           | SSE2              | Move packed double-precision floating-point
                                       |      |               |                   | values from xmm to m128 using nontemporal  
                                       |      |               |                   | hint.                                      
 VEX.128.66.0F.WIG 2B /r VMOVNTPD m128,| MR   | V/V           | AVX               | Move packed double-precision values        
 xmm1                                  |      |               |                   | in xmm1 to m128 using non-temporal hint.   
 VEX.256.66.0F.WIG 2B /r VMOVNTPD m256,| MR   | V/V           | AVX               | Move packed double-precision values        
 ymm1                                  |      |               |                   | in ymm1 to m256 using non-temporal hint.   

### Instruction Operand Encoding1
 Op/En| Operand 1    | Operand 2    | Operand 3| Operand 4
 ---  | --- | --- | --- | ---
 MR   | ModRM:r/m (w)| ModRM:reg (r)| NA       | NA       

### Description
Moves the packed double-precision floating-point values in the source operand
(second operand) to the destination operand (first operand) using a non-temporal
hint to prevent caching of the data during the write to memory. The source operand
is an XMM register or YMM register, which is assumed to contain packed double-precision,
floatingpointing data. The destination operand is a 128-bit or 256-bit memory
location. The memory operand must be aligned on a 16-byte (128-bit version)
or 32-byte (VEX.256 encoded version) boundary otherwise a generalprotection
exception (#GP) will be generated.

The non-temporal hint is implemented by using a write combining (WC) memory
type protocol when writing the data to memory. Using this protocol, the processor
does not write the data into the cache hierarchy, nor does it fetch the corresponding
cache line from memory into the cache hierarchy. The memory type of the region
being written to can override the non-temporal hint, if the memory address specified
for the non-temporal store is in an uncacheable (UC) or write protected (WP)
memory region. For more information on non-temporal stores, see “Caching of
Temporal vs. Non-Temporal Data” in Chapter 10 in the Intel® 64 and IA-32 Architectures
Software Developer's Manual, Volume 1.

Because the WC protocol uses a weakly-ordered memory consistency model, a fencing
operation implemented with the SFENCE or MFENCE instruction should be used in
conjunction with MOVNTPD instructions if multiple processors might use different
memory types to read/write the destination memory locations.

In 64-bit mode, use of the REX.R prefix permits this instruction to access additional
registers (XMM8-XMM15). Note: In VEX-128 encoded versions, VEX.vvvv is reserved
and must be 1111b, VEX.L must be 0; otherwise instructions will #UD.



### Intel C/C++ Compiler Intrinsic Equivalent
   | |  
---- | -----
 MOVNTPD: | void _mm_stream_pd(double \*p, __m128d     
          | a)                                        
 VMOVNTPD:| void _mm256_stream_pd (double \* p, __m256d
          | a);                                       

### SIMD Floating-Point Exceptions
None.

   | |  
---- | -----
 1.| ModRM.MOD = 011B is not permitted

### Other Exceptions
See Exceptions Type 1.SSE2; additionally

   | |  
---- | -----
 #UD| If VEX.vvvv != 1111B.
