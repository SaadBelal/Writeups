# Bit O Asm-2

### Level Goal
Find the value stored in the `eax` register. This challenge introduces how values are moved between memory addresses and registers.

### 1. Download the File
Download the assembly dump using the following command:
`wget https://artifacts.picoctf.net/c/510/disassembler-dump0_b.txt`

### 2. View the Assembly Code
If we examine the code, we see how the value is handled across different instructions:
```assembly
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a    ; The value 0x9fe1a is stored at the address [rbp-0x4]
<+22>:    mov    eax,DWORD PTR [rbp-0x4]        ; The value at [rbp-0x4] is moved into the eax register
<+25>:    pop    rbp
<+26>:    ret
3. Analysis
This challenge demonstrates that values aren't always moved directly into a register. Instead, assembly often uses relative addresses or memory pointers:

At <+15>, the hexadecimal value 0x9fe1a is first moved into a memory location on the stack: [rbp-0x4].

At <+22>, that same value is then copied from memory into the eax register.

4. Conversion
To find the flag, we convert the hexadecimal value 0x9fe1a to a decimal integer:

Hex: 0x9fe1a

Decimal: 654874

Flag
picoCTF{654874}
