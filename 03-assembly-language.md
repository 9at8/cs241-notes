# Assembly Language

It is a textual representation of binary language.

Example: Compute absolute value of `$1` into `$3`

## `slt` set less than

`slt $d, $s, $t`

### What does it do?

Sets `$d` to 1 if `$s < $t`, otherwise `$d = 0`

## `beq` branch equal

`beq $s, $t, i`

`i` can be negative or positive, and it's a 2s complement number

### What does it do?

`$s == $t` && `PC = PC + (i * 4)`

Similar command: `bne` branch not equal

### Examples

```assembly
slt $2, $1, $0
beq $2, $0, 1
sub $1, $0, $1
jr #31
```

```assembly
slt $2, $1, $0
bne $2, $0, 2
jr $31
sub $1, $0, $1
jr $31
```

Q: Calculate 13 + 12 + 11 + ... + 1 and store the result in `$3`

```assembly
lis $2
.word 13
add $3, $0, $0
add $3, $3, $2
lis $1
.word -1
add $2, $2, $1
bne $2, $0, -5
jr $31
```

A little bit optimized version:

```assembly
lis $2
.word 13
lis $1
.word -1
add $3, $0, $0
add $3, $3, $2
add $2, $2, $1
bne $2, $0, -3
jr $31
```

We can move these conditions because we don't need to initialize `$1` with -1 again and again

## Assembly labels (directives)

When you declare a label, it is associated with the instruction at that label. When the label is used in `beq`/`bne`, the assembler computes the offset = `label-offset - PC`

```assembly
lis $2
.word 13
lis $1
.word -1
add $3, $0, $0
loop: add $3, $3, $2
add $2, $2, $1
bne $2, $0, loop
jr $31
```

## `lw`: Load Word

`lw $t, i($s)`

$t <- MEM[$s +i]

## `sw`: store word

`sw $t, i($s)`

## Special Address

### `0xffff000c`

Storing a value to this address will cause the least significant byte to `stdout`

```assembly
lis $1
.word 0xffff000c
lis $2
.word 67
sw $2, 0($1)   ; outputs C to stdout
```

### `0xffff0004`

Loading a value from this address will read the next keystroke from `stdin` - 1byte

```assembly
lis $1
.word 0xffff0004

lw $2, 0($1)
```

## `mult`: Multiply

`mult $s, $t`

But where does the result go?

The result could be larger that 32 bits, therefore, the result is stored in `hi`:`lo`

`hi`: Higher 32 bits

`lo`: Lower 32 bits

**multi** for multiplication

Use `mfhi $d` and `mflo $d` for moving values from `hi` and `lo` to `$d`

## `div`: Divide

`div $s, $t`

`lo` <- `$s` / `$t`

`hi` <- `$s` % `$t`

## Example

`$1` contains the address of an array of 32-bit ints. `$2` contains the number of elements in the array. Read `array[3]` into `$2`

|Address|Data|
|--|--|
|40| _32 bit int_ |
|44||
|48||
|52||

```assembly
lis $5
.word 3
lis $4
.word 4
mult $4, $5
mflo $5
add $5, $1, $5
lw $3, 0($5)
jr $31
```

A better solution using offsets:

```assembly
lw $3, 12($1)
jr $31
```

