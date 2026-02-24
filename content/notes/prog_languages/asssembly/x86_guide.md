+++
title = 'x86 Guide'
date = 2024-02-11

+++

![x86 Registers](/images/x86_registers.png)

### Memory and Addressing Modes

#### Declaring Static Data Regions

- Can declare static data regions (analogous to global variables).
- Data declarations should be preceded by `.DATA` directive.
- Following this directive, the directives `DB`, `DW`, and `DD` can be used to declare one, two or four byte data locations.
- Example declarations:

```
.DATA
var  DB 64 ; declare a byte, referred to as location var, contains 64
var2 DB ?  ; declare an uninitialized byte
       DB 10 ; declare a byte with no label containing the value 10
             ; location is var2 + 1
X    DW ?
Y    DD 30000
```

```
Z   DD 1,2,3  ; Declare three 4-byte values, initialized to 1, 2, 3
bytes DB 10   ; Declare 10 uninitialized bytes starting at location bytes
        DUP(?)
arr DD 100    ; Declare 100 4-byte words starting at location arr
      DUP(0)    ; all initialized to 0
str DB 'hello',0 ; Declare 6 bytes starting at the address str, initialized to the ASCII character values for hello and the null(0) byte.
```

#### Addressing Memory

- Some examples of mov instructions using address computations are:

```
mov eax, [ebx] ; Move the 4 bytes in memory at the address contained in EBX into EAX
mov [var], ebx ; Move the contents of EBX into the 4 bytes at memory address var
mov eax, [esi-4] ; Move 4 bytes at memory address ESI + (-4) into EAX
mov [esi+eax], cl ; Move the contents of CL into the byte at address ESI+EAX
mov edx [esi+4*ebx] ; Move the 4 bytes of data at address ESI + 4*EBX into EDX
```

#### Size Directives

- The size directives `BYTE PTR`, `WORD PTR` and `DWORD PTR` indicates the sizes of 1, 2, and 4 bytes respectively.

```
mov BYTE PTR [ebx],2  ; Move 2 into the single byte at the address stored in EBX
mov WORD PTR [ebx],2  ; Move the 16-bit integer representation of 2 into the 2 bytes starting at the address in EBX
mov DWORD PTR [ebx],2 ; Move the 32-bit integer representation of 2 into the 4 bytes starting at the address in EBX.
```

### Instructions

- Machine instructions generally fall into three categories: data movement, arithmetic/logic, and control-flow.

#### Data Movement Instructions

- **mov** - Move : The mov instruction copies the data item referred to by its second operand (i.e. register contents, memory contents, or a constant value) into the location referred to by its first operand (i.e. a register or memory).
- **push** - Push : The push instruction places its operand onto the top of the hardware supported stack in memory. Specifically, push first decrements ESP by 4, then places its operand into the contents of the 32-bit location at address [ESP].
- **pop** - Pop stack : The pop instruction removes the 4-byte data element from the top of the hardware-supported stack into the specified operand (i.e. register or memory location). It first moves the 4 bytes located at memory location [SP] into the specified register or memory location, and then increments SP by 4
- **lea** - Load effective address : The lea instruction places the address specified by its second operand into the register specified by its first operand. This is useful for obtaining a pointer into a memory region. `lea <reg32>,<mem>`

#### Arithmetic and Logic Instructions

- **add** - Integer Addition : The add instruction adds together its two operands, storing the result in its first operand. Note, whereas both operands may be registers, at most one operand may be a memory location
- **sub** — Integer Subtraction : The sub instruction stores in the value of its first operand the result of subtracting the value of its second operand from the value of its first operand. As with add
- **inc**, **dec**
- **imul**
- **idiv** : The idiv instruction divides the contents of the 64 bit integer EDX:EAX (constructed by viewing EDX as the most significant four bytes and EAX as the least significant four bytes) by the specified operand value. The quotient result of the division is stored into EAX, while the remainder is placed in EDX. `idiv <reg32>` `idiv <mem>`. `idiv ebx` - divide the contents of EDX:EAX by the contents of EBX.
- **and**, **or**, **xor**, **not**
- **neg** peforms two's complement negation of the operand contents.
- **shl**, **shr**

