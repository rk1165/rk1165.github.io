+++
title = 'Programming from the ground up'
date = 2024-02-11

+++

### 3. Lifting the Hood

- The CPU chip's most important job is to communicate with the computer's memory system.
- the CPU passes an address to the memory system, and the memory system either accepts data from the CPU for storage at that address or places
  the data found at that address on the computer’s data bus for the CPU to
  process.
- Peripherals devices have both address pins and data pins.
- Some peripherals, graphics boards in particular, have their own memory chips, and now their own dedicated CPUs.
- Special signals go out on the bus with the address to indicate whether the address represents a location in memory or one of the peripherals attached to the data bus.
- The address of a peripheral is called an _I/O address_ to differentiate between it and a _memory address_.
- Every CPU contains registers.
- The CPU's immediate work-in-progress is held in temporary storage containers called registers.
- Registers have individiual names such as EAX and EDI.
- There is a set of codes that mean something to the CPU. These codes are called _machine instructions_
- Inside the CPU is a special register called the _instruction pointer_ that quite literally contains the address of the next instruction to be fetched from memory and executed.
- All of the electrical mechanisms by which the CPU does what its instructions tell it to do are called the CPU's _microarchitecture_
- IBM had taken the program code that handled the keyboard, the display and the disk drives and burned it into a special kind of memory chip called ROM.
- The software on the ROM was called the BIOS.
- Sofware like the BIOS, which existed on ROM chips, was nicknamed _firmware_
- The Linux kernel was entirely separate from the user interface.
- System memory was tagged as either _kernel space_ or _user space_, and nothing running in user space could write to (nor generally read from) anything stored in kernel space.
- Communication b/w kernel space and user space was handled through strictly controlled system calls.
- Direct access to physical hardware, including memory, video, and peripherals, was limited to software running in kernel space. Programs wishing to make use of system peripherals could only get access through kernel-mode device drivers.

### 4. Location, Location, Location

- There are a fair number of different ways address memory in the x86 CPU family. Each of these ways is called a _memory model_.
- The oldest and now ancient memory model is called the _real mode flat model_
- The elderly and now retired memory model is called the _real mode segmented model_.
- The newest memory model is called _protected mode flat model_
- The addresses in a megabyte memory run from 00000H to 0FFFFFH.
- A megabyte memory bank has 20 address lines.
- In a real mode segmented model, an x86 CPU can "see" a full megabyte of memory. That is, the CPU chips set themselves up so that they can use 20 of their 32 address pins and can pass a 20-bit address to the memory system.
- The CPU's view of memory in real mode segmented modek is peculiar. It is constrained to look at memory in chunks, were no chunk is larger than 65,536 bytes in length.
- In the context of real mode segmented model, a _segment_ is a region of memory that begins on a paragraph boundary and extends for some number of bytes. In real mode segmented model, this number is <= 64K.
- A _paragraph_ is a measure of memory equal to 16 bytes.
- Any memory address evenly divisible by 16 is called a _paragraph boundary_
- the number of the paragraph boundary at which a segment begins is called the _segment address_ of that particular segment. Each segment address is 16 bytes farther along in memory than the segment address before it.
- Every byte in memory is assumed to reside in a segment. A byte's complete address, then, consists of the address of its segment, along with the distance of the byte from the start of that segment.
- The address of the segment is the byte's segment address. The byte's distance from the start of the segment is the byte's _offset address_
- The 8088, 8086, and 80286 have exactly four segment registers specifically designated as holders of segment address. The 386 and later CPU have two more.
- Each segment register is a 16-bit memory location existing within the CPU chip itself.
- The segment registers have names that reflect their general functions: CS, DS, SS, ES, FS, and GS.
- FS and GS exist only in the 286 and later x86 CPUs.
- _CS stands for code segment_ Machine instructions exist at some offset into a code segment. The segment address of the code segment of the currently executing instruction is contained in CS
- _DS stands for data segment_ Variables and other data exist at some offset into a data segment.
- _SS stands for stack segment_ The _stack_ is used for temporary storage of data and addresses.
- _ES stands for extra segment_ a spare segment that may be used for specifying a location in memory
- _FS and GS are clones of ES_
- In our current 32-bit world, the general-purpose registers fall into three general classes:
- the 16-bit general-purpose registers.
- the 32-bit extended gpr
- the 8-bit register halves.
- the 16 bit and 8-bit registers are actually names of regions _inside_ the 32-bit registers.
- Ther are 8 16-bit general-purpose registers: AX, BX, CX, DX, BP, SI, DI and SP.
- When Intel expanded x86 arch to 32 bits it doubled the size of all eight registers and gave them new names by prefixing an E in front of each register name resulting in EAX, EBX, ECX, EDX, EBP, ESI, EDI, and ESP.
- The lower 16 bits of ESI, for example, may be referenced as SI.
- The low 16 bits of EAX are themselves divided into two 8-bit halves.The 16-bit register AX is present as lower 16-bit portion of EAX; but AX is itself divided into 8-bit halves. The high half is AH and the low half is AL. This dual nature involves only AX, BX, CX and DX
- The _instruction pointer_ contains the offset address of the next machine instruction to be executed in the current code segment.
- A _code segment_ is an area of memory where machine instructions arestored.
- IP or EIP in 32-bit.
- _flags_ register is 16 bits in size and its formal name is FLAGS. It is 32 bits in size in the 386 and later CPUs and its formal name is EFLAGS.
- Most of the bits in the flags register are used as single-bit registers called _flags_ Each of these individiual flags has a name, such as CF, DF, OF, and so on, and each has a very specific meaning within the CPU.
- The x86-64 architecture defines three general modes: real mode, protected mode, and long mode.
- The 64 bit versions of the registers are renamed beginning with R: EAX becomes RAX, EBX becomes RBX

