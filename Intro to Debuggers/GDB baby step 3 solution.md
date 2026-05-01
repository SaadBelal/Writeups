# GDB Baby Step 3 — picoCTF Writeup

## Objective

Extract the raw bytes stored at a specific memory location during execution.

---

## Step 1: Download the Challenge Binary

```bash
wget https://artifacts.picoctf.net/c/531/debugger0_c
```

---

## Step 2: Prepare and Launch GDB

```bash
chmod +x ./debugger0_c
gdb ./debugger0_c
```

---

## Step 3: Disassemble the `main` Function

```gdb
disassemble main
```

### Disassembly Output

```assembly
0x0000000000401106 <+0>:     endbr64
0x000000000040110a <+4>:     push   %rbp
0x000000000040110b <+5>:     mov    %rsp,%rbp
0x000000000040110e <+8>:     mov    %edi,-0x14(%rbp)
0x0000000000401111 <+11>:    mov    %rsi,-0x20(%rbp)
0x0000000000401115 <+15>:    movl   $0x2262c96b,-0x4(%rbp)
0x000000000040111c <+22>:    mov    -0x4(%rbp),%eax
0x000000000040111f <+25>:    pop    %rbp
0x0000000000401120 <+26>:    ret
```

---

## Analysis

The instruction:

```assembly
movl $0x2262c96b, -0x4(%rbp)
```

stores the 4-byte value `0x2262c96b` into memory at address `rbp - 0x4`.

Key detail: x86-64 architecture uses **little-endian** byte ordering.
This means the least significant byte is stored first in memory.

So the value:

```
0x2262c96b
```

is laid out in memory as:

```
0x6b 0xc9 0x62 0x22
```

---

## Step 4: Set Breakpoint Before Function Returns

```gdb
break *main+25
```

This breakpoint is placed after the value is written to memory but before the function exits, ensuring the data is still accessible.

---

## Step 5: Run the Program

```gdb
run
```

---

## Step 6: Examine Memory

```gdb
x/4xb $rbp-0x4
```

Output:

```text
0x7fffffffddbc: 0x6b    0xc9    0x62    0x22
```

---

## Explanation

* `$rbp-0x4` points to the exact memory location where the value was stored.
* `x/4xb` means:

  * `x` → examine memory
  * `/4` → display 4 units
  * `x` → format in hexadecimal
  * `b` → unit size is 1 byte

This command reveals the raw byte representation in memory rather than the full integer value.

---

## Final Answer

Reconstructing the bytes in the observed order:

```
0x6bc96222
```

---

## Flag

```
picoCTF{0x6bc96222}
```

