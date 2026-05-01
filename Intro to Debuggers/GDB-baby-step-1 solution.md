# GDB baby step 1

### Level Goal
Gather the value of eax register at the end of main function

### 1. Download the File
Download the assembly dump using the following command: `wget https://artifacts.picoctf.net/c/512/debugger0_a`

after downloaded the file, we can disassemble the main function with the following commands

```bash
chmod +x ./debugger0_a
gdb ./debugger0_a
```
```(gdb)
disassemble main
```
