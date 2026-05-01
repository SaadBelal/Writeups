Bit O Asm-1
Level Goal
Find the value of the eax register.

1. Download the file
Use the following command to download the assembly dump:
wget [https://artifacts.picoctf.net/c/509/disassembler-dump0_a.txt](https://artifacts.picoctf.net/c/509/disassembler-dump0_a.txt)

2. View the code
If we cat the assembly code, we see the following instructions:

Code snippet
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret
3. Solve
We can see that eax is assigned the value 0x30.

Hex: 0x30

Decimal: 48

Flag: picoCTF{48}
