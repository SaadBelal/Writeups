# GDB baby step 2

## Objective
same as the last one, but this time, we are playing with breakpoints and loops in the program


## Step 1: Download the Challenge Binary

### 1. Download the File
Download the assembly dump using the following command: `wget https://artifacts.picoctf.net/c/520/debugger0_b`

```bash
chmod +x ./debugger0_b
gdb ./debugger0_b
```

---

## Step 3: Disassemble the `main` Function

Inside GDB, run:

```gdb
disassemble main
```

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

and inside gdb we set up a breakpoint, where it will pause the execution:

```gdb
(gdb) break *main+59
```

And run the program normally through the GDB debugger:

```gdb
run
```
it will stop with the following message:

```gdb
Breakpoint 1, 0x0000000000401141 in main ()
```
