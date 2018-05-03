# Lecture 1

## What is a sequential program?

- not concurrent / parallel
- single threaded

**Goal**: Understanding how sequential programs work

## Describe the relationship between high-level programming languages and machine level data representation

### Different abstractions

```
C -> Assembly language (A2) -> Machine language (A1)
|              |                    |
|- Compiler ---|--- Assembler (A3) -|
  (A5 - A10)
```

> You will get to write programs, that read programs and output programs.

Sort of like higher order programs

## Data Representation

### Key concepts

#### Bit

The smallest unit of data, can be 0 or 1

#### Byte

Sequence of 8 bits

#### Word

Size is machine specific

|Architecture|Word Size|
|--|--|
|32bits|32bits|
|64bits|64bits|

We'll use 32bit architecture in this course

#### Nibble

Half of a byte = 4bits

### Decimal

Powers of 10

### Binary

Powers of 2

- decimal(1000011) = 2^6 + 2^1 + 2^0 = 67
- binary(67) = repeated division by 2 = 1000011

### Hexadecimal

Powers of 16

0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F

- decimal(42) = repeated division by 16 = 2A
- decimal(F4A) = 15 * 16^2 + 4 * 16^1 + 10 * 16^0 = 3914

### The need for different representations

Possible 32 bit machine instruction

|1100|1010|1101|0000|0101|1011|0010|0001|
|--|--|--|--|--|--|--|--|
|C|A|D|0|4|B|2|1|

`0xCAD04B21`

Now we write the hex, instead of that long ass binary number.

## Numeric Ranges

Given n bits, what is the range of numbers we can represent? = 2^n

|Binary|Unsigned Integer|Signed Integer|Two's complement|
|--|--|--|--|
|000|0|0|0|
|001|1|1|1|
|010|2|2|2|
|011|3|3|3|
|100|4|-0|-4|
|101|5|-1|-3|
|110|6|-2|-2|
|111|7|-3|-1|

### Signed Integers

- Most significant bit is reserved for the sign

#### Problems

- Two representations of 0, 0 and -0
- Requires separate circuits to do addition and subtraction

### Two's Complement

- Use congruence to represent negative numbers

```
n = number of bits
N = 2^n
```

To represent some number `-b`, we'll use `N - b`

Example: To compute `a - b`

- `a + (-b)`
- `a + (N - b)` 
- `a + ((N - 1) - b) + 1`

`N - 1` will always be a string of all `1`s

Computing `((N - 1) - b)` is trivial as we can just flip the bits of `b`

## ASCII Encoding

`1000011` might be ASCII `c` as well!
`1000011` = 67 = `c`

## Unicode Encoding

Size: 4bytes
