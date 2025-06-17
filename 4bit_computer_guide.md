# Complete 4-Bit Computer System Guide

## Table of Contents
1. [Architecture Overview](#architecture-overview)
2. [Core Components](#core-components)
3. [Instruction Set](#instruction-set)
4. [Control Signals](#control-signals)
5. [Microcode Implementation](#microcode-implementation)
6. [Example Programs](#example-programs)
7. [How It All Works Together](#how-it-all-works-together)

## Architecture Overview

This 4-bit computer implements all fundamental components of a modern computer in a simplified form:

```
    ┌─────────────────────────────────────────────────────────┐
    │                        8-bit Bus                        │
    └─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬──┬─┬─┬─┬─┬─┬─┬─┘
      │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │   │ │ │ │ │ │ │ │
    ┌─▼─┐ ┌──▼──┐ ┌─▼─┐ ┌─▼─┐ ┌─▼─┐ ┌▼┐ ┌▼┐ ┌──▼──┐ ┌─▼─┐ ┌─▼─┐
    │RAM│ │ MAR │ │ IR│ │ PC│ │ A │ │B│ │O│ │ ALU │ │ I │ │CL │
    │   │ │     │ │   │ │   │ │   │ │ │ │ │ │     │ │   │ │   │
    └───┘ └─────┘ └───┘ └───┘ └───┘ └─┘ └─┘ └─────┘ └───┘ └───┘
```

### Key Features:
- **4-bit data width** with 8-bit address space (256 bytes of memory)
- **Von Neumann architecture** (shared memory for instructions and data)
- **16 instructions** covering all essential operations
- **Flag-based conditional execution** (N, C, Z flags)
- **Microcode-controlled execution** for precise timing

## Core Components

### 1. Central Processing Unit (CPU)

#### Accumulator (Register A)
- **Purpose**: Primary working register for arithmetic and logic operations
- **Size**: 4 bits
- **Function**: Stores operands and results of calculations
```
Register A: [A3][A2][A1][A0]
Example: 1011 (decimal 11)
```

#### Arithmetic Logic Unit (ALU)
- **Operations**: Addition, Subtraction with carry/borrow
- **Inputs**: Register A and Register B (or immediate values)
- **Outputs**: Result + Status flags (N, C, Z)
- **Flag Generation**:
  - **N (Negative)**: Set when result's MSB = 1
  - **C (Carry)**: Set when operation produces carry/borrow
  - **Z (Zero)**: Set when result = 0000

#### Program Counter (PC)
- **Purpose**: Points to the next instruction to execute
- **Size**: 8 bits (addresses 0x00 to 0xFF)
- **Operations**: Auto-increment, manual load for jumps

#### Instruction Register (IR)
- **Purpose**: Holds the current instruction being executed
- **Format**: [OPCODE][OPERAND] for 2-byte instructions
- **Decoding**: Upper 4 bits = instruction, lower 4 bits = addressing

### 2. Memory System

#### Random Access Memory (RAM)
- **Capacity**: 256 bytes (8-bit addressing)
- **Organization**: 
  - Program area: 0x00-0x7F (128 bytes)
  - Data area: 0x80-0xFF (128 bytes)
- **Access**: Through Memory Address Register (MAR)

#### Memory Address Register (MAR)
- **Purpose**: Holds address for memory operations
- **Size**: 8 bits
- **Source**: PC (instruction fetch) or operand (data access)

### 3. Control Unit

#### Step Counter (SC)
- **Purpose**: Sequences through microcode steps
- **States**: 000 (fetch) → 001 (decode) → 010-110 (execute)
- **Reset**: Returns to 000 after instruction completion

#### Control Logic (CL)
- **Function**: Generates control signals based on:
  - Current instruction (from IR)
  - Current step (from SC)
  - Flag states (N, C, Z)
- **Output**: 16 control signals coordinating all operations

### 4. Input/Output System

#### Input Register
- **Purpose**: Receives data from external devices
- **Interface**: Manual switches, keypad, or serial input
- **Operation**: INP instruction loads value into accumulator

#### Output Register
- **Purpose**: Displays computation results
- **Interface**: LEDs, 7-segment display, or serial output
- **Operation**: OUT instruction shows accumulator content

## Instruction Set

### Data Movement Instructions

| Instruction | Opcode | Size | Description | Example |
|-------------|--------|------|-------------|---------|
| **NOP** | 0000 | 1 byte | No operation | `0000` |
| **LDA** | 0001 | 2 bytes | Load from memory to accumulator | `0001 1000` (load from 0x80) |
| **LDI** | 0010 | 2 bytes | Load immediate to accumulator | `0010 0101` (load value 5) |
| **STA** | 0011 | 2 bytes | Store accumulator to memory | `0011 1000` (store to 0x80) |

### Arithmetic Instructions

| Instruction | Opcode | Size | Flags Updated | Description |
|-------------|--------|------|---------------|-------------|
| **ADD** | 0100 | 2 bytes | Yes | Add memory content to accumulator |
| **ADI** | 0101 | 2 bytes | Yes | Add immediate value to accumulator |
| **SUB** | 0110 | 2 bytes | Yes | Subtract memory content from accumulator |
| **SUI** | 0111 | 2 bytes | Yes | Subtract immediate value from accumulator |

### Control Flow Instructions

| Instruction | Opcode | Size | Condition | Description |
|-------------|--------|------|-----------|-------------|
| **JMP** | 1000 | 2 bytes | Always | Unconditional jump |
| **JPN** | 1001 | 2 bytes | N flag set | Jump if negative |
| **JPP** | 1010 | 2 bytes | N=0, Z=0 | Jump if positive |
| **JPC** | 1011 | 2 bytes | C flag set | Jump if carry |
| **JPZ** | 1100 | 2 bytes | Z flag set | Jump if zero |

### I/O Instructions

| Instruction | Opcode | Size | Description |
|-------------|--------|------|-------------|
| **OUT** | 1101 | 1 byte | Output accumulator to display |
| **INP** | 1110 | 1 byte | Input from keyboard to accumulator |
| **HLT** | 1111 | 1 byte | Halt processor |

## Control Signals

### Signal Definitions

| Signal | Full Name | Purpose |
|--------|-----------|---------|
| **HALT** | Halt | Stops the system clock |
| **FLGU** | Flags Update | Updates flag register from ALU |
| **MARI** | Memory Address Register In | Loads MAR from bus |
| **IRGI** | Instruction Register In | Loads IR from bus |
| **RGAI** | Register A In | Loads accumulator from bus |
| **RGAO** | Register A Out | Puts accumulator on bus |
| **RGBI** | Register B In | Loads register B from bus |
| **RGOI** | Register Output In | Loads output register |
| **RGIO** | Register Input Out | Puts input register on bus |
| **RAMI** | RAM In | Writes bus data to memory |
| **RAMO** | RAM Out | Reads memory data to bus |
| **ALUO** | ALU Out | Puts ALU result on bus |
| **SUBT** | Subtract | Configures ALU for subtraction |
| **PGCI** | Program Counter In | Loads PC from bus |
| **PGCO** | Program Counter Out | Puts PC on bus |
| **PGCC** | Program Counter Count | Increments PC |

### Microcode Table (Simplified)

```
Instruction | Step | Flags | Active Control Signals
------------|------|-------|----------------------
FETCH       | 000  | ---   | MARI, PGCO, PGCC
            | 001  | ---   | RAMO, IRGI
LDA addr    | 010  | ---   | MARI, PGCO, PGCC  
            | 011  | ---   | RAMO, MARI
            | 100  | ---   | RAMO, RGAI
ADD addr    | 010  | ---   | MARI, PGCO, PGCC
            | 011  | ---   | RAMO, MARI  
            | 100  | ---   | RAMO, RGBI
            | 101  | ---   | FLGU (update flags)
            | 110  | ---   | ALUO, RGAI
OUT         | 010  | ---   | RGAO, RGOI
```

## Example Programs

### 1. Countdown Timer
```assembly
# Countdown from input to 0
Address | Code | Label | Instruction | Comment
--------|------|-------|-------------|--------
0x00    | 1110 | start | INP         | Get input number
0x01    | 0111 | loop  | SUI #1      | Subtract 1
0x02    | 0001 |       |             | (immediate value)
0x03    | 1101 |       | OUT         | Display current value
0x04    | 1100 |       | JPZ start   | If zero, restart
0x05    | 0000 |       |             | (jump to start)
0x06    | 1000 |       | JMP loop    | Otherwise continue
0x07    | 0001 |       |             | (jump to loop)
```

### 2. Simple Calculator (Addition)
```assembly
# Add two input numbers
Address | Code | Label | Instruction | Comment
--------|------|-------|-------------|--------
0x00    | 1110 | start | INP         | Get first number
0x01    | 0011 |       | STA temp    | Store temporarily  
0x02    | 0x80 |       |             | (at address 0x80)
0x03    | 1110 |       | INP         | Get second number
0x04    | 0100 |       | ADD temp    | Add first number
0x05    | 0x80 |       |             | (from address 0x80)
0x06    | 1101 |       | OUT         | Display result
0x07    | 1000 |       | JMP start   | Repeat program
0x08    | 0000 |       |             | (jump to start)
...
0x80    | 0000 | temp  | (data)      | Storage location
```

### 3. Multiplication by Repeated Addition
```assembly
# Multiply A × B using repeated addition
Address | Code | Label | Instruction | Comment
--------|------|-------|-------------|--------
0x00    | 1110 | start | INP         | Get multiplicand
0x01    | 0011 |       | STA total   | Initialize total
0x02    | 0x80 |       |             |
0x03    | 0011 |       | STA amount  | Store amount to add
0x04    | 0x81 |       |             |
0x05    | 1110 |       | INP         | Get multiplier
0x06    | 0011 |       | STA count   | Initialize counter
0x07    | 0x82 |       |             |
0x08    | 0001 | loop  | LDA count   | Check counter
0x09    | 0x82 |       |             |
0x0A    | 0111 |       | SUI #1      | Decrement counter
0x0B    | 0001 |       |             |
0x0C    | 1100 |       | JPZ end     | If zero, we're done
0x0D    | 0x14 |       |             |
0x0E    | 0011 |       | STA count   | Save new counter
0x0F    | 0x82 |       |             |
0x10    | 0001 |       | LDA total   | Get current total
0x11    | 0x80 |       |             |
0x12    | 0100 |       | ADD amount  | Add multiplicand
0x13    | 0x81 |       |             |
0x14    | 0011 |       | STA total   | Save new total
0x15    | 0x80 |       |             |
0x16    | 1000 |       | JMP loop    | Continue loop
0x17    | 0x08 |       |             |
0x18    | 0001 | end   | LDA total   | Get final result
0x19    | 0x80 |       |             |
0x1A    | 1101 |       | OUT         | Display result
0x1B    | 1111 |       | HLT         | Stop program
...
0x80    | 0000 | total | (data)      | Accumulates result
0x81    | 0000 | amount| (data)      | Amount to add each time
0x82    | 0000 | count | (data)      | Loop counter
```

## How It All Works Together

### 1. Instruction Execution Cycle

Every instruction follows this basic cycle:

#### Fetch Phase (Steps 000-001)
1. **Step 000**: `MARI ← PC, PC++`
   - Load current PC value into MAR
   - Increment PC for next instruction
2. **Step 001**: `IR ← Memory[MAR]`
   - Read instruction from memory into IR

#### Decode Phase (Step 010)
- Control Logic examines IR to determine instruction type
- Activates appropriate microcode sequence

#### Execute Phase (Steps 011-110)
- Varies by instruction complexity
- May involve multiple memory accesses
- Updates registers and flags as needed

### 2. Example: ADD Instruction Execution

```
Initial State:
- PC = 0x10
- Memory[0x10] = 0100 (ADD opcode)
- Memory[0x11] = 0x80 (operand address)
- Memory[0x80] = 0101 (value 5)
- Accumulator = 0011 (value 3)

Step 000: MARI ← 0x10, PC ← 0x11
Step 001: IR ← 0100 (ADD instruction loaded)
Step 010: MARI ← 0x11, PC ← 0x12 (get operand address)
Step 011: MARI ← 0x80 (set up data address)
Step 100: Register B ← 0101 (load operand)
Step 101: Update flags based on 3 + 5 = 8
Step 110: Accumulator ← 1000 (result of 3 + 5)

Final State:
- PC = 0x12 (ready for next instruction)
- Accumulator = 1000 (value 8)
- Flags: N=1, C=0, Z=0
```

### 3. Conditional Jump Example

```
Current State:
- Accumulator = 0000 (zero)
- Z flag = 1 (set because accumulator is zero)
- PC = 0x05
- Memory[0x05] = 1100 (JPZ opcode)
- Memory[0x06] = 0x10 (jump target)

Execution:
Step 000-001: Fetch JPZ instruction
Step 010: Check Z flag status
Step 011: Since Z=1, load 0x10 into PC
Result: PC = 0x10 (jump taken)

If Z flag were 0:
Step 010: Check Z flag status  
Step 011: Since Z=0, do nothing
Result: PC = 0x07 (continue sequential execution)
```

### 4. Memory Organization Best Practices

```
Memory Layout:
0x00-0x1F: Main program code
0x20-0x3F: Subroutines and functions  
0x40-0x7F: Extended program space
0x80-0xFF: Data storage area

Example Data Area Usage:
0x80: temp1     # Temporary storage
0x81: temp2     # Second temporary  
0x82: counter   # Loop counter
0x83: result    # Final results
0x84-0xFF: Array data or additional variables
```

### 5. Programming Tips

#### Efficient Flag Usage
```assembly
# Compare two numbers (A - B)
LDA num1        # Load first number
SUB num2        # Subtract second number
JPZ equal       # Jump if equal (Z flag set)
JPN less        # Jump if A < B (N flag set)  
JMP greater     # Otherwise A > B
```

#### Loop Patterns
```assembly
# Standard countdown loop
LDI count_val   # Initialize counter
STA counter
loop:
    # ... loop body ...
    LDA counter
    SUI #1      # Decrement
    STA counter
    JPZ done    # Exit when zero
    JMP loop
done:
```

## Simple Operating System (4BitOS)

### OS Architecture

Our 4BitOS provides basic system services while fitting in minimal memory:

```
Memory Layout with OS:
0x00-0x0F: Boot Loader & Interrupt Vectors
0x10-0x2F: Kernel Code (System Calls)
0x30-0x4F: Device Drivers
0x50-0x7F: User Program Space
0x80-0x9F: OS Data Structures
0xA0-0xFF: User Data Space
```

### Boot Sequence

```assembly
# Boot Loader (Address 0x00)
0x00    | 1110 | boot    | INP         | Get boot device ID
0x01    | 0011 |         | STA bootdev | Store boot device
0x02    | 0x80 |         |             |
0x03    | 1000 |         | JMP kernel  | Jump to kernel
0x04    | 0x10 |         |             |

# Interrupt Vector Table
0x05    | 1000 |         | JMP timer_int    | Timer interrupt
0x06    | 0x30 |         |                  |
0x07    | 1000 |         | JMP input_int    | Input interrupt  
0x08    | 0x32 |         |                  |
0x09    | 1000 |         | JMP output_int   | Output interrupt
0x0A    | 0x34 |         |                  |
```

### Kernel Core

```assembly
# Kernel Entry Point (Address 0x10)
kernel:
0x10    | 0010 |         | LDI #0      | Initialize system
0x11    | 0000 |         |             |
0x12    | 0011 |         | STA current_pid | No process running
0x13    | 0x81 |         |             |
0x14    | 0011 |         | STA proc_count  | Zero processes
0x15    | 0x82 |         |             |
0x16    | 1000 |         | JMP scheduler   | Start scheduler
0x17    | 0x20 |         |             |

# System Call Handler
syscall:
0x18    | 0001 |         | LDA syscall_num | Get system call number
0x19    | 0x83 |         |                 |
0x1A    | 0111 |         | SUI #1          | Check if print (syscall 1)
0x1B    | 0001 |         |                 |
0x1C    | 1100 |         | JPZ sys_print   | Jump to print handler
0x1D    | 0x1F |         |                 |
0x1E    | 1111 |         | HLT             | Unknown syscall - halt

sys_print:
0x1F    | 0001 |         | LDA print_value | Get value to print
0x20    | 0x84 |         |                 |
0x21    | 1101 |         | OUT             | Output the value
0x22    | 1000 |         | JMP return_user | Return to user program
0x23    | 0x60 |         |                 |
```

### Simple Scheduler

```assembly
# Round-Robin Scheduler (Address 0x20)
scheduler:
0x20    | 0001 |         | LDA proc_count  | Check if any processes
0x21    | 0x82 |         |                 |
0x22    | 1100 |         | JPZ idle        | If none, go idle
0x23    | 0x2F |         |                 |
0x24    | 0001 |         | LDA current_pid | Get current process
0x25    | 0x81 |         |                 |
0x26    | 0101 |         | ADI #1          | Switch to next process
0x27    | 0001 |         |                 |
0x28    | 0111 |         | SUI #3          | Wrap around at 3 processes
0x29    | 0003 |         |                 |
0x2A    | 1010 |         | JPP no_wrap     | If positive, no wrap needed
0x2B    | 0x2D |         |                 |
0x2C    | 0010 |         | LDI #0          | Reset to process 0
0x2D    | 0000 |         |                 |
no_wrap:
0x2E    | 0011 |         | STA current_pid | Save new current process
0x2F    | 0x81 |         |                 |
idle:
0x30    | 1000 |         | JMP user_space  | Jump to user programs
0x31    | 0x50 |         |                 |
```

## C Programming and Compilation Process

### 1. Simple C Program

```c
// hello.c - Simple C program for 4-bit computer
#include <4bitos.h>

int main() {
    int a, b, result;
    
    // Get two numbers from user
    a = input();
    b = input();
    
    // Add them
    result = a + b;
    
    // Display result
    output(result);
    
    return 0;
}
```

### 2. C Standard Library (4bitos.h)

```c
// 4bitos.h - System header for 4-bit OS
#ifndef _4BITOS_H
#define _4BITOS_H

// System call numbers
#define SYS_PRINT 1
#define SYS_INPUT 2
#define SYS_EXIT  3

// System call wrapper functions
int input(void);
void output(int value);
void exit(int code);

// Implementation using inline assembly
inline int input(void) {
    int result;
    asm volatile("INP" : "=a"(result));
    return result;
}

inline void output(int value) {
    asm volatile("STA 0x84\n\t"    // Store value for syscall
                 "LDI #1\n\t"       // Load syscall number
                 "STA 0x83\n\t"     // Store syscall number  
                 "JMP 0x18"         // Call kernel
                 : : "a"(value));
}

#endif
```

### 3. Compilation Process

#### Stage 1: Preprocessing
```bash
# cpp (C Preprocessor) expands includes and macros
cpp hello.c > hello.i
```

**Result (hello.i):**
```c
// Expanded C code after preprocessing
int input(void) {
    int result;
    asm volatile("INP" : "=a"(result));
    return result;
}

void output(int value) {
    asm volatile("STA 0x84\n\t"
                 "LDI #1\n\t" 
                 "STA 0x83\n\t"
                 "JMP 0x18"
                 : : "a"(value));
}

int main() {
    int a, b, result;
    a = input();
    b = input();
    result = a + b;
    output(result);
    return 0;
}
```

#### Stage 2: Compilation to Assembly
```bash
# gcc compiles C to assembly
gcc -S hello.i -o hello.s
```

**Result (hello.s):**
```assembly
# Generated assembly for 4-bit processor
.text
.global main

main:
    # Function prologue
    LDI #0x7F           # Initialize stack pointer
    STA stack_ptr
    
    # int a = input();
    JMP input_func      # Call input function
    STA var_a           # Store result in variable a
    
    # int b = input();  
    JMP input_func      # Call input function
    STA var_b           # Store result in variable b
    
    # result = a + b;
    LDA var_a           # Load variable a
    ADD var_b           # Add variable b
    STA var_result      # Store in result
    
    # output(result);
    LDA var_result      # Load result
    JMP output_func     # Call output function
    
    # return 0;
    LDI #0              # Load return value
    JMP function_exit   # Exit function

input_func:
    INP                 # Hardware input instruction
    JMP return_address  # Return to caller

output_func:
    STA 0x84           # Store value for system call
    LDI #1             # System call number for print
    STA 0x83
    JMP 0x18           # Jump to kernel syscall handler
    JMP return_address # Return to caller

# Variable storage
var_a:      .byte 0
var_b:      .byte 0  
var_result: .byte 0
stack_ptr:  .byte 0
```

#### Stage 3: Assembly to Machine Code
```bash
# Assembler converts assembly to machine code
as hello.s -o hello.o
```

**Result (hello.o - Object File):**
```
Address | Machine Code | Assembly
--------|-------------|----------
0x50    | 0010 0111   | LDI #0x7F
0x51    | 1111        |
0x52    | 0011 1001   | STA stack_ptr  
0x53    | 1001        |
0x54    | 1000 0110   | JMP input_func
0x55    | 0110        |
0x56    | 0011 1001   | STA var_a
0x57    | 0111        |
0x58    | 1000 0110   | JMP input_func
0x59    | 0110        |
0x5A    | 0011 1001   | STA var_b
0x5B    | 1000        |
0x5C    | 0001 1001   | LDA var_a
0x5D    | 0111        |
0x5E    | 0100 1001   | ADD var_b
0x5F    | 1000        |
0x60    | 0011 1001   | STA var_result
0x61    | 1001        |
0x62    | 0001 1001   | LDA var_result
0x63    | 1001        |
0x64    | 1000 0110   | JMP output_func
0x65    | 1100        |
0x66    | 1110        | input_func: INP
0x67    | 1000 xxxx   | JMP return_address
0x68    | xxxx        |
0x69    | 0011 1000   | output_func: STA 0x84
0x6A    | 0100        |
0x6B    | 0010 0000   | LDI #1
0x6C    | 0001        |
0x6D    | 0011 1000   | STA 0x83
0x6E    | 0011        |
0x6F    | 1000 0001   | JMP 0x18
0x70    | 1000        |
```

#### Stage 4: Linking
```bash
# Linker combines object files and resolves addresses
ld hello.o -o hello.bin
```

**Linker resolves:**
- Function addresses
- Variable locations  
- System call addresses
- Library functions

#### Stage 5: Loading and Execution

```assembly
# OS Loader (simplified)
load_program:
0x40    | 0010 0101   | LDI #0x50    | Set load address
0x41    | 0000        |              |
0x42    | 0011 1000   | STA load_addr | Store load address
0x43    | 1010        |              |
0x44    | 1110        | INP          | Get program size
0x45    | 0011 1000   | STA prog_size | Store program size
0x46    | 1011        |              |

load_loop:
0x47    | 1110        | INP          | Read program byte
0x48    | 0001 1000   | LDA load_addr | Get current address
0x49    | 1010        |              |
0x4A    | 0011        | STA 0x90     | Use as memory address
0x4B    | 1001 0000   |              |
0x4C    | xxxx        | STA (0x90)   | Store program byte
0x4D    | 0001 1000   | LDA load_addr | Increment load address
0x4E    | 1010        |              |
0x4F    | 0101 0000   | ADI #1       |
0x50    | 0001        |              |
0x51    | 0011 1000   | STA load_addr |
0x52    | 1010        |              |
0x53    | 0001 1000   | LDA prog_size | Decrement size counter
0x54    | 1011        |              |
0x55    | 0111 0000   | SUI #1       |
0x56    | 0001        |              |
0x57    | 0011 1000   | STA prog_size |
0x58    | 1011        |              |
0x59    | 1010 0100   | JPP load_loop | Continue if more bytes
0x5A    | 0111        |              |
0x5B    | 1000 0101   | JMP 0x50     | Execute loaded program
0x5C    | 0000        |              |
```

## Complete Execution Flow

### 1. Boot Process
1. **Power On**: CPU starts at address 0x00
2. **Boot Loader**: Initializes system, loads OS kernel
3. **Kernel Init**: Sets up interrupt vectors, device drivers
4. **Scheduler Start**: Begins process management

### 2. Program Compilation & Loading
1. **C Source** → **Preprocessor** → **Compiler** → **Assembly**
2. **Assembly** → **Assembler** → **Object Code**
3. **Object Code** → **Linker** → **Executable**
4. **OS Loader** reads executable into memory

### 3. Program Execution
```
User runs: hello.bin

1. OS Scheduler allocates process ID
2. Loader reads hello.bin into memory at 0x50
3. CPU jumps to 0x50 (program entry point)
4. Program executes:
   - Calls input() → INP instruction → waits for user input
   - User enters: 3
   - Calls input() again → user enters: 5  
   - Performs addition: 3 + 5 = 8
   - Calls output(8) → system call to kernel
   - Kernel executes OUT instruction → displays 8
5. Program returns 0 and exits
6. Scheduler switches to next process
```

### 4. Detailed Execution Trace

```
Time | PC   | Instruction | Accumulator | Memory | Comment
-----|------|-------------|-------------|--------|--------
T1   | 0x50 | LDI #0x7F   | 0111        | -      | Initialize stack
T2   | 0x52 | STA 0x99    | 0111        | [0x99]=0x7F | Store stack pointer  
T3   | 0x54 | JMP 0x66    | 0111        | -      | Call input function
T4   | 0x66 | INP         | 0011        | -      | User inputs 3
T5   | 0x56 | STA 0x97    | 0011        | [0x97]=0x3 | Store in var_a
T6   | 0x58 | JMP 0x66    | 0011        | -      | Call input again
T7   | 0x66 | INP         | 0101        | -      | User inputs 5
T8   | 0x5A | STA 0x98    | 0101        | [0x98]=0x5 | Store in var_b
T9   | 0x5C | LDA 0x97    | 0011        | -      | Load var_a (3)
T10  | 0x5E | ADD 0x98    | 1000        | -      | Add var_b: 3+5=8
T11  | 0x60 | STA 0x99    | 1000        | [0x99]=0x8 | Store result
T12  | 0x62 | LDA 0x99    | 1000        | -      | Load result
T13  | 0x64 | JMP 0x69    | 1000        | -      | Call output function
T14  | 0x69 | STA 0x84    | 1000        | [0x84]=0x8 | Store for syscall
T15  | 0x6B | LDI #1      | 0001        | -      | Load syscall number
T16  | 0x6D | STA 0x83    | 0001        | [0x83]=0x1 | Store syscall number
T17  | 0x6F | JMP 0x18    | 0001        | -      | Jump to kernel
T18  | 0x18 | LDA 0x83    | 0001        | -      | Kernel reads syscall
T19  | 0x1A | SUI #1      | 0000        | -      | Check if print syscall
T20  | 0x1C | JPZ 0x1F    | 0000        | -      | Jump to print handler
T21  | 0x1F | LDA 0x84    | 1000        | -      | Load value to print
T22  | 0x21 | OUT         | 1000        | -      | Display: 8
T23  | 0x22 | JMP 0x60    | 1000        | -      | Return to user program
```

This complete system demonstrates how a modern computer works from the lowest hardware level up through the operating system to high-level programming languages. Even though it's simplified to 4 bits, all the fundamental concepts are present: hardware abstraction, system calls, process management, compilation toolchain, and program execution.