# From Power Button to Web Page: A Detailed Computer Process

This guide walks through the entire process of a computer starting from pressing the power button to running a C program and displaying a web page served by a local web server. We'll include a minimal kernel, a simple operating system, file system details, and how a web server handles requests (including clicks). Each step is explained in sequence for clarity, with small, functional code snippets covering all components. The explanation assumes we're building a computer from scratch on a Linux-like system with x86-64 architecture, keeping it beginner-friendly but comprehensive.

## 1. Pressing the Power Button

When you press the power button:
- **Power Supply Unit (PSU)**: Converts AC (e.g., 120V/240V) to DC (3.3V, 5V, 12V), powering the motherboard, CPU, RAM, and peripherals.
- **Motherboard**: Distributes power and signals via the chipset (e.g., Intel Z690 or AMD B550).
- **BIOS/UEFI**: Stored in a ROM chip (e.g., SPI flash), the Basic Input/Output System or Unified Extensible Firmware Interface starts. It runs the **Power-On Self-Test (POST)** to check:
  - CPU functionality.
  - RAM integrity (via memory tests).
  - GPU presence.
  - Storage devices (SSD/HDD).
  - Keyboard and other peripherals.
- **POST Output**: If errors occur, the motherboard emits beep codes (e.g., 3 beeps for RAM failure) or displays codes on a debug LED.
- **CPU Reset**: The CPU’s program counter is set to the BIOS/UEFI entry point (e.g., `0xFFFF0` in x86 real mode).

**Why?** The PSU and BIOS/UEFI initialize the hardware, ensuring it’s functional before loading software.

## 2. BIOS/UEFI Initialization

The BIOS/UEFI firmware:
- **Initializes Chipset**: Configures the memory controller, PCIe lanes, USB controllers, and SATA ports.
- **RAM Setup**: Sets memory timings (e.g., CAS latency) and maps physical memory.
- **Device Detection**: Scans for storage devices, GPUs, and network cards.
- **Boot Device Selection**: Reads the boot order (e.g., SSD, USB) from CMOS or UEFI variables.
- **Loads Bootloader**: Finds the bootloader in the Master Boot Record (MBR) or EFI System Partition (ESP) of the boot device and loads it into RAM (e.g., at `0x7C00` for MBR).

**Example BIOS/UEFI Pseudo-code**:
```c
// bios.c (simplified)
void bios_main() {
    post_check_hardware(); // Run POST
    init_chipset();        // Configure memory, PCIe
    init_ram();            // Set up memory
    detect_devices();      // Find storage, GPU
    load_bootloader(BOOT_DEVICE); // Read MBR/ESP
    jump_to_bootloader();  // Transfer control
}
```

**Why?** BIOS/UEFI prepares the hardware and hands control to the bootloader, which loads the OS.

## 3. Bootloader Execution

The bootloader (e.g., a simplified GRUB-like program) loads the operating system kernel.

**Example Bootloader Code** (x86 Assembly, simplified):
```nasm
; bootloader.asm
bits 16
org 0x7C00

start:
    mov ax, 0x07C0
    mov ds, ax
    mov es, ax
    ; Read kernel from disk (sector 2)
    mov ah, 0x02    ; BIOS read sector
    mov al, 1       ; Read 1 sector
    mov ch, 0       ; Cylinder 0
    mov cl, 2       ; Sector 2
    mov dh, 0       ; Head 0
    mov bx, 0x1000  ; Load at 0x1000
    int 0x13        ; BIOS disk interrupt
    ; Switch to protected mode
    cli
    lgdt [gdt_desc]
    mov eax, cr0
    or eax, 1
    mov cr0, eax
    jmp 0x08:protected_mode

bits 32
protected_mode:
    mov ax, 0x10
    mov ds, ax
    mov es, ax
    ; Jump to kernel
    jmp 0x1000

gdt:
    ; Null descriptor, code segment, data segment
gdt_desc:
    dw gdt_end - gdt - 1
    dd gdt
gdt_end:

times 510-($-$$) db 0
dw 0xAA55 ; Boot signature
```

**Explanation**:
- Loads the kernel from disk sector 2 to memory address `0x1000`.
- Switches from 16-bit real mode to 32-bit protected mode (required for modern OS).
- Jumps to the kernel’s entry point.

**Why?** The bootloader bridges BIOS/UEFI and the OS, loading the kernel into memory.

## 4. Minimal Kernel and Operating System

We’ll create a minimal OS with a kernel that supports basic functionality: memory management, process scheduling, interrupts, and a simple file system.

### Kernel Code
Here’s a simplified kernel in C and assembly.

**Kernel Entry (Assembly)**:
```nasm
; kernel_entry.asm
section .text
global _start
extern kmain

_start:
    mov esp, 0x9000 ; Set stack
    call kmain      ; Call C kernel
    cli
    hlt             ; Halt
```

