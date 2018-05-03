# Machine Language

It is a set of machine instructions.

## Machine Instruction

It's a sequence of bits that have exactly one meaning on a particular architecture

## MIPS: Microprocessor without Interlocked Pipeline Stages

We'll be working with MIPS 32, which has 32 bit instructions

### Central Unit

- Fetch, execute instructions
- Timing unit
- Communicating with peripherals

### ALU: Arithmetic Logic Unit

- Operations and comparisons

### Memory

- Registors: fast (because it's on the chip)
- RAM: slow (because it's far from the chip)
- 4 more registers: `PC`, `IR`, `hi`, `lo`

#### Registers

We have 32 general purpose registers, each having a size of 32 bit registers

We need 5 bits to mention any register address: 2^5 = 32

`$0` always holds the value 0
`$30` and `$31` are reserved for storing addresses to RAM

#### RAM

It's slower, but a lot more memory. We can visualize this as a byte array. Often, we will use addresses that are multiples of 4

|Address||
|--|--|
|`0x00`|32 bit data|
|`0x04`||
|`0x08`||
|`0x0c`||

Operations can only be performed in values in registers, which means:

- We might need to load data from RAM, into the registers
- And then to store it back, once the operation is done

### But how does a processor execute a program?

A program has a code section, and a data section. How does the processor know which part of memory is code?

Short answer: It does not!

We have a special register - `PC` - Program Counter (32 bits)

This contains the address of the next instruction to execute

A program called the loader laods the program in the memory (RAM), and sets `PC` to the start of where the code is.

Convention: Program is loaded at start address 0, then PC = 0

### Fetch/Execute Cycle

This is implemented in the hardware

1. PC = 0
2. loop
```
{
  IR <- MEM[PC] // fetches 32 bit instruction into Instruction Register
  PC = PC + 4
  // decode/execute
}
```

#### So how is it going to stop?

It won't! It's an infinite loop. It's only gonna stop when our program tells it to stop.

The loader places the "return address" in `$31`. To stop, a program must jump to this return address.


