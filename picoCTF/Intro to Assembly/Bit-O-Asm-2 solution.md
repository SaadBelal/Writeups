# Bit O Asm-2

**Level Goal:** 
This one have the same solution and steps as the last one

**Step 1: Download the file**
Download the file using the command: 
`wget https://artifacts.picoctf.net/c/510/disassembler-dump0_b.txt`

```assembly
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a    ; 0x9fe1a is copied on the address [rbp-0x4]
<+22>:    mov    eax,DWORD PTR [rbp-0x4]        ; same value is copied into eax
<+25>:    pop    rbp
<+26>:    ret
```

This one should teach that a value can be pointed to another address,
in this case we want the value in EAX, but the value is actually a line above, that's because assembly can have relative addresses. The address is RBP
The solution is the decimal value of 0x9fe1a is (654874 in decimal) is pointed into eax , so eax have that value.

Flag: picoCTF{654874}