**Kernel Main (C)**:
```c
// kernel.c
#include <stdint.h>

#define VGA_ADDR 0xB8000 // VGA text buffer

// Simple VGA print
void print(const char* str) {
    volatile char* vga = (char*)VGA_ADDR;
    while (*str) {
        *vga++ = *str++;
        *vga++ = 0x07; // Gray on black
    }
}

// Process structure
typedef struct {
    uint32_t pid;
    uint32_t esp; // Stack pointer
} Process;

// Simple scheduler
Process procs[2];
uint32_t current_proc = 0;

void schedule() {
    current_proc = (current_proc + 1) % 2;
    // Simplified: Switch stack (real OS would save registers)
}

// Timer interrupt handler
void timer_handler() {
    schedule();
    print("Tick\n");
}

// Memory management (simple bitmap)
uint8_t mem_map[1024]; // 4MB bitmap
void* alloc_page() {
    for (int i = 0; i < 1024; i++) {
        if (!mem_map[i]) {
            mem_map[i] = 1;
            return (void*)(i * 4096);
        }
    }
    return 0;
}

void kmain() {
    print("Minimal OS Booted!\n");
    
    // Set up IDT (Interrupt Descriptor Table)
    setup_idt();
    // Set up timer (IRQ0)
    setup_timer(1000); // 1ms ticks
    
    // Initialize processes
    procs[0].pid = 1;
    procs[1].pid = 2;
    
    // Enable interrupts
    asm volatile("sti");
    
    // Kernel loop
    while (1) {
        asm volatile("hlt");
    }
}

// Simplified IDT setup
void setup_idt() {
    // Map timer interrupt (IRQ0) to handler
}

// Simplified timer setup
void setup_timer(uint32_t freq) {
    // Program PIT (Programmable Interval Timer)
}
```

**Explanation**:
- **VGA Output**: Prints to the screen using the VGA text buffer.
- **Memory Management**: Uses a bitmap to allocate 4KB pages.
- **Process Scheduling**: Switches between two processes using a simple round-robin scheduler.
- **Interrupts**: Handles timer interrupts (IRQ0) to trigger scheduling.
- **IDT**: Sets up the Interrupt Descriptor Table for interrupt handling.

### File System
Our OS uses a simple in-memory file system (like tmpfs).

**File System Code**:
```c
// fs.c
typedef struct {
    char name[32];
    char data[512];
    uint32_t size;
} File;

File fs[10]; // 10 files max
uint32_t file_count = 0;

int create_file(const char* name, const char* data, uint32_t size) {
    if (file_count >= 10) return -1;
    strncpy(fs[file_count].name, name, 31);
    memcpy(fs[file_count].data, data, size);
    fs[file_count].size = size;
    file_count++;
    return 0;
}

File* find_file(const char* name) {
    for (int i = 0; i < file_count; i++) {
        if (strcmp(fs[i].name, name) == 0) return &fs[i];
    }
    return 0;
}
```

**Explanation**:
- Stores files in an array with name, data, and size.
- Supports creating and finding files.
- In a real OS, this would be on disk with a structure like ext4 or FAT32.

**Why?** The kernel manages hardware, processes, memory, and files, providing an environment for user programs. The file system organizes data storage.

## 5. Compiling and Running a C Program

Our OS supports running a simple C program. We’ll use the same program as before.

**C Program**:
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

### Compilation Process
We compile this program using a cross-compiler targeting our OS.

#### Compiler
**Pseudo-code** (simplified, assumes hosted environment for simplicity):
```c
// my_compiler.c
void compile_c_to_asm(const char* c_code, const char* asm_output) {
    FILE* asm_file = fopen(asm_output, "w");
    fprintf(asm_file, "section .text\n");
    fprintf(asm_file, "global _start\nextern printf\n");
    fprintf(asm_file, "_start:\n");
    fprintf(asm_file, "    mov rbp, rsp\n");
    fprintf(asm_file, "    mov rcx, 1\n");
    fprintf(asm_file, "loop_start:\n");
    fprintf(asm_file, "    cmp rcx, 5\n");
    fprintf(asm_file, "    jg loop_end\n");
    fprintf(asm_file, "    lea rdi, [rel format]\n");
    fprintf(asm_file, "    mov rsi, rcx\n");
    fprintf(asm_file, "    xor rax, rax\n");
    fprintf(asm_file, "    call printf\n");
    fprintf(asm_file, "    inc rcx\n");
    fprintf(asm_file, "    jmp loop_start\n");
    fprintf(asm_file, "loop_end:\n");
    fprintf(asm_file, "    mov rax, 60\n");
    fprintf(asm_file, "    xor rdi, rdi\n");
    fprintf(asm_file, "    syscall\n");
    fprintf(asm_file, "section .data\n");
    fprintf(asm_file, "format db 'Number: %d', 10, 0\n");
    fclose(asm_file);
}
```

