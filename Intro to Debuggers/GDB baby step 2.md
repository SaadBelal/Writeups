# GDB Baby Step 2 

## Objective

Determine the value stored in the `EAX` register at the end of the `main` function using breakpoints and runtime inspection.

---

## Step 1: Download the Challenge Binary

Retrieve the file:

```bash
wget https://artifacts.picoctf.net/c/520/debugger0_b
```

---

## Step 2: Prepare and Launch GDB

```bash
chmod +x ./debugger0_b
gdb ./debugger0_b
```

---

## Step 3: Disassemble the `main` Function

Inside GDB:

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
0x0000000000401115 <+15>:    movl   $0x1e0da,-0x4(%rbp)
0x000000000040111c <+22>:    movl   $0x25f,-0xc(%rbp)
0x0000000000401123 <+29>:    movl   $0x0,-0x8(%rbp)
0x000000000040112a <+36>:    jmp    0x401136 <main+48>
0x000000000040112c <+38>:    mov    -0x8(%rbp),%eax
0x000000000040112f <+41>:    add    %eax,-0x4(%rbp)
0x0000000000401132 <+44>:    addl   $0x1,-0x8(%rbp)
0x0000000000401136 <+48>:    mov    -0x8(%rbp),%eax
0x0000000000401139 <+51>:    cmp    -0xc(%rbp),%eax
0x000000000040113c <+54>:    jl     0x40112c <main+38>
0x000000000040113e <+56>:    mov    -0x4(%rbp),%eax
0x0000000000401141 <+59>:    pop    %rbp
0x0000000000401142 <+60>:    ret
```

---

## Analysis

Key variables:

* `-0x4(%rbp)` → accumulator (final result)
* `-0x8(%rbp)` → loop counter
* `-0xc(%rbp)` → loop limit (`0x25f` = 607)

The loop:

* Initializes counter to `0`
* Iterates while counter < 607
* Adds counter value to accumulator each iteration

This represents a summation loop:

```
accumulator = 0x1e0da + (0 + 1 + 2 + ... + 606)
```

Rather than computing manually, runtime inspection is more efficient.

---

## Step 4: Set Breakpoint at Function Epilogue

Set a breakpoint just before returning from `main`:

```gdb
break *main+59
```

---

## Step 5: Execute the Program

```gdb
run
```

Execution pauses at:

```
Breakpoint 1, 0x0000000000401141 in main ()
```

---

## Step 6: Inspect Register Value

```gdb
info registers eax
```

Output:

```
eax            0x4af4b             307019
```

---

## Final Answer

* EAX (Hex): `0x4af4b`
* EAX (Decimal): `307019`

---

## Flag

```
picoCTF{307019}
```

---

## Notes

* Breakpoints allow inspection of register state at precise execution points.
* The value returned by `main` is stored in `EAX`.
* Loop-based calculations can be validated dynamically instead of manually computing large sums.