### 5. The Right to Assemble

- A computer architecture that stores the least significant byte of a multibyte value at the lowest offset is called _little endian_
- Assemblers read source code files and generate an object code file containing the machine instructions that the CPU understands, plus any data you've defined in the source code.
- Modern assemblers generate a binary file called an _object module_ or simply an object code file
- Object code files cannot themselves be run as programs. An additional step, called _linking_, is necessary to turn object code files into executable program files.
- `nasm -f elf -g -F stabs eatsyscall.asm`
- `-f elf` specifies that the .o file will be generated in the "elf" format
- `-g` specifies that debug information is to be included in the .o file
- `-F stabs` specifies that debug information is to be generated in the "stabs" format
- `make -B` to forcefully compile.
- to link the 32bit object file using ld we use the following command `ld -m elf_i386 -o eatsyscall eatsyscall.o`

### 7. Following Your Instructions

- We need a starting point that is marked as global, the label `_start`.
- We also need to define a data section and a text section.
- The data section holds named data items that are to be given initial values when the program runs.
- The text section holds program code.
- The `.bss` section holds uninitialized data - that is, space for data items that are given no initial values when the program begins running.
- These are empty buffers, basically, for data that will be generated or read from somewhere while the program is running.
- The `MOV` instruction can move a byte, word, or double word of data from one register to another, from a register into memory, or from memory into a register.
- `MOV` can't move data directly from one address in memory to a different address in memory.
- `mov eax,1` the leftmost operand belonging to machine instruction is the _destination operand_. The second operand from the left is the _source operand_.
- Data stored inside a CPU register is known as _register data_, and accessing register data directly is an addressing mode called _register addressing_.
- Register addressing is done by simply naming the register you want to work with.
- The `ADD` instruction adds the source and destination operands and the sum replaces whatever was in the destination operand.
- _memory data_ is stored somewhere in the sliver of system memory "owned" by a program, at a 32-bit memory address.
- Only _one_ of an instruction's two operands may specify a memory location.
- To specify that you want the data at the memory location contained in a
  register, rather than the data in the register itself, you use square brackets around the name of the register.