#### Control Flow Instructions

- **jmp**
- **jcondition** These instructions are conditional jumps that are based on the status of a set of condition codes that are stored in a special register called the _machine status word_.
- **cmp**
- **call**, **ret** - Subroutine call and return

### Calling Convention

- The calling convention is a protocol about how to call and return from routines.
  ![Stack during Subroutine call](/images/stack.png)

## Programming from the ground up

- movl (%esp), %eax : moves whatever is at the top of the stack into %eax
- movl %esp, %eax : %eax would just hold the pointer to the top of the stack rather than the value at the top
- movl 4(%esp), %eax : access the value right below the top of the stack. This instruction uses the base pointer addressing modes which simply adds 4 to %esp before looking up the value being pointed to.
- C language calling convention:
  1. Before executing a function, a program pushes all of the parameters for the function onto the stack in the reverse order that they are documented.
  2. The the program issues a `call` instruction indicating which function it wishes to start.
  3. The `call` instruction does two things:
     1. It pushes the address of the next instruction i.e return address, onto the stack.
     2. It modifies the instruction pointer (%eip) to point to the start of the function.
- as --32 power.s -o power.o
- ld -m elf_i386 factorial.o -o factorial
- gcc -S -m32 try.c
- stdin : file descriotor always 0
- stdout: file descriptor always 1
- stderr : file descriptor always 2
- movb (%eax,%edi,1), %cl : It is using an indexed indirect addressing mode. It says to start at %eax and go %edi locations forward, with each location being 1 byte big. It takes the value found there, and put it in %cl.

### x86_64 Assembly

- rax - temporary register; when we call a syscall, rax must contain syscall number
- rdi - 1st arg
- rsi - 2nd arg
- rdx - 3rd arg
- rcx - 4th arg
- r8 - 5th arg
- r9 - 6th arg
- We have 16 general-purpose registers : RAX, RBX, RCX, RDX, RDI, RSI, RBP, RSP and R8-R15
- RBP - points to the base of the current stack frame.
- RSP - points to the top of the current stack frame.
- `push` - increments RSP and stores argument in location pointed by stack pointer.
- `pop` - copy data to argument from location pointed by stack pointer.
- `size_t sys_write(unsigned int fd, const char * buf, size_t count)` sys_write syscall. It has value 1 in syscall table.
- 60 is a number of exit syscall.
- Syscall is the way a user level program asks the operating system to do something for it.
- `data` - section is used for declaring initialized data or constants.
- `bss` - section is used for declaring non initialized variables
- `text` - section is used for code.
- The fundamental data types are bytes, words (2 bytes), doublewords (4 bytes), quadwords (8 bytes), and double quadwords (16 bytes)
- NASM supports a number of pseudo-instructions:
  - DB, DW, DD, DQ, DT, DO, DY and DZ - used for declaring initialized data.
  - RESB, RESW, RESD, RESQ, REST, RESO, RESY, RESZ - used for declaring non initialized variables.
  - INCBIN - includes external binary files
  - EQU - defines constant for e.g - `one equ 1`
  - TIMES - Repeating instructions or Data
- Arithmetic operations
  - ADD, SUB
  - MUL - unsigned multiply
  - IMUL - signed multiply
  - DIV - unsigned divide
  - IDIV - signed divide
  - INC, DEC
  - NEG - negate
- String operations
  - REP - repeat while rcx is not zero
  - MOVSB - copy a string of bytes (MOVSW, MOVSD and etc..)
  - CMPSB - byte string comparison
  - SCASB - byte string scanning
  - STOSB - write byte to string
- cld - resets df flag to zero.
- `$` - returns position in memory of string where $ defined
- `$$` - returns position in memory of current section start
- NASM supports two form of macro
  - single-line
  - multiline
