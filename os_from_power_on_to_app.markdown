# Operating System: From Power-On to Application Execution

This guide explains how an operating system (OS) functions from the moment a computer is powered on to running an application. We focus on a minimal Linux-like OS for x86-64, detailing all kernel and OS components, including hardware initialization, kernel boot, process management, memory management, file systems, device drivers, interrupts, and application execution. The explanation is structured sequentially with navigation links and comments for clarity, using small code snippets to illustrate concepts. This is designed for beginners building a computer from scratch.

## Table of Contents
- [1. Power-On and Hardware Initialization](#1-power-on-and-hardware-initialization)
- [2. BIOS/UEFI and Bootloader](#2-biosuefi-and-bootloader)
- [3. Kernel Initialization](#3-kernel-initialization)
- [4. Memory Management](#4-memory-management)
- [5. Process Management](#5-process-management)
- [6. Interrupt Handling](#6-interrupt-handling)
- [7. Device Drivers](#7-device-drivers)
- [8. File System](#8-file-system)
- [9. System Calls and User Space](#9-system-calls-and-user-space)
- [10. Running an Application](#10-running-an-application)
- [11. Conclusion](#11-conclusion)

---

## 1. Power-On and Hardware Initialization

When you press the power button, the computer begins its startup process.

```c
// Conceptual representation of hardware power-on
void power_on() {
    // PSU converts AC to DC (3.3V, 5V, 12V)
    power_supply_activate();
    // Motherboard distributes power to CPU, RAM, etc.
    motherboard_power_distribute();
    // CPU resets, program counter set to BIOS/UEFI address
    cpu_reset(0xFFFF0);
}
```

**Steps**:
- **Power Supply Unit (PSU)**: Converts AC (e.g., 120V) to DC, supplying power to components.
- **Motherboard**: Routes power and signals via chipset (e.g., Intel Z690).
- **BIOS/UEFI**: Stored in ROM (SPI flash), runs the Power-On Self-Test (POST):
  - Checks CPU, RAM, GPU, storage, and peripherals.
  - Emits beep codes or displays errors if hardware fails.
- **CPU**: Resets, setting the program counter to the BIOS/UEFI entry point (e.g., `0xFFFF0`).

**Why?** Initializes hardware, ensuring components are functional before software execution.

[Back to Top](#table-of-contents)

---

## 2. BIOS/UEFI and Bootloader

The BIOS/UEFI firmware prepares the hardware and loads the bootloader.

```c
// bios.c (simplified pseudo-code)
// Initializes hardware and loads bootloader
void bios_main() {
    post_check_hardware();    // Run POST
    init_chipset();           // Configure memory controller, PCIe
    init_ram();               // Set memory timings
    detect_devices();         // Identify storage, GPU
    load_bootloader("MBR");  // Load from disk
    jump_to_bootloader(0x7C00);
}
```

**BIOS/UEFI Steps**:
- Initializes chipset, RAM timings, and I/O controllers (USB, SATA).
- Scans for bootable devices (SSD, USB).
- Loads the bootloader from the Master Boot Record (MBR) or EFI System Partition (ESP) into RAM at `0x7C00`.

**Bootloader Code** (simplified x86 assembly):
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
    mov al, 1       ; 1 sector
    mov ch, 0       ; Cylinder 0
    mov cl, 2       ; Sector 2
    mov dh, 0       ; Head 0
    mov bx, 0x1000  ; Load at 0x1000
    int 0x13        ; BIOS disk interrupt
    ; Switch to 32-bit protected mode
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
    jmp 0x1000      ; Jump to kernel

gdt:
    ; Null descriptor, code segment, data segment
gdt_desc:
    dw gdt_end - gdt - 1
    dd gdt
gdt_end:

times 510-($-$$) db 0
dw 0xAA55 ; Boot signature
```

**Bootloader Steps**:
- Loads the kernel from disk sector 2 to memory (`0x1000`).
- Switches CPU from 16-bit real mode to 32-bit protected mode.
- Jumps to the kernel’s entry point.

**Why?** BIOS/UEFI sets up hardware, and the bootloader loads the kernel into memory.

[Back to Top](#table-of-contents)

---

## 3. Kernel Initialization

The kernel initializes core OS components.

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

#define VGA_ADDR 0xB8000

// Print to VGA text buffer
void print(const char* str) {
    volatile char* vga = (char*)VGA_ADDR;
    while (*str) {
        *vga++ = *str++;
        *vga++ = 0x07; // Gray on black
    }
}

void kmain() {
    print("Kernel Booted!\n");
    // Initialize core components
    init_memory();      // Memory management
    init_idt();         // Interrupts
    init_processes();   // Process management
    init_filesystem();  // File system
    init_drivers();     // Device drivers
    init_syscalls();    // System calls
    
    // Start scheduler
    enable_interrupts();
    start_scheduler();
    
    while (1) {
        asm volatile("hlt"); // Wait for interrupts
    }
}
```

**Steps**:
- Sets up the stack and calls `kmain`.
- Initializes memory, interrupts, processes, file system, drivers, and system calls.
- Enables interrupts and starts the scheduler.
- Enters an idle loop, waiting for interrupts.

**Why?** The kernel sets up essential OS services, enabling program execution.

[Back to Top](#table-of-contents)

---

## 4. Memory Management

The kernel manages physical and virtual memory.

```c
// memory.c
#define PAGE_SIZE 4096
uint8_t mem_map[1024]; // Bitmap for 4MB memory

// Allocate a free page
void* alloc_page() {
    for (int i = 0; i < 1024; i++) {
        if (!mem_map[i]) {
            mem_map[i] = 1;
            return (void*)(i * PAGE_SIZE);
        }
    }
    return 0;
}

// Free a page
void free_page(void* addr) {
    uint32_t idx = (uint32_t)addr / PAGE_SIZE;
    mem_map[idx] = 0;
}

// Initialize memory
void init_memory() {
    // Set up page tables (simplified)
    uint32_t* page_table = (uint32_t*)0x10000;
    for (int i = 0; i < 1024; i++) {
        page_table[i] = (i * PAGE_SIZE) | 0x3; // Present, writable
    }
    // Load page table into CR3
    asm volatile("mov %0, %%cr3" :: "r"(page_table));
}
```

**Functionality**:
- **Physical Memory**: Uses a bitmap to track free 4KB pages.
- **Virtual Memory**: Sets up page tables for address translation.
- **Page Faults**: Handles invalid memory accesses via interrupts (not shown).
- **Allocation**: Provides `alloc_page` and `free_page` for processes.

**Why?** Memory management isolates processes, allocates resources, and maps virtual to physical addresses.

[Back to Top](#table-of-contents)

---

## 5. Process Management

The kernel creates and schedules processes.

```c
// process.c
typedef struct {
    uint32_t pid;
    uint32_t esp; // Stack pointer
    uint32_t eip; // Instruction pointer
} Process;

Process procs[4]; // Support 4 processes
uint32_t proc_count = 0;
uint32_t current_proc = 0;

// Create a process
int create_process(void* entry) {
    if (proc_count >= 4) return -1;
    Process p = {
        .pid = proc_count + 1,
        .esp = (uint32_t)alloc_page() + PAGE_SIZE,
        .eip = (uint32_t)entry
    };
    procs[proc_count++] = p;
    return p.pid;
}

// Simple round-robin scheduler
void schedule() {
    current_proc = (current_proc + 1) % proc_count;
    // Switch context (simplified)
    asm volatile(
        "mov %0, %%esp\n"
        "jmp *%1\n"
        :: "r"(procs[current_proc].esp), "r"(procs[current_proc].eip)
    );
}

void init_processes() {
    // Create idle process
    create_process(idle_loop);
}
```

**Functionality**:
- **Process Creation**: Allocates memory and sets up stack/instruction pointers.
- **Scheduling**: Switches processes using a round-robin algorithm.
- **Context Switching**: Saves/restores CPU state (simplified here).

**Why?** Process management allows multiple programs to run concurrently, sharing CPU time.

[Back to Top](#table-of-contents)

---

## 6. Interrupt Handling

The kernel handles hardware and software interrupts.

```c
// interrupts.c
#define IDT_SIZE 256
struct idt_entry {
    uint16_t offset_low;
    uint16_t selector;
    uint8_t zero;
    uint8_t type_attr;
    uint16_t offset_high;
} idt[IDT_SIZE];

void set_idt_entry(uint8_t vec, void* handler) {
    uint32_t addr = (uint32_t)handler;
    idt[vec].offset_low = addr & 0xFFFF;
    idt[vec].selector = 0x08; // Code segment
    idt[vec].zero = 0;
    idt[vec].type_attr = 0x8E; // Interrupt gate
    idt[vec].offset_high = (addr >> 16) & 0xFFFF;
}

// Timer interrupt handler
void timer_handler() {
    schedule(); // Switch process
    print("Tick\n");
}

void init_idt() {
    // Set up IDT
    set_idt_entry(32, timer_handler); // IRQ0 (timer)
    // Load IDT
    struct { uint16_t size; uint32_t addr; } idt_desc = { IDT_SIZE * 8 - 1, (uint32_t)idt };
    asm volatile("lidt %0" :: "m"(idt_desc));
}

void enable_interrupts() {
    asm volatile("sti");
}
```

**Functionality**:
- **IDT**: Maps interrupt vectors to handlers.
- **Timer Interrupt**: Triggers scheduling (e.g., every 1ms).
- **Other Interrupts**: Handle keyboard, mouse, disk I/O, etc. (not shown).

**Why?** Interrupts allow the OS to respond to hardware events and manage multitasking.

[Back to Top](#table-of-contents)

---

## 7. Device Drivers

Drivers enable the kernel to interact with hardware.

```c
// drivers.c
void init_drivers() {
    init_timer();  // Programmable Interval Timer
    init_keyboard(); // PS/2 or USB keyboard
    init_disk();   // SATA/SSD
}

// Timer driver (simplified)
void init_timer(uint32_t freq) {
    uint32_t divisor = 1193180 / freq; // PIT clock
    outb(0x43, 0x36); // Set PIT mode
    outb(0x40, divisor & 0xFF); // Low byte
    outb(0x40, (divisor >> 8) & 0xFF); // High byte
}

// Keyboard driver (simplified)
char read_keyboard() {
    while (!(inb(0x64) & 1)); // Wait for data
    return inb(0x60); // Read scancode
}
```

**Functionality**:
- **Timer**: Generates periodic interrupts for scheduling.
- **Keyboard**: Reads input from PS/2 or USB.
- **Disk**: Manages storage access (not fully shown).
- Drivers use I/O ports or memory-mapped I/O to communicate with hardware.

**Why?** Drivers abstract hardware, allowing the kernel to control devices.

[Back to Top](#table-of-contents)

---

## 8. File System

A simple in-memory file system organizes data.

```c
// fs.c
typedef struct {
    char name[32];
    char data[512];
    uint32_t size;
} File;

File fs[10];
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

void init_filesystem() {
    // Initialize file system
    create_file("test.txt", "Hello, OS!", 10);
}
```

**Functionality**:
- **Structure**: Array of files with name, data, and size.
- **Operations**: Create and find files.
- **Real Systems**: Use disk-based file systems (ext4, NTFS) with inodes and data blocks.

**Why?** The file system organizes data for programs and the kernel.

[Back to Top](#table-of-contents)

---

## 9. System Calls and User Space

System calls allow user programs to interact with the kernel.

```c
// syscall.c
#define SYS_WRITE 1
#define SYS_EXIT 60

void syscall_handler(uint32_t syscall_num, uint32_t arg1, uint32_t arg2) {
    if (syscall_num == SYS_WRITE) {
        print((const char*)arg1); // Write to VGA
    } else if (syscall_num == SYS_EXIT) {
        // Terminate process (simplified)
        procs[current_proc].pid = 0;
        schedule();
    }
}

void init_syscalls() {
    // Set up syscall interrupt (e.g., int 0x80)
    set_idt_entry(0x80, syscall_handler);
}
```

**Functionality**:
- **Syscalls**: `write` outputs text, `exit` terminates a process.
- **User Space**: Programs run in unprivileged mode, using syscalls to access kernel services.

**Why?** System calls provide a safe interface between user programs and the kernel.

[Back to Top](#table-of-contents)

---

## 10. Running an Application

We run a simple C program.

**Application Code**:
```c
// app.c
#include <stdio.h>

int main() {
    for (int i = 1; i <= 5; i++) {
        printf("Number: %d\n", i);
    }
    return 0;
}
```

**Minimal libc** (for `printf`):
```c
// libc.c
void printf(const char* fmt, int num) {
    char buf[32];
    // Simple number-to-string conversion
    int i = 0;
    if (num == 0) buf[i++] = '0';
    while (num > 0) {
        buf[i++] = (num % 10) + '0';
        num /= 10;
    }
    buf[i] = 0;
    // Reverse string
    for (int j = 0; j < i / 2; j++) {
        char tmp = buf[j];
        buf[j] = buf[i - 1 - j];
        buf[i - 1 - j] = tmp;
    }
    // Print via syscall
    asm volatile("mov $1, %eax; mov %0, %ebx; int $0x80" :: "r"(buf));
}
```

**Compilation** (simplified):
- **Compiler**: Translates `app.c` to assembly (`app.asm`).
- **Assembler**: Converts `app.asm` to `app.o`.
- **Linker**: Links `app.o` with `libc.o` to create `app_executable`.

**Kernel Loading**:
```c
void load_app(const char* path) {
    File* file = find_file(path);
    if (!file) return;
    void* mem = alloc_page();
    memcpy(mem, file->data, file->size);
    create_process(mem); // Start process
}
```

**Execution**:
- Kernel loads `app_executable` into memory.
- Creates a process and schedules it.
- Program runs, calling `printf` (via syscall), outputting to VGA.

**Output**:
```
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

**Why?** The OS manages the application’s lifecycle, from loading to execution, using its core services.

[Back to Top](#table-of-contents)

---

## 11. Conclusion

From power-on, the OS orchestrates hardware and software:
- **Power-On**: Initializes hardware via PSU and BIOS/UEFI.
- **Bootloader**: Loads the kernel.
- **Kernel**: Sets up memory, processes, interrupts, drivers, file system, and syscalls.
- **Application**: Runs in user space, using kernel services to execute and output results.

Each component—memory, processes, interrupts, drivers, file system, and syscalls—works together to provide a stable environment for applications. This minimal OS demonstrates core functionalities, mirroring real systems like Linux.