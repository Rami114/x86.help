## AESIMG - Perform the AES InvMixColumn Transformation
> Operation 

``` slim
// AESIMC
DEST[127:0] <- InvMixColumns( SRC );
DEST[VLMAX-1:128] (Unmodified)

// VAESIMC 
DEST[127:0] <- InvMixColumns( SRC );
DEST[VLMAX-1:128] <- 0;
```

> Intel C/C++ Compiler Intrinsic Equivalent

``` c
// (V)AESIMC
__m128i _mm_aesimc (__m128i)
```

Opcode / Instruction | Op/En | 64/32bit Mode Support | CPUID Feature Flag | Description
-------------------- | ----- | ----------- | --------------- | -----------
66 0F 38 DB /r AESIMC xmm1, xmm2/m128  | RM   | V/V | AES | Perform the InvMixColumn transformation on a 128-bit round key from xmm2/m128and store the result in xmm1.
 VEX.128.66.0F38.WIG DB /r VAESIMC xmm1,xmm2/m128 | RM   | V/V | Both AES and AVX flags| Perform the InvMixColumn transformation on a 128-bit Round key from xmm2/m128 and store the result in xmm1.
 
### Instruction Operand Encoding
Op/En  | Operand 1  | Operand 2  | Operand 3  | Operand 4
------ | ---------- | ---------- | ---------- | ---------
RM | ModRM:reg (r, w) | ModRM:r/m (r ) | NA | NA

### Description
Perform the InvMixColumns transformation on the source operand and store the result in the destination operand. 
The destination operand is an XMM register. The source operand can be an XMM register or a 128-bit memory location.

<aside class="notification">
the AESIMC instruction should be applied to the expanded AES round keys (except
for the first and last round key) in order to prepare them for decryption using
the “Equivalent Inverse Cipher” (defined in FIPS 197).
</aside>

128-bit Legacy SSE version: Bits (VLMAX-1:128) of the corresponding YMM destination register remain unchanged. 

VEX.128 encoded version: Bits (VLMAX-1:128) of thedestination YMM register are zeroed.

<aside class="notification">
In VEX-encoded versions, VEX.vvvv is reserved and must be 1111b, otherwise instructions will #UD.
</aside>

### SIMD Floating-Point Exceptions
None

### Other Exceptions
See Exceptions Type 4; additionally

      | | 
 ---- | -----
 **`#UD`**   |  If VEX.vvvv ≠ 1111B.