- `mov eax,[ebx]` : to move the word in memory at address contained in EBX into register EAX.
- In assembly language variable names represent addresses, _not_ data!
- The size issue get tricky when we want to write data in a register out to memory. This is done by a _size specifier_.
- `mov [EatMsg],byte 'G'` other size specifier include WORD and DWORD.
- EFlags as a whole is a single 32-bit register. Each of those 32 bits is a flag. Each of the EFlags register's flags has a two or three letter symbol by which programmers know them.
- The **Overflow flag** generally used as the "carry flag" in signed arithmetic.
- The **Direction flag** dictates the direction that activity moves during the execution of string instructions.
- The **Interrup enable flag** is a two-way flag. Can set it using the `STI` and `CLI` instructions. When IF is set, interrupts are enabled and may occur when requested. When cleared, interrupts are ignored by the CPU.
- The **Sign flag** becomes set when the result of an operation forces the operand to become negative.
- The **Zero flag** becomes set when the results of an operation become zero. Used a lot for conditional jumps.
- `INC` and `DEC` increment and decrement an operand by one, respectively.
- The `INC` doesn't affect the Carry flag, so don't try to use it to perform multidigt arithmetic.
- `DEC` affects the OF, SF, ZF, AF, and PF flags. The DEC EBX instruction cleared all of them.
- The `JNZ` instruction tests the value of the zero flag. If ZF is set, then nothing happens, else the execution travels to a new destination in the program.
- The desination is provided as a _lable_
- Whether a given binary pattern represents a signed or an unsigned value depends on how you choose to use it.
- `JMP` instruction does _not_ look at the flags. When executed, it _always_ jumps to its operand, hence the mnemonic.
- The `NEG` instruction will take a positive value as its operand, and negate that value - that is, make it negative. It does so by generating the two's complement form of the positive value.
- If we move -42 - a value in 16-bit register like AX into a 32-bit register like EBX _the sign bit will no longer be the sign bit_. -42 changes to 65494 because the sign bit is now just another bit in a binary value.
- A way out of this trap is the instruction `MOVSX` which means "Move with Sign Extension"
- `MOVSX` instruction performs _sign extension_ on its operands.
- "r/m" means register or memory.
- Some instructions act on registers or even memory locations that are not stated in a list of operands. These instructions do in fact have operands, but they represent assumptions made by the instruction. Such operands are called _implict operands_ and they do not change and can't be changed.
- `MUL` and `DIV` handle unsigned calculations
- `IMUL` and `IDIV` handle signed calculations
- immediate values cannot be used as operands for `MUL`
- `MUL` takes only one operand, which contains one of the factors to be multiplied. The implicit operands depend on the size of the explicit one.
- mul r/m8 AL AX
- mul r/m16 AX DX and AX
- mul r/m32 EAX EDX and EAX
- Immediate values cannot be used as operands for `MUL`
- `MUL` very helpfully sets the CF when the value of the product overflows the low-order register. If, after a `MUL`, you find CF set to 0, you can ignore the high-order register.
- You place a dividend value in EDX and EAX, which means that it may be upto 64 bits in size. The divisor is stored in `DIV`'s only explicit operand, which may be a register or in memory. The quotient is returned in EAX, and the remainder in EDX
- `DIV`'s implicit operands act as the divisor.
- DIV r/m8 AL AH
- DIV r/m16 AX DX
- DIV r/m32 EAX EDX
- The `DIV` instruction does not affect any of the flags.
- `DIV` and `MUL` instructions are close to the slowest instructions in the entire x86 instruction set. The 32-bit version is slower than the 16-bit version, and the 8-bit version is the fastest of all.
- A _displacement_ is the distance between the current location in the code and another place in the code to which you want to jump. It's signed, because a positive displacement jumps higher (forward) in memory, whereas a negative displacement jumps you lower (back) in memory.
- A _machine cycle_ is one pulse of the master clock that makes the PC perform its magic.

### 8. Our Object All Sublime

- Ordinary user-space programs written in NASM for Linux are divided into three sections : .data, .bss, .text
- The .data section contains data defintions of initialized data items.
- Data buffers - where the data is to go after reading data from a disk file - are defined in the .bss section.
- The actual machine instructions that make up your program go into the .text section.
- The .text section contains symbols called _labels_ that identify locations in the program code for jumps and calls.
- A variable is defined by associating an identifier with a _data definition directive_
- Data definition directives look like this:

```
MyByte   db  07h             ; 8 bits in size
MyWord   dw  0FFFFh          ; 16 bits in size
MyDouble dd  0B8000000h      ; 32 bits in size
```

