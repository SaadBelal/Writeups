# Write-up: Reversing a Flag Scrambler Using IDA Pro

## Initial Discovery

The file `mystery.png` was opened using the hex editor (HxD)

Inside the file, the following suspicious string appeared:

```text
picoCTK€k5zsid6q_6099dfbd}
```

The structure strongly resembled a partially scrambled picoCTF flag.

---

# Challenge Overview

The target binary reads a 26-byte flag from `flag.txt` and writes a modified version of it into `mystery.png`.

The transformation logic consists of:

- Direct byte copies
- A loop that adds `+5` to several characters
- A single-byte subtraction of `-3`

To recover the original flag, the binary was analyzed statically using :contentReference[oaicite:1]{index=1}.

---

# Static Analysis

Unlike some decompilers that struggle with stack-allocated arrays, IDA Pro’s Hex-Rays decompiler successfully reconstructed the contiguous stack variables into a single array:

```c
_BYTE ptr[40];
```

This made the transformation logic very easy to follow.

---

# Decompiled Logic

Relevant pseudocode from `main`:

```c
puts("at insert");

fputc(ptr[0], v8);
fputc(ptr[1], v8);
fputc(ptr[2], v8);
fputc(ptr[3], v8);
fputc(ptr[4], v8);
fputc(ptr[5], v8);

for ( i = 6; i <= 14; ++i )
    fputc((char)(ptr[i] + 5), v8);

fputc((char)(ptr[15] - 3), v8);

for ( j = 16; j <= 25; ++j )
    fputc((char)ptr[j], v8);
```

---

# Deconstructing the Transformation

The flag is processed in four distinct sections.

## Indices 0–5

Characters are copied directly without modification.

This corresponds to:

```text
picoCT
```

---

## Indices 6–14

A loop applies:

\[
\text{char} + 5
\]

to each byte.

---

## Index 15

A single character is modified separately:

\[
\text{char} - 3
\]

---

## Indices 16–25

Characters are copied directly again.

This corresponds to the trailing section:

```text
_6099dfbd}
```

---

# Reversing the Scrambled Flag

Recovered scrambled string:

```text
picoCTK€k5zsid6q_6099dfbd}
```

To restore the original flag:

- Characters at indices `6–14` must be shifted back by `-5`
- Character at index `15` must be shifted forward by `+3`

---

# Byte Transformation Table

| Index | Scrambled | Reverse Operation | Decoded |
|------|------|------|------|
| 0–5 | `picoCT` | None | `picoCT` |
| 6 | `K` | `-5` | `F` |
| 7 | `€` | `-5` | `{` |
| 8 | `k` | `-5` | `f` |
| 9 | `5` | `-5` | `0` |
| 10 | `z` | `-5` | `u` |
| 11 | `s` | `-5` | `n` |
| 12 | `i` | `-5` | `d` |
| 13 | `d` | `-5` | `_` |
| 14 | `6` | `-5` | `1` |
| 15 | `q` | `+3` | `t` |
| 16–25 | `_6099dfbd}` | None | `_6099dfbd}` |

---

# Final Flag

```text
picoCTF{f0und_1t_6099dfbd}
```
