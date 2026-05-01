# Bit O Asm-1

**Level Goal:** 
We have to find the value of eax.

**Step 1: Download the file**
Download the file using the command: 
`wget https://artifacts.picoctf.net/c/509/disassembler-dump0_a.txt`

**Step 2: View the assembly code**
If we cat the assembly code, we see the following instructions:

```assembly
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret
```

Step 3: Analyze the result
From the code above, we see that the eax register has the value 0x30.

Step 4: Convert to Decimal
With a simple search, we see that 0x30 = 48.

Flag:
picoCTF{48}
