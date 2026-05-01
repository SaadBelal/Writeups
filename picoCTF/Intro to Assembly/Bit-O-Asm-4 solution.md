# Bit O Asm-4 

### Level Goal
This challenge introduces comparisons and branching logic in assembly. It demonstrates how code executes different paths (like if/else statements) using jumps and condition checks.  

### 1. Download the File
Download the assembly dump using the following command: `wget https://artifacts.picoctf.net/c/511/disassembler-dump0_d.txt`


### 2. The Assembly Breakdown
Below is the annotated code showing the execution flow:

```assembly
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a    ; [rbp-0x4] = 0x9fe1a
<+22>:    cmp    DWORD PTR [rbp-0x4],0x2710     ; Compare 0x9fe1a to 0x2710
<+29>:    jle    0x55555555514e <main+37>       ; If (0x9fe1a <= 0x2710) jump to <+37>
<+31>:    sub    DWORD PTR [rbp-0x4],0x65       ; Else: Subtract 0x65
<+35>:    jmp    0x555555555152 <main+41>       ; Jump to the end
<+37>:    add    DWORD PTR [rbp-0x4],0x65       ; (Skipped in this case)
<+41>:    mov    eax,DWORD PTR [rbp-0x4]        ; Move final value to eax
<+44>:    pop    rbp
<+45>:    ret
```

### 3. Step-by-Step Logic
Assembly logic can be thought of as a high-level `if/else` statement:

*   **The Initialization:** At line `<+15>`, the memory address `[rbp-0x4]` is set to `0x9fe1a`.[
*   **The Comparison (`cmp`):** Line `<+22>` acts like an `if` condition. It compares our value (`0x9fe1a`) against `0x2710`.
*   **The Branch (`jle`):** `jle` stands for **Jump if Less or Equal**. Since `0x9fe1a` (654,874) is **not** less than or equal to `0x2710` (10,000), the jump does **not** happen.
*   **The Subtraction (`sub`):** Because the jump was skipped, the code moves to line `<+31>` and subtracts `0x65` from our value.
*   **The Final Jump:** Line `<+35>` is a `jmp` (unconditional jump), which tells the code to skip the `add` instruction and go straight to the end at `<+41>`.

### 4. Final Calculation
1.  **Initial Value:** `0x9fe1a`
2.  **Operation:** `0x9fe1a - 0x65 = 0x9fdb5
3.  **Decimal Conversion:** `0x9fdb5` in decimal is **654773**.

Flag: picoCTF{654773}