**Output**: `simple.asm`.

#### Assembler
```c
// my_assembler.c
void assemble_to_object(const char* asm_file, const char* obj_file) {
    FILE* asm_fp = fopen(asm_file, "r");
    FILE* obj_fp = fopen(obj_file, "wb");
    char line[256];
    while (fgets(line, sizeof(line), asm_fp)) {
        if (strstr(line, "mov rcx, 1")) {
            unsigned char code[] = {0x48, 0xC7, 0xC1, 0x01, 0x00, 0x00, 0x00};
            fwrite(code, sizeof(code), 1, obj_fp);
        }
        else if (strstr(line, "cmp rcx, 5")) {
            unsigned char code[] = {0x48, 0x83, 0xF9, 0x05};
            fwrite(code, sizeof(code), 1, obj_fp);
        }
    }
    fclose(asm_fp);
    fclose(obj_fp);
}
```

**Output**: `simple.o`.

#### Linker
Links with a minimal `libc` (providing `printf`).

```c
// my_linker.c
void link_object_to_executable(const char* obj_file, const char* libc, const char* exec_file) {
    FILE* obj_fp = fopen(obj_file, "rb");
    FILE* libc_fp = fopen(libc, "rb");
    FILE* exec_fp = fopen(exec_file, "wb");
    unsigned char elf_header[] = {0x7F, 'E', 'L', 'F', 2, 1, 1, 0, /* ... */ };
    fwrite(elf_header, sizeof(elf_header), 1, exec_fp);
    char buffer[1024];
    size_t bytes;
    while ((bytes = fread(buffer, 1, sizeof(buffer), obj_fp)) > 0) {
        fwrite(buffer, 1, bytes, exec_fp);
    }
    while ((bytes = fread(buffer, 1, sizeof(buffer), libc_fp)) > 0) {
        fwrite(buffer, 1, bytes, exec_fp);
    }
    fclose(obj_fp);
    fclose(libc_fp);
    fclose(exec_fp);
}
```

**Output**: `simple_executable`.

### Running in Our OS
The kernel loads `simple_executable` into memory, creates a process, and schedules it.

**Pseudo-code (Kernel)**:
```c
void load_program(const char* path) {
    File* file = find_file(path);
    if (!file) return;
    void* mem = alloc_page();
    memcpy(mem, file->data, file->size);
    Process p = { .pid = file_count + 1, .esp = (uint32_t)mem + 4096 };
    procs[file_count] = p;
}
```

**Output** (via VGA):
```
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

**Why?** Compilation translates C to machine code, and the kernel runs it as a process.

## 6. Setting Up a Web Server

Our OS supports a simple HTTP server written in C, running as a user process.

**Web Server Code**:
```c
// web_server.c
#include <stdint.h>
#include <string.h>

// Simplified socket API (provided by kernel)
int socket_bind_listen(uint16_t port);
int socket_accept(int sock);
int socket_read(int conn, char* buf, int len);
int socket_write(int conn, char* buf, int len);

// Simple HTTP response
const char* response = 
    "HTTP/1.1 200 OK\r\n"
    "Content-Type: text/html\r\n"
    "Content-Length: 110\r\n"
    "\r\n"
    "<!DOCTYPE html><html><head><title>My Page</title></head>"
    "<body><h1>Hello!</h1><a href='/click'>Click me</a></body></html>";

void server_main() {
    int sock = socket_bind_listen(8080);
    while (1) {
        int conn = socket_accept(sock);
        char buf[512];
        int len = socket_read(conn, buf, 512);
        
        // Parse request (simplified)
        if (strstr(buf, "GET /click")) {
            // Handle click
            socket_write(conn, response, strlen(response)); // Same page for simplicity
        } else {
            socket_write(conn, response, strlen(response));
        }
        // Close connection
        socket_close(conn);
    }
}
```

**Kernel Socket API (Pseudo-code)**:
```c
// kernel_socket.c
int socket_bind_listen(uint16_t port) {
    // Allocate socket descriptor
    int sock = alloc_fd();
    // Bind to port (simplified)
    bind_port(sock, port);
    return sock;
}

int socket_accept(int sock) {
    // Wait for connection
    return alloc_fd();
}
```

**Explanation**:
- Binds to port 8080 and listens for HTTP requests.
- Serves a simple HTML page with a clickable link (`/click`).
- Handles `GET /click` by sending the same page (simplified).

### File System Integration
The server could read `index.html` from the file system:
```c
File* file = find_file("index.html");
socket_write(conn, file->data, file->size);
```

**HTML File**:
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head><title>My Page</title></head>
<body>
    <h1>Hello!</h1>
    <a href="/click">Click me</a>
</body>
</html>
```

