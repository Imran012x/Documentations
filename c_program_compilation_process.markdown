# Understanding C Program Compilation: From Source to Executable

This guide explains how to create a simple C program with a for loop, compile it, assemble it, link it with libraries, and generate an executable file. We'll assume we're building everything from scratch on a new computer, so each step is explained in a beginner-friendly way. We'll also create simplified versions of a compiler, assembler, and linker to illustrate the process.

## Step 1: Writing the C Program

Let's start with a simple C program that uses a for loop to print numbers from 1 to 5.

```c
// simple.c
#include <stdio.h>

int main() {
    for (int i = 1; i <= 5; i++) {
        printf("Number: %d\n", i);
    }
    return 0;
}
```

**Explanation**:
- `#include <stdio.h>`: Includes the standard input/output library for using `printf`.
- `int main()`: The program's entry point.
- `for (int i = 1; i <= 5; i++)`: A for loop that iterates from 1 to 5.
- `printf("Number: %d\n", i)`: Prints each number.
- `return 0`: Indicates successful program completion.

This source code is written in a high-level language (C) that humans understand, but computers need it translated into machine code.

## Step 2: Creating a Simple Compiler

A **compiler** translates C code into assembly language, which is closer to machine code. For simplicity, we'll describe a minimal compiler that converts our C code into x86-64 assembly (for a 64-bit Linux system).

### Compiler Code (Pseudo-code)

```c
// my_compiler.c (simplified pseudo-code)
#include <stdio.h>
#include <string.h>

void compile_c_to_asm(const char* c_code, const char* asm_output) {
    FILE* asm_file = fopen(asm_output, "w");
    
    // Write assembly header
    fprintf(asm_file, "section .text\n");
    fprintf(asm_file, "global _start\n");
    fprintf(asm_file, "extern printf\n");
    
    // Parse C code (simplified)
    if (strstr(c_code, "int main")) {
        fprintf(asm_file, "_start:\n");
        fprintf(asm_file, "    mov rbp, rsp\n"); // Set up stack frame
        
        // Handle for loop (i from 1 to 5)
        fprintf(asm_file, "    mov rcx, 1\n"); // i = 1
        fprintf(asm_file, "loop_start:\n");
        fprintf(asm_file, "    cmp rcx, 5\n"); // Compare i with 5
        fprintf(asm_file, "    jg loop_end\n"); // Jump if i > 5
        
        // Call printf
        fprintf(asm_file, "    lea rdi, [rel format]\n"); // Load format string
        fprintf(asm_file, "    mov rsi, rcx\n"); // Pass i
        fprintf(asm_file, "    xor rax, rax\n"); // Clear rax for printf
        fprintf(asm_file, "    call printf\n");
        
        fprintf(asm_file, "    inc rcx\n"); // i++
        fprintf(asm_file, "    jmp loop_start\n");
        fprintf(asm_file, "loop_end:\n");
        
        // Exit program
        fprintf(asm_file, "    mov rax, 60\n"); // Syscall: exit
        fprintf(asm_file, "    xor rdi, rdi\n"); // Status 0
        fprintf(asm_file, "    syscall\n");
    }
    
    // Add data section for format string
    fprintf(asm_file, "section .data\n");
    fprintf(asm_file, "format db 'Number: %d', 10, 0\n");
    
    fclose(asm_file);
}
```

**Explanation**:
- The compiler reads the C code and recognizes the `main` function and for loop.
- It generates x86-64 assembly code, setting up the stack, translating the loop, and calling `printf`.
- It includes a data section for the format string `"Number: %d\n"`.
- The output is an assembly file (e.g., `simple.asm`).

### Generated Assembly Code

Running our compiler on `simple.c` produces:

```nasm
; simple.asm
section .text
global _start
extern printf

_start:
    mov rbp, rsp
    mov rcx, 1
loop_start:
    cmp rcx, 5
    jg loop_end
    lea rdi, [rel format]
    mov rsi, rcx
    xor rax, rax
    call printf
    inc rcx
    jmp loop_start
loop_end:
    mov rax, 60
    xor rdi, rdi
    syscall

section .data
format db "Number: %d", 10, 0
```

