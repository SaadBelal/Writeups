# GDB Baby Step 4 — picoCTF Writeup

## Objective

Identify the constant value used to multiply the `EAX` register by analyzing function

---

## Step 1: Download the Challenge Binary

```bash id="p1o2as"
wget https://artifacts.picoctf.net/c/532/debugger0_d
```

---

## Step 2: Prepare and Launch GDB

```bash id="z8k2lm"
chmod +x ./debugger0_d
gdb ./debugger0_d
```

---

## Step 3: Disassemble the `main` Function

```gdb id="v9x1qd"
disassemble main
```

### Disassembly Output

```assembly id="a72jsk"
0x000000000040111c <+0>:     endbr64
0x0000000000401120 <+4>:     push   %rbp
0x0000000000401121 <+5>:     mov    %rsp,%rbp
0x0000000000401124 <+8>:     sub    $0x20,%rsp
0x0000000000401128 <+12>:    mov    %edi,-0x14(%rbp)
0x000000000040112b <+15>:    mov    %rsi,-0x20(%rbp)
0x000000000040112f <+19>:    movl   $0x28e,-0x4(%rbp)
0x0000000000401136 <+26>:    movl   $0x0,-0x8(%rbp)
0x000000000040113d <+33>:    mov    -0x4(%rbp),%eax
0x0000000000401140 <+36>:    mov    %eax,%edi
0x0000000000401142 <+38>:    call   0x401106 <func1>
0x0000000000401147 <+43>:    mov    %eax,-0x8(%rbp)
0x000000000040114a <+46>:    mov    -0x4(%rbp),%eax
0x000000000040114d <+49>:    leave
0x000000000040114e <+50>:    ret
```

---

## Analysis of `main`

* A value `0x28e` is stored and later moved into `EAX`.
* That value is passed as an argument to another function:

  ```assembly id="b3k2df"
  call 0x401106 <func1>
  ```
* The result of this function is stored, but not returned by `main`.

To understand the multiplication, the called function must be analyzed.

---

## Step 4: Disassemble `func1`

```gdb id="c8lmwq"
disassemble func1
```

### Disassembly Output

```assembly id="d91xpo"
0x0000000000401106 <+0>:     endbr64
0x000000000040110a <+4>:     push   %rbp
0x000000000040110b <+5>:     mov    %rsp,%rbp
0x000000000040110e <+8>:     mov    %edi,-0x4(%rbp)
0x0000000000401111 <+11>:    mov    -0x4(%rbp),%eax
0x0000000000401114 <+14>:    imul   $0x3269,%eax,%eax
0x000000000040111a <+20>:    pop    %rbp
0x000000000040111b <+21>:    ret
```

---

## Analysis of `func1`

The critical instruction:

```assembly id="k29xms"
imul $0x3269, %eax, %eax
```

* Performs signed multiplication.
* Multiplies the value in `EAX` by the constant `0x3269`.
* Stores the result back into `EAX`.

This constant is the required value.

---

## Final Answer

* Constant (Hex): `0x3269`
* Constant (Decimal): `12905`

---

## Flag

```id="qwe91m"
picoCTF{12905}
```