**Why?** The server runs as a process, using kernel sockets to handle HTTP requests.

## 7. Displaying the Web Page and Handling Clicks

A simple browser (running as a process) accesses the server.

**Browser Pseudo-code**:
```c
// browser.c
void browser_main() {
    int sock = socket_connect("127.0.0.1", 8080);
    const char* req = "GET / HTTP/1.1\r\nHost: localhost\r\n\r\n";
    socket_write(sock, req, strlen(req));
    char buf[1024];
    int len = socket_read(sock, buf, 1024);
    // Parse HTML (simplified)
    print_to_vga(buf); // Display on screen
    // Handle click (e.g., mouse interrupt)
    if (mouse_click_detected()) {
        socket_write(sock, "GET /click HTTP/1.1\r\nHost: localhost\r\n\r\n", 40);
        len = socket_read(sock, buf, 1024);
        print_to_vga(buf);
    }
}
```

**Kernel Mouse Handling**:
```c
void mouse_interrupt_handler() {
    // Read PS/2 or USB mouse data
    int x, y, button;
    read_mouse(&x, &y, &button);
    if (button & 1) {
        notify_browser_click(x, y);
    }
}
```

**Explanation**:
- The browser sends a `GET /` request to `localhost:8080`.
- The server responds with `index.html`.
- A mouse click on the link triggers a `GET /click` request, handled by the server.
- The kernel’s interrupt handler detects mouse input and notifies the browser.

**Why?** The browser and server communicate via sockets, with the kernel handling I/O and interrupts.

## 8. File System Details

The file system organizes data:
- **Structure**: In-memory array of `File` structs (name, data, size).
- **Operations**:
  - `create_file`: Adds a file.
  - `find_file`: Retrieves a file by name.
- **Storage**: In RAM (like tmpfs). A real OS uses disk-based systems (ext4, NTFS) with:
  - Inodes: Metadata (permissions, size).
  - Data blocks: File content.
- **Kernel Role**: Provides syscalls (`open`, `read`, `write`) to access files.

**Why?** The file system enables programs (e.g., server) to store and retrieve data.

## 9. Sequence of Operations

1. **Power On**: PSU powers components, BIOS/UEFI runs POST.
2. **BIOS/UEFI**: Initializes hardware, loads bootloader.
3. **Bootloader**: Loads kernel into RAM, switches to protected mode.
4. **Kernel**: Sets up memory, processes, interrupts, file system, and sockets.
5. **OS**: Runs user processes (C program, web server, browser).
6. **C Program**:
   - Compiled to assembly, assembled to object, linked with `libc`.
   - Kernel loads and schedules it, outputs via `printf`.
7. **Web Server**:
   - Runs as a process, binds to port 8080.
   - Serves `index.html` or handles `/click` requests.
8. **Browser**:
   - Sends HTTP requests to server.
   - Displays HTML via VGA.
   - Handles clicks via mouse interrupts.
9. **File System**: Stores `simple.c`, `index.html`, accessed by programs.

## 10. Why Each Component is Necessary

- **Hardware**: Executes instructions and stores data.
- **BIOS/UEFI**: Initializes hardware and starts boot.
- **Bootloader**: Loads the kernel.
- **Kernel/OS**: Manages resources (CPU, memory, I/O, files).
- **Compiler/Assembler/Linker**: Translates C to executable code.
- **Libraries**: Provide functions like `printf`.
- **File System**: Organizes data storage.
- **Web Server**: Serves web content over HTTP.
- **Browser**: Renders web pages and handles user input.
- **Networking**: Enables client-server communication.
- **Interrupts**: Handle asynchronous events (timer, mouse).

## 11. Building and Running

**Build Tools**:
```bash
gcc -o my_compiler my_compiler.c
gcc -o my_assembler my_assembler.c
gcc -o my_linker my_linker.c
nasm -f elf32 bootloader.asm -o bootloader.o
gcc -m32 -nostdlib kernel_entry.asm kernel.c fs.c -o kernel.bin
```

**Run C Program**:
```bash
./my_compiler simple.c simple.asm
./my_assembler simple.asm simple.o
./my_linker simple.o libc.a simple_executable
# Load simple_executable into OS file system
```

**Run Web Server**:
```bash
# Compile and load web_server.c similarly
```

**Access Browser**:
- In our OS, the browser outputs to VGA.
- In a real system: Open `http://localhost:8080`.

## 12. Conclusion

From pressing the power button, the computer initializes hardware, boots a minimal OS with a kernel, runs a C program, and serves a web page via a local server. The kernel manages processes, memory, files, and networking, while the server handles HTTP requests, including clicks. The file system organizes data, and interrupts enable interactivity. This detailed process shows how each component works together to deliver a functional system.