**Why is this needed?**
- C code is too abstract for the CPU. Assembly is a low-level language that maps closely to machine instructions.
- The compiler bridges the gap, translating high-level constructs like loops into CPU instructions.

## Step 3: Creating a Simple Assembler

An **assembler** converts assembly code into machine code (object code). For simplicity, we'll describe a minimal assembler that processes our `simple.asm` file.

### Assembler Code (Pseudo-code)

```c
// my_assembler.c (simplified pseudo-code)
#include <stdio.h>

void assemble_to_object(const char* asm_file, const char* obj_file) {
    FILE* asm_fp = fopen(asm_file, "r");
    FILE* obj_fp = fopen(obj_file, "wb");
    
    char line[256];
    while (fgets(line, sizeof(line), asm_fp)) {
        // Parse assembly instructions (simplified)
        if (strstr(line, "mov rcx, 1")) {
            // Machine code for "mov rcx, 1"
            unsigned char code[] = {0x48, 0xC7, 0xC1, 0x01, 0x00, 0x00, 0x00};
            fwrite(code, sizeof(code), 1, obj_fp);
        }
        else if (strstr(line, "cmp rcx, 5")) {
            // Machine code for "cmp rcx, 5"
            unsigned char code[] = {0x48, 0x83, 0xF9, 0x05};
            fwrite(code, sizeof(code), 1, obj_fp);
        }
        // Add more instruction translations...
    }
    
    // Add symbol table and relocation entries (simplified)
    // Note: Real assemblers generate ELF object files with sections
    fclose(asm_fp);
    fclose(obj_fp);
}
```

**Explanation**:
- The assembler reads each assembly instruction and converts it to binary machine code.
- For example, `mov rcx, 1` becomes the byte sequence `48 C7 C1 01 00 00 00`.
- The output is an object file (e.g., `simple.o`), typically in ELF format on Linux.
- The assembler also handles symbols (like `printf`) and marks them for the linker to resolve.

**Output**: `simple.o` (binary object file, not human-readable).

**Why is this needed?**
- Assembly code is still text-based. The CPU executes binary instructions.
- The assembler translates mnemonics (like `mov`) into their binary equivalents.

## Step 4: Creating a Simple Linker

A **linker** combines object files and libraries into a single executable. Our program uses `printf`, which is in the C standard library (`libc`), so the linker resolves this reference.

### Linker Code (Pseudo-code)

```c
// my_linker.c (simplified pseudo-code)
#include <stdio.h>

void link_object_to_executable(const char* obj_file, const char* libc, const char* exec_file) {
    // Read object file
    FILE* obj_fp = fopen(obj_file, "rb");
    // Read libc (simplified, assumes precompiled library)
    FILE* libc_fp = fopen(libc, "rb");
    // Create executable
    FILE* exec_fp = fopen(exec_file, "wb");
    
    // Create ELF header
    unsigned char elf_header[] = {
        0x7F, 'E', 'L', 'F', 2, 1, 1, 0, // ELF magic
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, // More header fields...
    };
    fwrite(elf_header, sizeof(elf_header), 1, exec_fp);
    
    // Copy object code from simple.o
    char buffer[1024];
    size_t bytes;
    while ((bytes = fread(buffer, 1, sizeof(buffer), obj_fp)) > 0) {
        fwrite(buffer, 1, bytes, exec_fp);
    }
    
    // Resolve symbols (e.g., printf)
    // In a real linker, this involves searching libc for printf's address
    // Simplified: Assume printf is at a fixed offset in libc
    // Copy relevant libc code from from libc
    // Copy relevant parts of libc
    while ((bytes = fread(buffer, 1, sizeof(buffer), buffer, libc_fp) > 0)) {
        fwrite(buffer, 1, bytes, exec_fp);
    }
    
    // Write program header and section tables
    // Simplified: Assume minimal headers for executable
    // Fixup addresses for printf calls
    fclose(obj_fp);
    fclose(libc_fp);
    fclose(exec_fp);
}
```

