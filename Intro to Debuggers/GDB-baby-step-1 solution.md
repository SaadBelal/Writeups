# GDB Baby Step 1

### Objective

Determine the value stored in the `EAX` register at the end of the `main` function.

---

## Step 1: Download the Challenge Binary

### 1. Download the File
Download the assembly dump using the following command: `wget https://artifacts.picoctf.net/c/512/debugger0_a`


---

##  Step 2: Prepare and Launch GDB

Make the binary executable and open it with GDB:

```bash
chmod +x ./debugger0_a
gdb ./debugger0_a
```

---

## Step 3: Disassemble the `main` Function

Inside GDB, run:

```gdb
disassemble main
```

### Disassembly Output

```assembly
0x0000000000001129 <+0>:    endbr64
0x000000000000112d <+4>:    push   %rbp
0x000000000000112e <+5>:    mov    %rsp,%rbp
0x0000000000001131 <+8>:    mov    %edi,-0x4(%rbp)
0x0000000000001134 <+11>:   mov    %rsi,-0x10(%rbp)
0x0000000000001138 <+15>:   mov    $0x86342,%eax
0x000000000000113d <+20>:   pop    %rbp
0x000000000000113e <+21>:   ret
```

---

## Analysis

The critical instruction is:

```assembly
mov $0x86342, %eax
```

* This moves the immediate value `0x86342` directly into the `EAX` register.
* No further modifications to `EAX` occur before the function returns.

---

## Final Answer

* **EAX Value (Hex):** `0x86342`
* **EAX Value (Decimal):** `549698`

---

## 🚩 Flag

```
picoCTF{549698}
```

