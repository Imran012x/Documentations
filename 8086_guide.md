# Complete Intel 8086 Microprocessor Guide - Everything You Need to Know

## Table of Contents
1. [What is a Microprocessor?](#what-is-a-microprocessor)
2. [The 8086 Story](#the-8086-story)
3. [Basic Architecture - The Factory Analogy](#basic-architecture---the-factory-analogy)
4. [Pin Configuration and Connections](#pin-configuration-and-connections)
5. [Internal Architecture Deep Dive](#internal-architecture-deep-dive)
6. [Memory Organization - The City System](#memory-organization---the-city-system)
7. [Register Set - The Toolbox Collection](#register-set---the-toolbox-collection)
8. [Addressing Modes - Different Ways to Find Things](#addressing-modes---different-ways-to-find-things)
9. [Instruction Set - The Command Language](#instruction-set---the-command-language)
10. [Flag Register - The Status Dashboard](#flag-register---the-status-dashboard)
11. [Interrupt System - The Emergency Response](#interrupt-system---the-emergency-response)
12. [Input/Output Operations](#inputoutput-operations)
13. [Assembly Language Programming](#assembly-language-programming)
14. [Complete Programming Examples](#complete-programming-examples)
15. [System Design and Interfacing](#system-design-and-interfacing)
16. [Performance and Timing](#performance-and-timing)
17. [Comparison with Other Processors](#comparison-with-other-processors)
18. [Legacy and Modern Impact](#legacy-and-modern-impact)

---

## 1. What is a Microprocessor?

**Simple Analogy**: Think of a microprocessor as the **brain of a robot factory**. Just like a factory manager who:
- Reads work orders (instructions)
- Tells workers what to do (controls other components)
- Does calculations (arithmetic operations)
- Remembers important information (stores data)
- Makes decisions based on conditions

A microprocessor is a single chip that contains all the circuits needed to perform these "thinking" operations in a computer.

### Key Characteristics:
- **Single Chip**: Everything fits on one piece of silicon
- **Programmable**: Can follow different sets of instructions
- **General Purpose**: Can be used for many different tasks
- **Digital**: Works with 1s and 0s (binary numbers)

## 2. The 8086 Story

### Historical Context
**Year**: 1978
**Company**: Intel Corporation
**Significance**: First 16-bit microprocessor that became widely successful

**Analogy**: The 8086 was like **upgrading from a bicycle (8-bit processors) to a motorcycle (16-bit)**. It could:
- Handle twice as much data at once
- Address much more memory
- Run faster and more complex programs

### Why 8086 was Revolutionary:
1. **16-bit Architecture**: Could process 16 bits (2 bytes) simultaneously
2. **1MB Memory**: Could address 1,048,576 bytes of memory
3. **Backward Compatible**: Could run older 8-bit software
4. **Affordable**: Made powerful computing accessible to businesses

## 3. Basic Architecture - The Factory Analogy

Think of the 8086 as a **highly organized factory** with different departments:

### The Factory Layout:
```
┌─────────────────────────────────────────────────────────────┐
│                    8086 MICROPROCESSOR FACTORY             │
├─────────────────────────────────────────────────────────────┤
│  EXECUTION UNIT (EU)          │  BUS INTERFACE UNIT (BIU)   │
│  ┌─────────────────────────┐   │  ┌─────────────────────┐    │
│  │ ALU (Calculator Dept)   │   │  │ Segment Registers   │    │
│  │ General Registers       │   │  │ Instruction Queue   │    │
│  │ Flag Register          │   │  │ Address Generation  │    │
│  │ Control Logic          │   │  │ Bus Control Logic   │    │
│  └─────────────────────────┘   │  └─────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
         ↕                                    ↕
    Internal Bus                         External Bus
```

### Two Main Departments:

#### 1. Bus Interface Unit (BIU) - The Shipping & Receiving Department
- **Job**: Handles all communication with outside world
- **Components**:
  - **Segment Registers**: Address departments in the memory city
  - **Instruction Queue**: Pre-fetches instructions (like having orders ready)
  - **Address Generation**: Calculates where to find things
  - **Bus Control**: Manages data traffic

#### 2. Execution Unit (EU) - The Production Department  
- **Job**: Actually does the work (calculations, logic operations)
- **Components**:
  - **ALU**: The calculator that does math and logic
  - **General Registers**: Personal toolboxes for workers
  - **Control Logic**: The supervisor who interprets instructions
  - **Flag Register**: Status board showing current conditions

### How They Work Together:
1. **BIU** fetches instructions from memory (like getting work orders)
2. **BIU** stores instructions in queue (like having a to-do list ready)
3. **EU** takes instructions from queue and executes them
4. **EU** and **BIU** can work simultaneously (pipelining)

## 4. Pin Configuration and Connections

The 8086 comes in a **40-pin DIP (Dual In-line Package)** - think of it as a centipede with 40 legs, each leg having a specific job.

### Pin Categories (The Centipede's Legs):

#### Power Pins (The Life Support):
- **VCC (Pin 20)**: +5V power supply (like electrical outlet)
- **VSS (Pin 11)**: Ground (like electrical ground wire)

#### Clock Pin (The Heartbeat):
- **CLK (Pin 19)**: Clock input (like a metronome keeping time)

#### Address/Data Buses (The Highway System):
- **AD0-AD15 (Pins 2-16, 39)**: Address/Data Bus (16 lanes for traffic)
- **A16/S3-A19/S6 (Pins 35-38)**: Address/Status lines

#### Control Signals (Traffic Lights):
- **ALE (Pin 25)**: Address Latch Enable
- **RD (Pin 32)**: Read signal
- **WR (Pin 29)**: Write signal  
- **M/IO (Pin 28)**: Memory or I/O select
- **DT/R (Pin 27)**: Data Transmit/Receive
- **DEN (Pin 26)**: Data Enable

#### Interrupt Pins (Emergency Signals):
- **INTR (Pin 18)**: Interrupt Request
- **INTA (Pin 24)**: Interrupt Acknowledge
- **NMI (Pin 17)**: Non-Maskable Interrupt

#### Other Control Pins:
- **RESET (Pin 21)**: System reset (like restart button)
- **READY (Pin 22)**: System ready signal
- **HOLD (Pin 31)**: Bus hold request
- **HLDA (Pin 30)**: Hold acknowledge

### Pin Diagram Visualization:
```
        ┌─────┐
    GND │ 1 40│ VCC
   AD14 │ 2 39│ AD15
   AD13 │ 3 38│ A16/S3
   AD12 │ 4 37│ A17/S4
   AD11 │ 5 36│ A18/S5
   AD10 │ 6 35│ A19/S6
    AD9 │ 7 34│ BHE/S7
    AD8 │ 8 33│ MN/MX
    AD7 │ 9 32│ RD
    AD6 │10 31│ HOLD
    AD5 │11 30│ HLDA
        └─────┘
```

## 5. Internal Architecture Deep Dive

### Execution Unit (EU) - The Worker Bee

**Analogy**: Think of EU as a **skilled craftsman** with:

#### ALU (Arithmetic Logic Unit) - The Swiss Army Knife
- **Arithmetic Operations**: Addition, subtraction, multiplication, division
- **Logic Operations**: AND, OR, XOR, NOT
- **Shift Operations**: Moving bits left or right
- **Compare Operations**: Checking if numbers are equal, greater, less

#### General Purpose Registers - The Toolbox
**Like a carpenter's toolbox** with different tools for different jobs:
- **AX**: The main hammer (accumulator for arithmetic)
- **BX**: The wrench (base register for addressing)
- **CX**: The measuring tape (counter for loops)
- **DX**: The screwdriver (data register for I/O)

#### Pointer and Index Registers - The Measuring Tools
- **SP**: Stack pointer (like a bookmark in a stack of papers)
- **BP**: Base pointer (like a reference point on a ruler)
- **SI**: Source index (like pointing to start of copying)
- **DI**: Destination index (like pointing to where to paste)

### Bus Interface Unit (BIU) - The Logistics Manager

**Analogy**: Think of BIU as a **shipping manager** who:

#### Instruction Queue - The Order Buffer
- **6-byte queue**: Like having 6 work orders ready
- **Pre-fetching**: Getting next instructions while current one executes
- **Pipeline Effect**: Keeps the factory running smoothly

#### Segment Registers - The Address Book
- **CS**: Code Segment (where instructions live)
- **DS**: Data Segment (where data lives)  
- **SS**: Stack Segment (where temporary data lives)
- **ES**: Extra Segment (extra storage space)

#### Address Generation - The GPS System
**Calculating Physical Address**:
```
Physical Address = (Segment Register × 16) + Offset
```

**Example**: If CS = 1000h and IP = 0200h
```
Physical Address = (1000h × 10h) + 0200h = 10200h
```

## 6. Memory Organization - The City System

**Analogy**: Think of memory as a **large city** with 1,048,576 houses (1MB), each house having a unique address.

### Segmented Memory Model - The District System

Instead of one long address, the 8086 uses a **district + house number** system:

#### How Addressing Works:
```
┌─────────────┐    ┌──────────────┐    ┌─────────────────┐
│   District  │ +  │ House Number │ =  │ Complete Address│
│ (Segment×16)│    │   (Offset)   │    │   (Physical)    │
└─────────────┘    └──────────────┘    └─────────────────┘
```

#### Memory Map - The City Layout:
```
Memory Address Range    Purpose
┌─────────────────────┬──────────────────────────────────┐
│ 00000h - 003FFh     │ Interrupt Vector Table (1KB)    │
│ 00400h - 004FFh     │ BIOS Data Area (256 bytes)      │
│ 00500h - 9FFFFh     │ User Program Area (640KB-2KB)   │
│ A0000h - AFFFFh     │ EGA/VGA Video Memory (64KB)     │
│ B0000h - B7FFFh     │ Monochrome Video Memory (32KB)  │
│ B8000h - BFFFFh     │ Color Video Memory (32KB)       │
│ C0000h - C7FFFh     │ Video BIOS ROM (32KB)           │
│ C8000h - EFFFFh     │ Reserved/Expansion ROM (160KB)  │
│ F0000h - FFFFFh     │ System BIOS ROM (64KB)          │
└─────────────────────┴──────────────────────────────────┘
```

### Segment Types - Different Neighborhoods:

#### 1. Code Segment (CS) - The Business District
- **Purpose**: Stores program instructions
- **Size**: Up to 64KB
- **Access**: Read-only during execution
- **Pointer**: IP (Instruction Pointer) points to current instruction

#### 2. Data Segment (DS) - The Residential Area
- **Purpose**: Stores program data (variables, arrays)
- **Size**: Up to 64KB  
- **Access**: Read/Write
- **Usage**: Most data operations default to DS

#### 3. Stack Segment (SS) - The Temporary Storage Warehouse
- **Purpose**: Stores temporary data, function parameters, return addresses
- **Size**: Up to 64KB
- **Access**: LIFO (Last In, First Out) like a stack of plates
- **Pointer**: SP (Stack Pointer) points to top of stack

#### 4. Extra Segment (ES) - The Overflow Storage
- **Purpose**: Additional data storage, string operations destination
- **Size**: Up to 64KB
- **Access**: Read/Write
- **Usage**: String operations, extra data space

### Memory Access Examples:

#### Example 1: Reading an Instruction
```
CS = 2000h, IP = 0150h
Physical Address = (2000h × 10h) + 0150h = 20150h
The processor fetches instruction from memory location 20150h
```

#### Example 2: Accessing Data
```
DS = 3000h, Offset = 0080h  
Physical Address = (3000h × 10h) + 0080h = 30080h
Data is read from/written to memory location 30080h
```

#### Example 3: Stack Operation
```
SS = 4000h, SP = 0100h
Physical Address = (4000h × 10h) + 0100h = 40100h
Stack operations occur at memory location 40100h
```

## 7. Register Set - The Toolbox Collection

**Analogy**: Registers are like a **craftsman's toolbox** - each tool has a specific purpose, but some can be used for multiple tasks.

### 16-bit General Purpose Registers - The Main Tools

#### AX Register - The Master Tool (Accumulator)
```
┌─────────┬─────────┐
│   AH    │   AL    │  ← Can be used as two 8-bit registers
└─────────┴─────────┘
│      AX           │  ← Or as one 16-bit register
└───────────────────┘
```
- **Primary Use**: Arithmetic operations, I/O operations
- **Special Features**: Required for multiply/divide operations
- **8-bit Parts**: AH (high byte), AL (low byte)

#### BX Register - The Address Helper (Base)
```
┌─────────┬─────────┐
│   BH    │   BL    │
└─────────┴─────────┘  
│      BX           │
└───────────────────┘
```
- **Primary Use**: Base address for memory access
- **Special Features**: Can hold memory addresses for indirect addressing
- **8-bit Parts**: BH (high byte), BL (low byte)

#### CX Register - The Counter (Count)
```
┌─────────┬─────────┐
│   CH    │   CL    │
└─────────┴─────────┘
│      CX           │  
└───────────────────┘
```
- **Primary Use**: Loop counters, shift/rotate counts
- **Special Features**: Automatically decremented in loop instructions
- **8-bit Parts**: CH (high byte), CL (low byte)

#### DX Register - The Data Porter
```
┌─────────┬─────────┐
│   DH    │   DL    │
└─────────┴─────────┘
│      DX           │
└───────────────────┘
```
- **Primary Use**: Data operations, I/O port addressing
- **Special Features**: Extends AX in multiply/divide operations
- **8-bit Parts**: DH (high byte), DL (low byte)

### Pointer and Index Registers - The Precision Tools

#### SP (Stack Pointer) - The Stack Manager
```
┌───────────────────┐
│        SP         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Points to top of stack
- **Behavior**: Automatically decrements on PUSH, increments on POP
- **Range**: 0000h to FFFFh within stack segment

#### BP (Base Pointer) - The Frame Reference
```
┌───────────────────┐
│        BP         │ ← 16-bit only  
└───────────────────┘
```
- **Purpose**: Base pointer for stack frame access
- **Usage**: Accessing function parameters and local variables
- **Default Segment**: SS (Stack Segment)

#### SI (Source Index) - The Source Pointer
```
┌───────────────────┐
│        SI         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Source index for string operations
- **Usage**: Points to source data in string instructions
- **Default Segment**: DS (Data Segment)

#### DI (Destination Index) - The Destination Pointer
```
┌───────────────────┐
│        DI         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Destination index for string operations  
- **Usage**: Points to destination in string instructions
- **Default Segment**: ES (Extra Segment)

### Segment Registers - The Address Books

#### CS (Code Segment) - The Instruction Address Book
```
┌───────────────────┐
│        CS         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Points to current code segment
- **Usage**: Combined with IP to fetch instructions
- **Size**: Defines up to 64KB code space

#### DS (Data Segment) - The Data Address Book
```
┌───────────────────┐
│        DS         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Points to current data segment
- **Usage**: Default segment for most data operations
- **Size**: Defines up to 64KB data space

#### SS (Stack Segment) - The Stack Address Book
```
┌───────────────────┐
│        SS         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Points to current stack segment
- **Usage**: Combined with SP and BP for stack operations
- **Size**: Defines up to 64KB stack space

#### ES (Extra Segment) - The Extra Address Book
```
┌───────────────────┐
│        ES         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Points to extra data segment
- **Usage**: String operations, additional data storage
- **Size**: Defines up to 64KB extra space

### Special Purpose Registers - The Control Panel

#### IP (Instruction Pointer) - The Program Counter
```
┌───────────────────┐
│        IP         │ ← 16-bit only
└───────────────────┘
```
- **Purpose**: Points to next instruction to execute
- **Behavior**: Automatically incremented after each instruction
- **Range**: 0000h to FFFFh within code segment

#### FLAGS Register - The Status Dashboard
```
Bit: 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
     ┌──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┐
     │  │  │  │  │OF│DF│IF│TF│SF│ZF│  │AF│  │PF│  │CF│
     └──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┘
```

## 8. Addressing Modes - Different Ways to Find Things

**Analogy**: Think of addressing modes as **different ways to give directions** to find something:

### 1. Immediate Addressing - "Take This Exact Thing"
```assembly
MOV AX, 1234h    ; "Put the number 1234h directly into AX"
ADD BX, 50       ; "Add exactly 50 to BX"
```
**Analogy**: Like saying "Take exactly $20" - the value is given directly.

### 2. Register Addressing - "Take What's in This Container"
```assembly
MOV AX, BX       ; "Copy whatever is in BX to AX"
ADD CX, DX       ; "Add whatever is in DX to CX"
```
**Analogy**: Like saying "Take what's in the red box" - referring to a container.

### 3. Direct Addressing - "Go to This Specific Address"
```assembly
MOV AX, [1000h]  ; "Go to memory address 1000h and get the value"
MOV [2000h], BX  ; "Put BX's value at memory address 2000h"
```
**Analogy**: Like saying "Go to 123 Main Street" - a specific location.

### 4. Register Indirect Addressing - "Go Where This Points"
```assembly
MOV AX, [BX]     ; "BX contains an address; go there and get the value"
MOV [SI], CX     ; "SI contains an address; put CX's value there"
```
**Analogy**: Like saying "Go to the address written on this piece of paper."

### 5. Based Addressing - "Base Address Plus Offset"
```assembly
MOV AX, [BX+8]   ; "Go to address (BX + 8) and get the value"
MOV [BP+4], DX   ; "Put DX at address (BP + 4)"
```
**Analogy**: Like saying "Go to Main Street, then 8 houses down."

### 6. Indexed Addressing - "Array Access"
```assembly
MOV AX, [SI+2]   ; "Go to address (SI + 2) and get the value"
MOV [DI+6], BX   ; "Put BX at address (DI + 6)"
```
**Analogy**: Like saying "Go to the 3rd item in the list" (SI+2 for 16-bit items).

### 7. Based Indexed Addressing - "Base Plus Index"
```assembly
MOV AX, [BX+SI]     ; "Go to address (BX + SI) and get the value"
MOV [BP+DI], CX     ; "Put CX at address (BP + DI)"
```
**Analogy**: Like saying "Go to Building BX, Apartment SI."

### 8. Based Indexed with Displacement - "Full Address Calculation"
```assembly
MOV AX, [BX+SI+4]   ; "Go to address (BX + SI + 4) and get the value"
MOV [BP+DI+8], DX   ; "Put DX at address (BP + DI + 8)"
```
**Analogy**: Like saying "Go to Building BX, Apartment SI, Room 4."

### Practical Example - Array Access:
```assembly
; Accessing array elements
MOV BX, OFFSET ARRAY    ; BX points to start of array
MOV SI, 4               ; SI = index × 2 (for 16-bit elements)
MOV AX, [BX+SI]         ; Get ARRAY[2] (element 2)
```

## 9. Instruction Set - The Command Language

**Analogy**: Think of instructions as **commands you give to a very smart but literal robot**. Each command must be precise and unambiguous.

### Data Transfer Instructions - Moving Things Around

#### MOV - The Universal Mover
```assembly
MOV destination, source    ; Copy source to destination
```
**Examples**:
```assembly
MOV AX, BX        ; Copy BX to AX
MOV AX, 1234h     ; Put 1234h into AX
MOV [100h], AX    ; Put AX into memory location 100h
MOV BX, [200h]    ; Get value from memory 200h into BX
```

#### PUSH/POP - The Stack Operations
```assembly
PUSH source       ; Put source on top of stack
POP destination   ; Take top of stack into destination
```
**Examples**:
```assembly
PUSH AX          ; Put AX on stack, SP = SP - 2
POP BX           ; Get top of stack into BX, SP = SP + 2
PUSH 1234h       ; Put immediate value on stack
```

#### XCHG - The Exchanger  
```assembly
XCHG operand1, operand2   ; Exchange values
```
**Examples**:
```assembly
XCHG AX, BX      ; Swap contents of AX and BX
XCHG AX, [100h]  ; Swap AX with memory location 100h
```

### Arithmetic Instructions - The Math Department

#### Addition Family
```assembly
ADD dest, source     ; dest = dest + source
ADC dest, source     ; dest = dest + source + carry flag
INC dest            ; dest = dest + 1
```

#### Subtraction Family
```assembly
SUB dest, source     ; dest = dest - source  
SBB dest, source     ; dest = dest - source - carry flag
DEC dest            ; dest = dest - 1
NEG dest            ; dest = 0 - dest (two's complement)
CMP op1, op2        ; Compare op1 with op2 (sets flags only)
```

#### Multiplication Family
```assembly
MUL source          ; AX = AL × source (8-bit) or DX:AX = AX × source (16-bit)
IMUL source         ; Signed multiplication
```

#### Division Family
```assembly
DIV source          ; AL = AX ÷ source, AH = remainder (8-bit)
                    ; AX = DX:AX ÷ source, DX = remainder (16-bit)
IDIV source         ; Signed division
```

### Logic Instructions - The Decision Makers

#### Bitwise Operations
```assembly
AND dest, source    ; Bitwise AND
OR dest, source     ; Bitwise OR  
XOR dest, source    ; Bitwise XOR (Exclusive OR)
NOT dest           ; Bitwise NOT (complement)
TEST op1, op2      ; AND operation but only sets flags
```

#### Shift and Rotate Operations
```assembly
SHL dest, count    ; Shift Left (logical)
SHR dest, count    ; Shift Right (logical)  
SAL dest, count    ; Shift Arithmetic Left
SAR dest, count    ; Shift Arithmetic Right
ROL dest, count    ; Rotate Left
ROR dest, count    ; Rotate Right
RCL dest, count    ; Rotate through Carry Left
RCR dest, count    ; Rotate through Carry Right
```

### Control Transfer Instructions - The Navigation System

#### Unconditional Jumps
```assembly
JMP destination    ; Jump to destination
CALL procedure     ; Call procedure (pushes return address)
RET               ; Return from procedure (pops return address)
```

#### Conditional Jumps - Based on Flags
```assembly
JZ/JE label       ; Jump if Zero/Equal
JNZ/JNE label     ; Jump if Not Zero/Not Equal
JC/JB label       ; Jump if Carry/Below
JNC/JAE label     ; Jump if No Carry/Above or Equal
JS label          ; Jump if Sign (negative)
JNS label         ; Jump if No Sign (positive)
JO label          ; Jump if Overflow
JNO label         ; Jump if No Overflow
JP/JPE label      ; Jump if Parity/Parity Even
JNP/JPO label     ; Jump if No Parity/Parity Odd
```

#### Signed Comparisons
```assembly
JG/JNLE label     ; Jump if Greater/Not Less or Equal
JGE/JNL label     ; Jump if Greater or Equal/Not Less
JL/JNGE label     ; Jump if Less/Not Greater or Equal  
JLE/JNG label     ; Jump if Less or Equal/Not Greater
```

#### Loop Instructions
```assembly
LOOP label        ; Decrement CX, jump if CX ≠ 0
LOOPE/LOOPZ label ; Loop while Equal/Zero
LOOPNE/LOOPNZ label ; Loop while Not Equal/Not Zero
```

### String Instructions - The Text Processors

#### Basic String Operations
```assembly
MOVS             ; Move string (DS:SI → ES:DI)
MOVSB            ; Move string byte
MOVSW            ; Move string word

CMPS             ; Compare strings (DS:SI with ES:DI)
CMPSB            ; Compare string bytes
CMPSW            ; Compare string words

SCAS             ; Scan string (compare AL/AX with ES:DI)
SCASB            ; Scan string byte
SCASW            ; Scan string word

LODS             ; Load string (DS:SI → AL/AX)
LODSB            ; Load string byte
LODSW            ; Load string word  

STOS             ; Store string (AL/AX → ES:DI)
STOSB            ; Store string byte
STOSW            ; Store string word
```

#### String Prefixes
```assembly
REP              ; Repeat while CX ≠ 0
REPE/REPZ        ; Repeat while Equal/Zero
REPNE/REPNZ      ; Repeat while Not Equal/Not Zero
```

### Processor Control Instructions

#### Flag Control
```assembly
CLC              ; Clear Carry Flag
STC              ; Set Carry Flag  
CMC              ; Complement Carry Flag
CLD              ; Clear Direction Flag
STD              ; Set Direction Flag
CLI              ; Clear Interrupt Flag
STI              ; Set Interrupt Flag
```

#### Processor Control
```assembly
NOP              ; No Operation (do nothing)
HLT              ; Halt processor
WAIT             ; Wait for interrupt
ESC              ; Escape to coprocessor
LOCK             ; Lock bus during next instruction
```

## 10. Flag Register - The Status Dashboard

**Analogy**: The FLAGS register is like the **dashboard of a car** - it shows you the current status and conditions after each operation.

### Flag Register Layout:
```
Bit Position: 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
              ┌──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┐
   Flag Name: │  │  │  │  │OF│DF│IF│TF│SF│ZF│  │AF│  │PF│  │CF│
              └──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┘
   Reserved:   X  X  X  X                 X        X     X
```

### Individual Flag Descriptions:

#### CF (Carry Flag) - Bit 0 - The Overflow Indicator
**Purpose**: Indicates arithmetic carry or borrow
**Set When**: 
- Addition produces carry from MSB  
- Subtraction requires borrow to MSB
- Shift/rotate operations shift out a 1

**Example**:
```assembly
MOV AL, 0FFh    ; AL = 255
ADD AL, 1       ; AL = 0, CF = 1 (carry occurred)
```

#### PF (Parity Flag) - Bit 2 - The Even/Odd Counter  
**Purpose**: Indicates even or odd number of 1-bits in result
**Set When**: Result has even number of 1-bits
**Clear When**: Result has odd number of 1-bits

**Example**:
```assembly
MOV AL, 7       ; AL = 00000111b (three 1-bits, odd)
                ; PF = 0
MOV AL, 3       ; AL = 00000011b (two 1-bits, even)
                ; PF = 1
```

#### AF (Auxiliary Carry Flag) - Bit 4 - The BCD Helper
**Purpose**: Indicates carry from bit 3 to bit 4 (used for BCD arithmetic)
**Set When**: Carry occurs from lower nibble to upper nibble
**Usage**: Primarily for decimal arithmetic adjustments

**Example**:
```assembly
MOV AL, 0Fh     ; AL = 00001111b
ADD AL, 1       ; AL = 00010000b, AF = 1 (carry from bit 3)
```

#### ZF (Zero Flag) - Bit 6 - The Empty Tank Indicator
**Purpose**: Indicates if result is zero
**Set When**: Arithmetic or logic operation result is zero
**Clear When**: Result is non-zero

**Example**:
```assembly
MOV AX, 5
SUB AX, 5       ; AX = 0, ZF = 1
MOV BX, 10
SUB BX, 3       ; BX = 7, ZF = 0
```

#### SF (Sign Flag) - Bit 7 - The Positive/Negative Indicator
**Purpose**: Indicates sign of result (MSB of result)
**Set When**: Result is negative (MSB = 1)
**Clear When**: Result is positive (MSB = 0)

**Example**:
```assembly
MOV AL, 7
SUB AL, 10      ; AL = 0FDh (-3 in two's complement), SF = 1
```

#### TF (Trap Flag) - Bit 8 - The Single-Step Mode
**Purpose**: Enables single-step debugging mode
**Set When**: Processor executes one instruction then generates interrupt
**Usage**: Debugging tools use this for step-by-step execution

#### IF (Interrupt Flag) - Bit 9 - The Interrupt Gate
**Purpose**: Controls whether processor responds to maskable interrupts
**Set When**: Interrupts are enabled (STI instruction)
**Clear When**: Interrupts are disabled (CLI instruction)

**Example**:
```assembly
CLI             ; Clear IF, disable interrupts
; Critical code section
STI             ; Set IF, enable interrupts
```

#### DF (Direction Flag) - Bit 10 - The String Direction Controller
**Purpose**: Controls direction of string operations
**Clear (0)**: String operations increment (forward direction)
**Set (1)**: String operations decrement (backward direction)

**Example**:
```assembly
CLD             ; Clear DF, strings process forward
MOVSB           ; SI and DI increment after operation

STD             ; Set DF, strings process backward  
MOVSB           ; SI and DI decrement after operation
```

#### OF (Overflow Flag) - Bit 11 - The Range Overflow Detector
**Purpose**: Indicates signed arithmetic overflow
**Set When**: Signed operation result exceeds representable range
**Clear When**: No signed overflow occurred

**Example**:
```assembly
MOV AL, 127     ; Maximum positive 8-bit signed number
ADD AL, 1       ; AL = 128 (10000000b = -128), OF = 1
```

### Flag Usage in Practice:

#### Conditional Jump Examples:
```assembly
CMP AX, BX      ; Compare AX with BX
JE EQUAL        ; Jump if ZF = 1 (equal)
JG GREATER      ; Jump if ZF = 0 and SF = OF (greater)
JC CARRY_SET    ; Jump if CF = 1 (carry occurred)
```

#### Flag Manipulation:
```assembly
; Setting flags manually
STC             ; Set CF = 1
CLC             ; Clear CF = 0
STI             ; Set IF = 1 (enable interrupts)
CLI             ; Clear IF = 0 (disable interrupts)
STD             ; Set DF = 1 (backward string direction)
CLD             ; Clear DF = 0 (forward string direction)
```

## 11. Interrupt System - The Emergency Response

**Analogy**: Think of interrupts as the **emergency response system** in a city. Just like fire departments, police, and ambulances respond to emergencies, the processor has an interrupt system to handle urgent events.

### What are Interrupts?

**Definition**: An interrupt is a signal that temporarily stops normal program execution to handle a special event.

**Process**:
1. **Signal Received**: Hardware or software generates interrupt
2. **Current State Saved**: Processor saves current position and status
3. **Handler Executed**: Special routine handles the interrupt
4. **State Restored**: Processor returns to where it left off

### Types of Interrupts:

#### 1. Hardware Interrupts - External Emergency Calls

**Maskable Interrupts (INTR pin)**:
- **Can be disabled** by clearing IF flag
- **Examples**: Keyboard press, mouse movement, disk completion
- **Priority**: Can be ignored if processor is busy with critical tasks

**Non-Maskable Interrupt (NMI pin)**:
- **Cannot be disabled** - always responded to
- **Examples**: Memory parity errors, power failure warnings
- **Priority**: Highest priority - critical system events

#### 2. Software Interrupts - Planned Service Calls

**Format**: `INT n` where n = interrupt number (0-255)

**Examples**:
```assembly
INT 21h         ; DOS services (file operations, screen output)
INT 10h         ; BIOS video services
INT 16h         ; BIOS keyboard services  
INT 13h         ; BIOS disk services
```

#### 3. Internal Interrupts (Exceptions) - Self-Generated Emergencies

**Divide by Zero (INT 0)**:
```assembly
MOV AX, 10
MOV BL, 0
DIV BL          ; Generates INT 0 (divide by zero)
```

**Single Step (INT 1)**:
- Generated when TF flag is set
- Used for debugging

**Breakpoint (INT 3)**:
- One-byte instruction (CCh)
- Used by debuggers

**Overflow (INT 4)**:
- Generated by INTO instruction if OF flag is set

### Interrupt Vector Table - The Emergency Phone Directory

**Location**: Memory addresses 00000h - 003FFh (first 1KB of memory)
**Size**: 256 entries × 4 bytes each = 1024 bytes
**Format**: Each entry contains CS:IP address of interrupt handler

```
Memory Layout:
┌─────────────┬─────────────┬──────────────────────────────┐
│  Address    │ Interrupt # │         Purpose              │
├─────────────┼─────────────┼──────────────────────────────┤
│ 00000h-0003h│    INT 0    │ Divide by Zero               │
│ 00004h-0007h│    INT 1    │ Single Step                  │
│ 00008h-000Bh│    INT 2    │ NMI (Non-Maskable Interrupt) │
│ 0000Ch-000Fh│    INT 3    │ Breakpoint                   │
│ 00010h-0013h│    INT 4    │ Overflow                     │
│ 00014h-0017h│    INT 5    │ Print Screen                 │
│     ...     │     ...     │            ...               │
│ 00084h-0087h│    INT 21h  │ DOS Services                 │
│     ...     │     ...     │            ...               │
│ 003FCh-003FFh│    INT FFh  │ User Defined                 │
└─────────────┴─────────────┴──────────────────────────────┘
```

### Interrupt Processing Steps:

#### When Interrupt Occurs:
1. **Complete Current Instruction**: Finish executing current instruction
2. **Save Flags**: Push FLAGS register onto stack
3. **Clear IF and TF**: Disable interrupts and single-step mode
4. **Save Return Address**: Push CS and IP onto stack
5. **Load New CS:IP**: Get handler address from interrupt vector table
6. **Execute Handler**: Run interrupt service routine
7. **Return**: IRET instruction restores CS, IP, and FLAGS

#### Stack Changes During Interrupt:
```
Before Interrupt:     After Interrupt Entry:
┌─────────────┐      ┌─────────────┐
│             │      │    FLAGS    │ ← New SP
│             │      ├─────────────┤
│             │      │     CS      │
│             │      ├─────────────┤
│             │      │     IP      │
│             │      ├─────────────┤
│             │      │             │
│    SP →     │      │    SP →     │
└─────────────┘      └─────────────┘
```

### Common BIOS and DOS Interrupts:

#### INT 10h - Video Services
```assembly
; Set video mode
MOV AH, 0       ; Function: Set video mode
MOV AL, 3       ; Mode: 80x25 color text
INT 10h

; Display character
MOV AH, 0Eh     ; Function: Display character
MOV AL, 'A'     ; Character to display
INT 10h
```

#### INT 16h - Keyboard Services
```assembly
; Read keystroke
MOV AH, 0       ; Function: Read keystroke
INT 16h         ; AL = ASCII code, AH = scan code

; Check if key available
MOV AH, 1       ; Function: Check keystroke
INT 16h         ; ZF = 1 if no key available
```

#### INT 21h - DOS Services
```assembly
; Display string
MOV AH, 9       ; Function: Display string
LEA DX, MESSAGE ; DX points to string (ending with ')
INT 21h

; Read character from keyboard
MOV AH, 1       ; Function: Read character with echo
INT 21h         ; AL = character read

; Exit program
MOV AH, 4Ch     ; Function: Terminate program
MOV AL, 0       ; Exit code
INT 21h
```

### Custom Interrupt Handler Example:

```assembly
; Custom keyboard interrupt handler
NEW_KEYBOARD_HANDLER:
    PUSH AX         ; Save registers
    PUSH BX
    
    ; Read keyboard port
    IN AL, 60h      ; Read scan code from keyboard
    
    ; Process keystroke (custom logic here)
    ; ... your custom processing ...
    
    ; Send EOI (End of Interrupt) to interrupt controller
    MOV AL, 20h     ; EOI command
    OUT 20h, AL     ; Send to interrupt controller
    
    POP BX          ; Restore registers
    POP AX
    IRET            ; Return from interrupt
```

### Setting Up Custom Interrupt Handler:

```assembly
; Save original handler
MOV AH, 35h         ; DOS function: Get interrupt vector
MOV AL, 09h         ; Interrupt number (keyboard)
INT 21h             ; ES:BX now points to original handler
MOV WORD PTR OLD_HANDLER, BX
MOV WORD PTR OLD_HANDLER+2, ES

; Install new handler
MOV AH, 25h         ; DOS function: Set interrupt vector
MOV AL, 09h         ; Interrupt number
LEA DX, NEW_KEYBOARD_HANDLER ; DS:DX points to new handler
INT 21h

; ... program execution ...

; Restore original handler before exit
PUSH DS
LDS DX, OLD_HANDLER ; Load DS:DX with original handler address
MOV AH, 25h         ; DOS function: Set interrupt vector
MOV AL, 09h         ; Interrupt number
INT 21h
POP DS
```

## 12. Input/Output Operations

**Analogy**: Think of I/O operations as the **mail system** of the processor - it's how the processor communicates with the outside world (keyboard, screen, disk drives, etc.).

### I/O Address Space

The 8086 has a **separate I/O address space** of 64KB (0000h - FFFFh), distinct from memory space.

**Two Types of I/O**:
1. **Port-Mapped I/O**: Separate address space for I/O devices
2. **Memory-Mapped I/O**: I/O devices appear as memory locations

### I/O Instructions:

#### IN - Input from Port (Reading Mail)
```assembly
IN AL, port         ; Read byte from 8-bit port address
IN AX, port         ; Read word from 8-bit port address
IN AL, DX          ; Read byte from 16-bit port address in DX
IN AX, DX          ; Read word from 16-bit port address in DX
```

#### OUT - Output to Port (Sending Mail)
```assembly
OUT port, AL        ; Write byte to 8-bit port address
OUT port, AX        ; Write word to 8-bit port address  
OUT DX, AL         ; Write byte to 16-bit port address in DX
OUT DX, AX         ; Write word to 16-bit port address in DX
```

### Common I/O Ports:

#### System Ports:
```
Port Address    Device/Function
┌─────────────┬──────────────────────────────────────┐
│    20h      │ Interrupt Controller Command         │
│    21h      │ Interrupt Controller Data            │
│    40h-43h  │ Timer/Counter                        │
│    60h      │ Keyboard Data                        │
│    61h      │ Keyboard Control                     │
│    70h      │ CMOS RAM Address                     │
│    71h      │ CMOS RAM Data                        │
│   80h       │ POST (Power-On Self Test) Code       │
│   3F8h-3FFh │ COM1 Serial Port                     │
│   2F8h-2FFh │ COM2 Serial Port                     │
│   378h-37Fh │ LPT1 Parallel Port                   │
│   278h-27Fh │ LPT2 Parallel Port                   │
└─────────────┴──────────────────────────────────────┘
```

### I/O Examples:

#### Reading Keyboard Status:
```assembly
; Check if key is pressed
IN AL, 60h          ; Read keyboard data port
TEST AL, 80h        ; Check if key released (bit 7 = 1)
JZ KEY_PRESSED      ; Jump if key pressed (bit 7 = 0)
```

#### Controlling Speaker:
```assembly
; Turn on speaker
IN AL, 61h          ; Read port 61h (system control)
OR AL, 3            ; Set bits 0 and 1
OUT 61h, AL         ; Write back to port

; Set frequency (example: 1000 Hz)
MOV AX, 1193180     ; Timer frequency
MOV BX, 1000        ; Desired frequency
DIV BX              ; AX = count value
OUT 42h, AL         ; Send low byte to timer
MOV AL, AH
OUT 42h, AL         ; Send high byte to timer
```

#### Serial Port Communication:
```assembly
; Initialize COM1 (3F8h base address)
MOV DX, 3FBh        ; Line Control Register
MOV AL, 80h         ; Set DLAB (Divisor Latch Access Bit)
OUT DX, AL

; Set baud rate (9600 bps)
MOV DX, 3F8h        ; Divisor Latch Low
MOV AL, 12          ; Low byte of divisor for 9600 bps
OUT DX, AL
MOV DX, 3F9h        ; Divisor Latch High  
MOV AL, 0           ; High byte of divisor
OUT DX, AL

; Configure line parameters
MOV DX, 3FBh        ; Line Control Register
MOV AL, 3           ; 8 data bits, 1 stop bit, no parity
OUT DX, AL

; Send character
SEND_CHAR:
    MOV DX, 3FDh    ; Line Status Register
    IN AL, DX       ; Read status
    TEST AL, 20h    ; Check if transmitter ready
    JZ SEND_CHAR    ; Wait if not ready
    
    MOV DX, 3F8h    ; Data Register
    MOV AL, 'H'     ; Character to send
    OUT DX, AL      ; Send character
```

### Memory-Mapped I/O Example:

#### VGA Text Mode Display:
```assembly
; Display character at specific screen position
; Screen memory starts at B8000h (color text mode)
MOV AX, 0B800h      ; Video segment
MOV ES, AX          ; Set ES to video segment

; Calculate position: (row * 80 + column) * 2
MOV AX, 10          ; Row 10
MOV BX, 80          ; 80 columns per row
MUL BX              ; AX = row * 80
ADD AX, 20          ; Add column 20
SHL AX, 1           ; Multiply by 2 (char + attribute)
MOV DI, AX          ; DI = offset in video memory

; Store character and attribute
MOV AL, 'X'         ; Character to display
MOV AH, 0Fh         ; Attribute (white on black)
STOSW               ; Store character and attribute
```

## 13. Assembly Language Programming

**Analogy**: Assembly language is like **writing detailed instructions for a very literal robot** - every step must be explicit and in the correct order.

### Program Structure:

#### Basic Program Template:
```assembly
; Program comments start with semicolon
.MODEL SMALL            ; Memory model specification
.STACK 100h            ; Stack size (256 bytes)

.DATA                  ; Data segment
    ; Variable declarations go here
    MESSAGE DB 'Hello, World!
    NUMBER DW 1234h
    ARRAY DB 10 DUP(?)

.CODE                  ; Code segment
START:                 ; Program entry point
    ; Initialize data segment
    MOV AX, @DATA
    MOV DS, AX
    
    ; Main program code goes here
    
    ; Exit program
    MOV AH, 4Ch        ; DOS terminate function
    INT 21h            ; Call DOS
    
END START              ; End of program
```

### Memory Models:

#### Different Memory Models:
```assembly
.MODEL TINY            ; Code + Data + Stack ≤ 64KB (COM files)
.MODEL SMALL           ; Code ≤ 64KB, Data ≤ 64KB (most common)
.MODEL MEDIUM          ; Code > 64KB, Data ≤ 64KB  
.MODEL COMPACT         ; Code ≤ 64KB, Data > 64KB
.MODEL LARGE           ; Code > 64KB, Data > 64KB
.MODEL HUGE            ; Code > 64KB, Data > 64KB, arrays > 64KB
```

### Data Declaration:

#### Data Types:
```assembly
.DATA
    ; Byte declarations (8-bit)
    CHAR1 DB 'A'           ; Single character
    CHAR2 DB 65            ; ASCII value of 'A'
    CHAR3 DB ?             ; Uninitialized byte
    
    ; Word declarations (16-bit)
    NUM1 DW 1234h          ; Hexadecimal word
    NUM2 DW 5678           ; Decimal word
    NUM3 DW ?              ; Uninitialized word
    
    ; Double word declarations (32-bit)
    LONGNUM DD 12345678h   ; 32-bit value
    
    ; Arrays
    BUFFER DB 100 DUP(?)   ; 100 uninitialized bytes
    NUMBERS DW 5 DUP(0)    ; 5 words initialized to 0
    GRADES DB 85,90,78,92,88  ; Array with initial values
    
    ; Strings
    MSG1 DB 'Hello       ; String with DOS terminator
    MSG2 DB 'World',0      ; String with null terminator
    MSG3 DB 'Test',13,10,' ; String with CR/LF
```

### Procedures and Functions:

#### Simple Procedure:
```assembly
; Procedure to display a character
DISPLAY_CHAR PROC
    PUSH AX             ; Save registers
    PUSH DX
    
    MOV DL, AL          ; Character to display
    MOV AH, 2           ; DOS display character function
    INT 21h
    
    POP DX              ; Restore registers
    POP AX
    RET                 ; Return to caller
DISPLAY_CHAR ENDP

; Calling the procedure
MOV AL, 'A'             ; Character to display
CALL DISPLAY_CHAR       ; Call procedure
```

#### Procedure with Parameters:
```assembly
; Procedure to add two numbers
; Parameters: AX = first number, BX = second number
; Returns: AX = sum
ADD_NUMBERS PROC
    ADD AX, BX          ; Add the numbers
    RET                 ; Return with result in AX
ADD_NUMBERS ENDP

; Using the procedure
MOV AX, 10              ; First parameter
MOV BX, 20              ; Second parameter  
CALL ADD_NUMBERS        ; Call procedure
; AX now contains 30
```

### Conditional Programming:

#### IF-THEN-ELSE Structure:
```assembly
; Compare two numbers and display result
MOV AX, NUM1
CMP AX, NUM2            ; Compare NUM1 with NUM2
JE EQUAL                ; Jump if equal
JG GREATER              ; Jump if NUM1 > NUM2
JL LESS                 ; Jump if NUM1 < NUM2

GREATER:
    LEA DX, MSG_GREATER ; Load message address
    JMP DISPLAY         ; Jump to display
    
LESS:
    LEA DX, MSG_LESS    ; Load message address
    JMP DISPLAY         ; Jump to display
    
EQUAL:
    LEA DX, MSG_EQUAL   ; Load message address
    
DISPLAY:
    MOV AH, 9           ; DOS display string function
    INT 21h             ; Display message
```

### Loop Programming:

#### FOR Loop (using CX counter):
```assembly
; Print numbers 1 to 10
MOV CX, 10              ; Loop counter
MOV BX, 1               ; Starting number

LOOP_START:
    PUSH CX             ; Save loop counter
    
    ; Display current number (simplified)
    MOV AX, BX          ; Current number
    ADD AL, '0'         ; Convert to ASCII
    MOV DL, AL          ; Character to display
    MOV AH, 2           ; DOS display character
    INT 21h
    
    INC BX              ; Next number
    POP CX              ; Restore loop counter
    LOOP LOOP_START     ; Decrement CX and loop if not zero
```

#### WHILE Loop:
```assembly
; Read characters until Enter is pressed
INPUT_LOOP:
    MOV AH, 1           ; DOS read character function
    INT 21h             ; Read character into AL
    
    CMP AL, 13          ; Check for Enter key (ASCII 13)
    JE INPUT_DONE       ; Exit if Enter pressed
    
    ; Process character here
    ; ...
    
    JMP INPUT_LOOP      ; Continue loop
    
INPUT_DONE:
    ; Continue with rest of program
```

### String Processing:

#### String Length Calculation:
```assembly
; Calculate length of null-terminated string
; DS:SI points to string, returns length in CX
STRING_LENGTH PROC
    PUSH SI             ; Save SI
    XOR CX, CX          ; Clear length counter
    
LENGTH_LOOP:
    LODSB               ; Load byte from DS:SI into AL, increment SI
    CMP AL, 0           ; Check for null terminator
    JE LENGTH_DONE      ; Exit if found
    INC CX              ; Increment length
    JMP LENGTH_LOOP     ; Continue
    
LENGTH_DONE:
    POP SI              ; Restore SI
    RET                 ; Return with length in CX
STRING_LENGTH ENDP
```

#### String Copy:
```assembly
; Copy string from DS:SI to ES:DI
STRING_COPY PROC
    PUSH SI             ; Save registers
    PUSH DI
    
COPY_LOOP:
    LODSB               ; Load byte from source
    STOSB               ; Store byte to destination
    CMP AL, 0           ; Check for null terminator
    JNE COPY_LOOP       ; Continue if not end
    
    POP DI              ; Restore registers
    POP SI
    RET
STRING_COPY ENDP
```

## 14. Complete Programming Examples

### Example 1: Simple Calculator

```assembly
; Simple Calculator Program
.MODEL SMALL
.STACK 100h

.DATA
    MSG1 DB 'Enter first number (0-9): 
    MSG2 DB 13,10,'Enter second number (0-9): 
    MSG3 DB 13,10,'Enter operation (+,-,*,/): 
    MSG4 DB 13,10,'Result: 
    ERROR_MSG DB 13,10,'Error: Invalid operation!
    
    NUM1 DB ?
    NUM2 DB ?
    OPERATION DB ?
    RESULT DW ?

.CODE
START:
    ; Initialize data segment
    MOV AX, @DATA
    MOV DS, AX
    
    ; Get first number
    LEA DX, MSG1
    MOV AH, 9
    INT 21h
    
    MOV AH, 1               ; Read character
    INT 21h
    SUB AL, '0'             ; Convert ASCII to number
    MOV NUM1, AL
    
    ; Get second number
    LEA DX, MSG2
    MOV AH, 9
    INT 21h
    
    MOV AH, 1
    INT 21h
    SUB AL, '0'
    MOV NUM2, AL
    
    ; Get operation
    LEA DX, MSG3
    MOV AH, 9
    INT 21h
    
    MOV AH, 1
    INT 21h
    MOV OPERATION, AL
    
    ; Perform calculation
    MOV AL, NUM1            ; Load first number
    MOV BL, NUM2            ; Load second number
    
    CMP OPERATION, '+'
    JE DO_ADD
    CMP OPERATION, '-'
    JE DO_SUB
    CMP OPERATION, '*'
    JE DO_MUL
    CMP OPERATION, '/'
    JE DO_DIV
    
    ; Invalid operation
    LEA DX, ERROR_MSG
    MOV AH, 9
    INT 21h
    JMP EXIT_PROGRAM
    
DO_ADD:
    ADD AL, BL
    JMP DISPLAY_RESULT
    
DO_SUB:
    SUB AL, BL
    JMP DISPLAY_RESULT
    
DO_MUL:
    MUL BL                  ; AX = AL * BL
    JMP DISPLAY_RESULT
    
DO_DIV:
    CMP BL, 0               ; Check for division by zero
    JE DIVISION_ERROR
    XOR AH, AH              ; Clear AH for division
    DIV BL                  ; AL = AX / BL
    JMP DISPLAY_RESULT
    
DIVISION_ERROR:
    LEA DX, ERROR_MSG
    MOV AH, 9
    INT 21h
    JMP EXIT_PROGRAM
    
DISPLAY_RESULT:
    MOV RESULT, AX          ; Store result
    
    ; Display result message
    LEA DX, MSG4
    MOV AH, 9
    INT 21h
    
    ; Convert and display result
    MOV AX, RESULT
    CMP AX, 9               ; Check if single digit
    JLE SINGLE_DIGIT
    
    ; Multi-digit number (simplified for two digits)
    MOV BL, 10
    DIV BL                  ; AL = tens, AH = ones
    
    ADD AL, '0'             ; Convert tens to ASCII
    MOV DL, AL
    MOV AH, 2
    INT 21h
    
    MOV AL, AH              ; Get ones digit
    
SINGLE_DIGIT:
    ADD AL, '0'             ; Convert to ASCII
    MOV DL, AL
    MOV AH, 2
    INT 21h
    
EXIT_PROGRAM:
    MOV AH, 4Ch
    INT 21h
    
END START
```

### Example 2: Array Processing - Finding Maximum

```assembly
; Find Maximum Value in Array
.MODEL SMALL
.STACK 100h

.DATA
    ARRAY DB 23, 45, 12, 67, 34, 89, 56, 78, 91, 43
    ARRAY_SIZE EQU 10
    MAX_VALUE DB ?
    MSG1 DB 'Array values: 
    MSG2 DB 13,10,'Maximum value: 
    SPACE DB ' 

.CODE
START:
    MOV AX, @DATA
    MOV DS, AX
    
    ; Display array values
    LEA DX, MSG1
    MOV AH, 9
    INT 21h
    
    LEA SI, ARRAY           ; Point to array
    MOV CX, ARRAY_SIZE      ; Loop counter
    
DISPLAY_LOOP:
    MOV AL, [SI]            ; Get array element
    CALL DISPLAY_NUMBER     ; Display number
    
    ; Display space
    LEA DX, SPACE
    MOV AH, 9
    INT 21h
    
    INC SI                  ; Next array element
    LOOP DISPLAY_LOOP
    
    ; Find maximum value
    LEA SI, ARRAY           ; Reset to start of array
    MOV AL, [SI]            ; Initialize max with first element
    MOV MAX_VALUE, AL
    INC SI                  ; Move to second element
    MOV CX, ARRAY_SIZE - 1  ; Remaining elements
    
FIND_MAX_LOOP:
    MOV AL, [SI]            ; Get current element
    CMP AL, MAX_VALUE       ; Compare with current max
    JLE NOT_GREATER         ; Skip if not greater
    MOV MAX_VALUE, AL       ; Update maximum
    
NOT_GREATER:
    INC SI                  ; Next element
    LOOP FIND_MAX_LOOP
    
    ; Display maximum value
    LEA DX, MSG2
    MOV AH, 9
    INT 21h
    
    MOV AL, MAX_VALUE
    CALL DISPLAY_NUMBER
    
    ; Exit program
    MOV AH, 4Ch
    INT 21h

; Procedure to display a number (0-99)
DISPLAY_NUMBER PROC
    PUSH AX
    PUSH BX
    PUSH DX
    
    XOR AH, AH              ; Clear AH
    MOV BL, 10              ; Divisor
    DIV BL                  ; AL = tens, AH = ones
    
    ; Display tens digit (if not zero)
    CMP AL, 0
    JE DISPLAY_ONES
    ADD AL, '0'
    MOV DL, AL
    MOV AH, 2
    INT 21h
    
DISPLAY_ONES:
    MOV AL, AH              ; Get ones digit
    ADD AL, '0'
    MOV DL, AL
    MOV AH, 2
    INT 21h
    
    POP DX
    POP BX
    POP AX
    RET
DISPLAY_NUMBER ENDP

END START
```
    