**Explanation**:
- The linker reads `simple.o` and `libc.so` (or a simplified static `libc.a`).
- It creates an ELF executable file (`simple_exec`).
- It resolves `printf` by finding its code in `libc` and fixing the `call printf` instruction's address.
- It sets up the ELF header and program headers to tell the operating system how to load the executable.

**Output**: `simple_executable` (binary executable file).

**Why is linking needed?**
- The object file has undefined symbols (`printf`) that need addresses.
- The linker combines all code (our program + libraries) into one file.
- Libraries like `libc` provide essential functions (`printf`, `exit`, etc., ) that our program uses).

## Step 5: Library Linking Details

**What is `libc`?**
- The C standard library (`libc`) contains functions like `printf`, `scanf`, `malloc`, etc.
- On Linux, it’s typically `/usr/lib/x86_64-linux-gnu/libc.so` (shared) or `libc.a` (static).
- `printf` formats and outputs text, relying on system calls like `write` to interact with the OS.

**Why is `libc` required?**
- Writing `printf` from scratch would require handling string formatting and OS calls.
- `libc` provides pre-written, optimized for performance.
- Our program references `printf`, so the linker must include it.

**How is `libc` linked?**
- **Dynamic Linking**: The executable references `libc.so` at runtime, saving disk space.
- **Static Linking**: `libc.a`’s `printf`’s code is copied into the executable, making it standalone but larger.
- Our linker example uses dynamic linking for simplicity.

## Step 6: Final Executable File

The final executable (`simple_executable`) is an ELF file that the OS can run. It contains:
- **Header**: Tells the OS how to load the program.
- **Code Section**: Contains our program’s machine code (the loop and `printf` call).
- **Data Section**: Contains data like `"Number: %d\n"`.
- **Dynamic Linking Info**: References to `libc` functions like `printf`.

**Running the executable**:
On Linux:
```bash
chmod +x simple_executable
./simple_executable
```

**Expected Output**:
```
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

**Why is this the final step?**
- The executable is ready for the CPU to run, with all code and references resolved.
- The OS loads it into memory, sets up `libc`, and starts execution at `_start`.

## Step 7: Why Each Tool Is Required

- **Compiler**: Translates C to assembly because C is too abstract for CPUs.
- **Assembler**: Converts assembly to machine code because assembly is still text.
- **Linker**: Combines code and resolves references because programs often use external libraries.
- **Libraries**: Provide standard functions to avoid reinventing basic operations.
- **Executable**: Packages everything in a format the OS can execute.

## Step 8: Building the Tools

To build our compiler, assembler, and linker (assuming we’re on a Linux system with `gcc`):

```bash
gcc -o my_compiler my_compiler.c
gcc -o my_assembler my_assembler.c
gcc -o my_linker my_linker.c
```

Then compile the program:

```bash
./my_compiler simple.c simple.asm
./my_assembler simple.asm simple.o
./my_linker simple.o /usr/lib64-linux-gnu/libc.so simple_executable
./simple_executable
```

**Note**: Real-world tools (`gcc`, `as`, `ld`) are more complex but follow the same principles.

## Step 9: Why Build from Scratch?

Building a computer from scratch helps us understand:
- How high-level code becomes machine code.
- The role of each tool in the compilation pipeline.
- How libraries and the linker work together to create- Why creating an executable requires multiple steps.

## Conclusion

We’ve walked through creating a simple C program with a for loop, compiling it to assembly, assembling it to an object file, linking it with `libc`, and generating an executable. By building simplified versions of a compiler, assembler, and linker, we’ve shown how each step transforms the code into something the CPU can execute. This process, while simplified here, mirrors what real tools like `gcc`, `as`, and `ld` do on a modern computer.