- All single-line macro must start from %define directive. `%define macro_name(parameter) value`
- Multiline macro starts with %macro nasm directive and end with %endmacro. General form is following:

```
%macro number_of_parameters
  instruction
  instruction
  instruction
%endmacro
```

- All labels defined in a macro must start with %%
- NASM supports fillowing standard macros
- STRUC : we can use `STRUC` and `ENDSTRUC` for data structure definition.

```
struc person
  name: resb 10
  age : resb 1
endstruc
```

- We can include other asssembly files and jump to there labels or call functions with %include directive.
- GNU assembler has 6 postfixes for operations:
  - `b` - 1 byte operands
  - `w` - 2 bytes operands
  - `l` - 4 bytes operands
  - `q` - 8 bytes operands
  - `t` - 10 bytes operands
  - `o` - 16 bytes operands
- The rule is also applicable for another like `addl`, `xorb`, `cmpw`
- GNU assembler supports following operators for far functions call and jumps : `lcall $section, $offset`
- Far jump - a jump to an instruction located in a different segment than the current code segment but at the same privilege level, sometimes referred to as an intersegment jump.
- We can use C together with assembler in 3 ways:
  - Call assembly routines from C code
  - Call C routines from assembly code
  - Use inline assembly in C code
- How to build C and assembly code:

```
nasm -f elf64 -o casm.o casm.asm
gcc casm.o casm.c -o casm
```

- INLINE assembly
- The following method is to write assembly code directly in C code. The syntax is:
  - `asm [volatile] ("assembly code" : output operand : input operand : clobbers);`
