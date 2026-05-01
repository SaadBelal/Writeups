# Bit O Asm-2

### Level Goal
This one teaches us operations, like multiplication and addition in assembly.

### 1. Download the File
Download the assembly dump using the following command:
`https://artifacts.picoctf.net/c/530/disassembler-dump0_c.txt`

### 2. View the Assembly Code
If we examine the code, we see how the value is handled across different instructions:
```assembly
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0xc],0x9fe1a    ; [rbp-0xc] = 0x9fe1a (654874 in decimal)
<+22>:    mov    DWORD PTR [rbp-0x8],0x4        ; [rbp-0x8] = 0x4 (4 in decimal) 
<+29>:    mov    eax,DWORD PTR [rbp-0xc]        ; eax = [rbp-0xc] => 0x9fe1a (654874 in decimal)
<+32>:    imul   eax,DWORD PTR [rbp-0x8]        ; eax =  eax * [rbp-0x8]
<+36>:    add    eax,0x1f5                      ; eax = eax + 0x1f5 
<+41>:    mov    DWORD PTR [rbp-0x4],eax        ; [rbp-0x4] = eax 
<+44>:    mov    eax,DWORD PTR [rbp-0x4]        ; eax = [rbp-0x4]
<+47>:    pop    rbp
<+48>:    ret
```
### 3. Initialize the Variables

First, the code places two values into memory:

Line <+15>: The value 0x9fe1a (Decimal: 654874) is stored at [rbp-0xc].

Line <+22>: The value 0x4 (Decimal: 4) is stored at [rbp-0x8].

### 4. Move to RegisterLine

<+29>: The value from [rbp-0xc] is moved into the eax register.

eax = 6548743.

### 5. Addition (add)

Line <+36>: Add the hexadecimal value 0x1f5 to eax.

Convert 0x1f5 to decimal: 501.

eax = 2,619,496 + 501

eax = 2,619,997


### 6. Final Move
Line <+41> and <+44>: The result is moved to a temporary memory spot and then moved back into eax.

The value remains the same.

Flag: picoCTF{2619997}
