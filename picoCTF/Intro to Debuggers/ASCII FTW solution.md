# ASCII FTW — picoCTF Writeup

## Objective

Extract the flag stored as raw byte values in memory and interpret it as an ASCII string.

---

## Step 1: Disassemble the `main` Function

```gdb id="u1xk3q"
disassemble main
```

### Key Observation

A sequence of `movb` instructions writes individual byte values into memory:

```assembly id="p9z2lm"
movb $0x70,-0x30(%rbp)
movb $0x69,-0x2f(%rbp)
movb $0x63,-0x2e(%rbp)
movb $0x6f,-0x2d(%rbp)
...
movb $0x7d,-0x12(%rbp)
```

Each instruction stores a single byte. These bytes correspond to ASCII characters.

---

## Analysis

* The memory region starting at `$rbp-0x30` is filled sequentially with bytes.
* Each byte represents one ASCII character.
* Together, they form a contiguous string in memory.

Examples:

* `0x70` → `p`
* `0x69` → `i`
* `0x63` → `c`
* `0x6f` → `o`

This pattern continues, constructing the full flag.

---

## Step 2: Set Breakpoint After Memory Initialization &  Run the Program

```gdb id="g7k2ds"
break *main+151
run
```

This location is chosen because all bytes have already been written to memory.

Execution pauses at the breakpoint.

---

## Step 3: Examine Memory as String

```gdb
x/1sb $rbp-0x30
```

* `x` → examine memory
* `/1` → read one unit
* `s` → interpret as string
* `b` → byte-based access

---

## Output

```
"picoCTF{ASCII_IS_EASY_7BCD971D}"
```