- There are three floating point data types: single-precision, double-precision, double-extended-precision
- Single precision number : sign (1 bit , exponent (8 bits), mantissa (23 bits)
- Double precision number : sign (1 bit), exponent (11 bit), mantissa (52 bits)
- Extended precision : sign (1 bit), exponent (15 bit), mantissa (112 bits)
- x87 FPU - floating point unit provides high-performance floating-point processing.
- FPU has eight 10 byte registers organized in a ring stack. Top of the stack - ST(0), other registers are ST(1), ST(2)...ST(7)

```
section .data
  x dw 1.0
fld dword [x] ; pushes value of x to stack.
```

### NASM Tutorial

- NASM is line based. Most programs consist of **directives** followed by one or more **sections**. Line can have an optional **label**. Most lines have an **instruction** followed by zero or more **operands**
- ![nasm](/images/nasm.png)

- ![registers](/images/registers.png)
- These are the basic forms of addressing:
  - [ number ]
  - [ reg ]
  - [ reg + reg * scale ] scale is 1, 2, 4, 8
  - [ reg + number ]
  - [reg + reg*scale + number ]
- The number is called the **displacement**; the plain register is called the **base**; the register with the scale is called the **index**
- Examples

```
[750]                       ; displacement only
[rbp]                       ; base register only
[rcx + rsi*4]               ; base + index * scale
[rbp + rdx]                 ; scale is 1
[rbx - 8]                   ; displacement is -8
[rax + rdi*8 + 500]         ; all four components
[rbx + counter]             ; uses the address of the variable 'counter' as the displacement
```

- Immediate Operands

```
200           ; decimal
0200          ; still decimal - the leading 0 does not make it octal
0200d         ; explicitly decimal - d suffix
0d200         ; also decimal - 0d prefix
0c8h          ; hex - h suffix, but leading 0 is required because c8h looks like a var
0xc8          ; hex - the classic 0x prefix
0hc8          ; hex - for some reason NASM likes 0h
310q          ; octal - q suffix
0q310         ; octal - 0q prefix
11001000      ; binary - b suffix
0b1100_1000   ; binary - 0b prefix, underscores are allowed
```

- Defining Data and Reserving Space

```
db  0x55                  ; just the byte 0x55
db  0x55,0x56,0x57        ; three bytes in succession
db  'a',0x55              ; character constants are OK
db  'hello',13,10,'$'     ; so are string constants
dw  0x1234                ; 0x34 0x12
dw  'a'                   ; 0x61 0x00 (it's just a number)
dw  'ab'                  ; 0x61 0x62 (character constant)
dw  'abc'                 ; 0x61 0x62 0x63 0x00 (string)
dd  0x12345678            ; 0x78 0x56 0x34 0x12
dd  1.234567e20           ; floating-point constant
dq  0x123456789abcdef0    ; eight byte constant
dq  1.234567e20           ; double-precision float
dt  1.234567e20           ; extended-precision float
```

- To reserve space (without initializing), you can use the following pseudo instructions. They should go in a section called `.bss`. .bss section is for _writable_ data

```
buffer:     resb  64      ; reserve 64 bytes
wordvar:    resw  1       ; reserve a word
realarray:  resq  10      ; array of ten reals
```

- Calling Conventions
- This information can be found in [Wikipedia](http://en.wikipedia.org/wiki/X86_calling_conventions#x86-64_Calling_Conventions)
- The most important points are:
  - From left to right, pass as many parameters as will fit in registers. The order in which registers are allocated are
    - For integers and pointers : rdi, rsi, rdx, rcx, r8, r9
    - For floating-point : xmm0..xmm7
- Additional parameters are pushed on the stack, right to left, and are to be _removed_ by the caller after the call.
- After the parameters are pushed, the call instruction is made, so when the called function gets control, the return address is at [rsp], the first memory parameter is at `[rsp + 8]`, etc.
- The stack pointer `rsp` must be aligned to a 16-byte boundary before making a call.
- The only registers that the called function is required to preserve (the calle-save registers) are: rbp, rbx, r12, r13, r14, r15
- The callee is also supposed to save the control bits of the XMCSR and the x87 control word
- Integers are returned in `rax` or `rdx:rax`, and floating point values are returned in `xmm0` or `xmm1:xmm0`
- After an arithmetic or logic instruction, or the compare instruction, `cmp`, the processor sets or clears bits in its `rflags`. The most interesting are: s - sign, z - zero, c - carry, o - overflow
- The conditional instructions have three base forms: `j` for conditional jum, `cmov` for conditional move, and `set` for conditional set. The suffix of the instruction has one of the 30 forms: s ns z nz c nc o no p np pe po e ne l nl le nle g ng ge nge a na ae nae b nb be nbe.
- Some processor instructions that convert between integers and floating point values.
  - `cvtsi2sd` xmmreg, r/m32 xmmreg[63..0] <- intToDouble(r/m32)
  - `cvtsi2ss` xmmreg, r/m32 xmmreg[31..0] <- intToFloat(r/m32)
  - `cvtsd2si` reg32, xmmr/m64 reg32 <- doubleToInt(xmmr/m64)
  - `cvtss2si` reg32, xmmr/m32 reg32 <- floatToInt(xmmr/m32)
- The XMM registers can do arithmetic on floating point values one operation at a time (scalar) or multiple operations at a time (packed). The operations have the form: `op` xmmreg_or_memory, xmmreg
- For floating point addition, the instructions are:
  - `addpd` do 2 double-precision additions in parallel (add packed double)
  - `addsd` do just one double-precision addition, using the low 64-bits of the register (add scalar double)
  - `addps` do 4 single-precision additions in parallel (add packed single)
  - `addss` do just one single-precision addition, using the low 32-bits of the register (add scalar single)
- The XMM registers can also do arithmetic on integers. The instructions have the form:
  - `op` xmmreg_or_memory, xmmreg
- For integer addition, the instructions are:
  - `padd(b/w/d/q)` do 16/8/4/2 byte/word/dword/qword-additions
  - `padds(b/s)` do 16/8 byte/word-additions with signed saturation
  - `paddus(b/w)` do 16/8 byte-word additions with unsigned saturation
- One has to use `movaps` to move from memory to XMM registers.
