# Procedures

These are like functions. But we have a few problems here.

1. The same registers are accessible to all code. We might need a way to preserve/ protect register values.
2. How to call and then return from a procedure?

## Problem 1

### Solution 1

We can divide registers between procedures.

But then we might run out of registers. This also wouldn't work with recursion. :sob:

### Solution 2

Each callee (called procedures) should guarantee that it leaves the register unchanged.

This can be done by backing up whatever registers it uses, and then restores data abck to that register. But where can we backup? RAM!

This is a potential solution if we can figure out a systematic way of storing things in RAM without over writing previous data.

MIPS loader helps with this. It sets up `$30` to just past the last available memory spot. When a procedure is done with that memory, that is, it has restored the values of the registers it used, it sets frees up that memory by resetting `$30`.

Here, memory is being used as a stack. The callee pushes register values at the start and then pops register values just before returning.

Assembly code for doing a push and a pop: 

```assembly
; push

sw $8, -4($30)
lis $8
.word -4
add $30, $30, $8

; pop

lis $5
.word 4
add $30, $30, $5
lw $5, -4($30)
```

## Problem 2

Procedures are created by using a label and thenwriting the procedure body.

### Failed attempt

_Picture taken on May 10, 2018_

### Fix: `jalr` Jump to Link Register

`jalr $s`

`$31` <- `PC`, `PC` <- `$s`

## Parameter Passing:

The convention is that if you have a few parameters, then you should use registers. Otherwise use RAM.

But how do you pass in parameters? **YOU DONT!** The procedure just assumes that some register is gonna have that parameter. So we need to document it. There is no other way to do it. :sob:

```assembly
; Sum1toN
; $1 scratch (should be preserved)
; $2 input number, N (should be preserved)
; $3 output (don't preserve)

Sum1toN:
sw $1, -4($30)
sw $2, -8($30)

lis $1
.word 8
sub $30, $30, #1

add $3, $0, $0
lis $1
.word -1

loop:
add $3, $3, $2
add $2, $2, $1
bne $7, $0, loop

lis $1
.word 8
add $30, $30, $1
lw $1, -4($30)
lw $2, -8(30)
jr $31
```