- Strings are defined simply by associating a label with the place where the string _starts_.
- numeric literal `10` is EOL character. It indicates to the operating system where a line submitted for display to the Linux console ends.
- A statement containing the directive `EQU` is called an _equate_. An equate is way of associating a value with a label.
- to get length of the string we use '$'. The '$' token marks the spot where NASM is in the intermediate file (.o)
- In x86 architecture, the top of the stack is marked by a register called the _stack pointer_, with the formal name ESP. It's a 32-bit register, and it holds the memory address of the last item pushed onto the stack.
- As items are pushed onto the stack, the stack grows downward, toward low memory.
- You can place data onto the stack in several ways, but the most straightforward way involves a group of five related machine instructions: `PUSH`, `PUSHF`, `PUSHFD`, `PUSHA`, `PUSHAD`. All work similarly, and differ mostly in what they push onto the stack.
- `PUSH` pushes a 16-bit or 32-bit register or memory value that is specified in source-code.
- `PUSHF` pushes the 16-bit Flags register onto the stack.
- `PUSHFD` pushes the full 32-bit EFlags register onto the stack.
- `PUSHA` pushes all eight of the 16-bit general-purpose registers onto the stack.
- `PUSHAD` pushes all eight of the 32-bit general-purpose registers onto the stack.
- None of the x86 CPUs can push 8-bit registers onto the stack.
- User-mode Linux programs cannot push the segment registers onto the stack.
- Getting an item of data _off_ the stack is done with another quintet of instructions: `POP`, `POPF`, `POPFD`, `POPA`, `POPAD`
- To copy Flags or EFlags into a register, first push Flags or EFlags onto the stack.
- Stack's most important use is probably calling procedures and Linux kernel services.
- The way to call service routines inside Linux is referred as _kernel services call gate_.
- At the very start of x86 memory, down at segment 0, offset 0, is a special lookup table with 256 entries. Each entry is a complete memory address including segment and offset portions, for a total of 4 bytes per entry.
- Each of these addresses in the table is called an _interrupt vector_.
- The `INT` (INTerrupt) instruction is used to request the services of Linux in displaying its ad slogan string on the screen.
- The INT instruction pushes a return address onto the stack and then jumps to the address stored in a particular vector.
- Interrupt number 80h has pointed the way into darkest Linux to the _services dispatcher_ sort of switch with spurs heading to the many individual Linux kernel service routines.
- How the services dispatcher knows which one to execute? You have to tell the dispatcher which service you need, which you do by placing the service's number in register EAX.
- The instruction `IRET` pops the return address off the stack and then jumps to the address.
- The address of the beginning of the buffer is placed in EBP. The number of characters in the buffer is placed in the ECX register.
- `sys_read` return the number of bytes read

### 9. Bits, Flags, Branches, and Tables

- `AND` instruction looks at two bits and yields a third bits based on the values of the first two bits.
- A major use of the `AND` instruction is to isolate one or more bits outs of a byte value or a word value.
- Segment registers cannot be used with any of the bitwise logic instructions.
- `SHL` shifts its operand Left, whereas `SHR` shifts its operand Right.
- `shl <register/memory>,<count>`
- `RCL`, `RCR`, `ROL`, `ROR` bit bumped off one end of the operand reappears at the opposite end of the operand.
- `RCR` Rotate Carry Right and `RCL` Rotate Carry Left : Work as ROL and ROR but with a twist: The bits that are shifted out the end of an operand and reenter the operand at the beginning travel by way of the Carry flag.
- `CLC` clear the CF to 0. `STC` sets the CF to 1.
- An _unconditional_ jump is a jump that _always_ happens. `jmp <label>`
- Almost every conditional jump instruction has an alter ego: a jump when the specified condition is _not_ set to 1
- `JNZ` jumps to a label if ZF is 0, and falls through if ZF is 1.
- _Signed values_ are thought of as being _greater than_ or _less than_. For example, to test whether one signed operand is greater than another, you would use the `JG` (Jump if greater) mnemonic after a `CMP` instruction.
- _Unsigned values_ are thought of as being _above_ or _below_. For example, to tell whether one unsigned operand is greater than (above) another, use the `JA` (Jump if Above) mnemonic after a `CMP` instruction.
  ![Arithmetic tests useful after a CMP instruction](/images/tests.png)
- `TEST` performs an `AND` logical operation between two operands, and then sets the flags as the `AND` instruction would, _without_ altering the destination operation, as `AND` also would. `TEST` instruction syntax : `test <operand>, <bit mask>`
- `test ax,08h ; determine whether bit 3 of AX is set to 1`
- `test ax,00001000B` another way
- `TEST` is only useful for finding 1 bits. It checks for presence of a single 1 bit.
- `BT` (Bit Test) performs a very simple task: it copies the specified bit from the first operand into the CF. The syntax for BT is : `bt <value containing bit>, <bit number>` After BT immediately test the value in the CF and branch based on its value.
- `bt eax, 4 ; test bit 4 of AX`

##### Protected Mode Memory Addressing in Detail.

![Protected mode memory addressing](/images/pmma.png)

- The base and index registers may be any of the 32-bit GP registers including ESP
- The displacement may be any 32-bit constant.
- The scale must be one of the values 1, 2, 4, or 8.
- The index register is multiplied by the scale before the addtions are done.
- All of the elements are optional and may be used in almost any combination.
- 16-bit and 8-bit registers may _not_ be used in memory addressing.
- When the displacement term stands alone, it is virually always a symbolic address.
- `LEA` Load Effective address: It calculates an effective address given between the brackets of its source operand, and loads that address into any 32-bit GP register given as its destination operand.
- A translation table is a special type of table, and it works the following way: you set up a table of values, with one entry for every possible value that must be translated
- A number is used as an index into the table.
- The `XLAT` instruction is hard-coded to use certain registers in certain ways. Its two operands are implicit:
- The address of the translation table must be in EBX
- The character to be translated must be in AL
