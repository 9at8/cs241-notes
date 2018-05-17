# Assembler

_Check picture from May 10_

## Stages

### Analysis

- Making sense of what the input string is
- Understanding the meaning or intent

This stage can have multiple steps in it.

Some key concepts:

#### Tokenize/ Scanning/ Lexical Analysis

Convert input string into a sequence of _tokens_

The given string:

```
add $1, $2, $3
```

We want to make it something like this:

|||||
|--|--|--|--|
|add|$1|$2|$3|

We **don't** have to write our own tokenizer for A3

#### Parse

Parsing is to make sense of what each line means

- Simple to do for MIPS assembly
- Do it in an adhoc way
- Don't check for errors first, check for correct input, and then throw an error for everything else

### Synthesis

- equivalent machine instructions in binary

1. Produce within the assembler, the 32 bits representing the instruction
2. Output these bits:

- But how to deal with labels?

```assembly
begin: add $1, $2, #3

; stuff
; stuff
; stuff

beq $0, $0, begin
```

Use of label in `beq`/`bne` has to be converted to an instruction offset

(Address of where the label was defined - PC) / 4

Our assembler will need to keep track of a virtual PC

#### Algorithm for handling labels

1. Initialize PC = 0
2. Increment PC by 4 for each instruction
3. When a label is defined:
   - Store the "lexeme" of PC in a map of string and int
4. When a label is used:
   - if word is inside a `beq`/`bne`, use the formula
   - `.word <label>` just output the address

But what happens when we encounter a label that is going to be defined later?

Note: There can also be multiple labels in a single line.

```assembler
beq $1, $0, end
; stuff
; stuff
end: jr $31
```

Solution: Write a multi pass assembler

1. Pass 1
   - Tokenize
   - Parse sytax checks where possible
   - Create Symbol table for labels
2. Pass 2
   - Check uses of labels
   - generate output
	 - Produce the 32-bitrepresentation internally
	 - Output the bits

## Manipulating Bits

### Left bit shift

```
x << n
0001 << 0 = 0001
0001 << 1 = 0010
0001 << 2 = 0100
```

### Right bit shift

```
x >> n
0100 >> 0 = 0100
0100 >> 1 = 0010
0100 >> 2 = 0001
0100 >> 3 = 0000

// but for negative ints (2s complement)

1000 >> 1 = 1100
1000 >> 2 = 1110
```

## Encoding an `add` instruction

```assembly
add $3, $2, $4
```

```c++
int opcode = 0
int funct = 32
int s = 2
int t = 4
int d = 3
```

Here we use bit manipulation, and then use bitwise or to get the binary to build our instruction

```c++
opcode << 26
s << 21
t << 16
d << 11
funct

int instruction = opcode << 26 | s << 21 | t << 16 | d << 11 | funct

cout << instruction; // this won't work :(
```

What happens when we do this?

```c++
int x = 65;
char c = 65;

cout << x << c; // prints out 65A
```

The conversion doesn't take place for `char`s

We are going to use this to our advantage

```c++
opcode << 26
s << 21
t << 16
d << 11
funct

int instruction = opcode << 26 | s << 21 | t << 16 | d << 11 | funct

// here we go

char c = instr >> 24;
cout << c;

c = instr >> 16;
cout << c;

c = instr >> 8;
cout << c;

c = instr;
cout << c;
```

In racket, ther's a function called `writebyte` which is used to, well, write bytes

## Encoding a `beq` instruction

`beq $1, $2, -3`

||||||
|--|--|--|--|--|
|`000100`|`sssss`|`ttttt`|`iiiiiiii`|`iiiiiiii`|

The last `iiiiiiii...` will be encoded at `...1111101` which start with `1`s. So when we `|` it, our instruction would be made up of `1`s

So we need to make sure that all the bits except the needed bits are 0, so we can `&` those with `0xffff`
