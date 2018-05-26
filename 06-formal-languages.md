# Formal Languages

Specifications: MIPS Reference Sheet

Recognition: Done by the assembler (is the program valid?)

Low level languages have a simple structure

- Easy to do recognition
- Easyto synthesize

High level languages have complex structure

- Harder to recognize
- Harder to synthesize

**How to handle the increased complexity?**

Use formal theory in string string recognition. This is applicable to any general language

Formal language: mathematically precise way of specifying a language

**Noam Chomsky**: Linguist, philosopher, social and political activist

## Definitions

### Alphabet

It is a finite sequence of symbols

Examples: {0, 1}, {0, 1, 2, ..., 9}, {a, b, c, d}, {`add`, `jr`, `beq`, `$1`}

### Word

A finite sequence of symbols.

Examples: 42 over {0, 1, 2, ..., 9}, `jr $31` over MIPS alphabet

#### Empty Word

A word with no symbols

### Language

A possibly infinite set of words

L = all binary strings of length 8
|L| = 2^8 finite language

#### Empty Language

{} - wo words

### Recognition

A yes/no answer whether a given input program allows a language specification

Example: "Is the input a valud word in the language?"

## How to automatically recognize whether an input is in a language?

Depends on how complex the language is!

```
{a^(2n), n >= 0}   // easy

{all valid MIPS assembly program}  // easy

{all valid C/C++/Java Programs}  // harder
```

We can classify languages based on the type of recognition algorithm needed for that language

**Easy to recognize -> Hard to recognize**

Finite, regular, context free, context sensitive, recursive, undecidable

## Finite Languages

A finite set of words

Specification = list out all the words

Recognition = compare input word to each word in the language

### But can we do this more efficiently?

Example L = {cat, car, cow}

We want to write a recognizer by reading the input word exactly once and without storing previous characters.

### Bubble Diagram

_Check picture taken on May 17, 2018_

Can we generalize this to other finite languages? Yes!

For MIPS keywords:

_Check picture taken on May 17, 2018_

But the problem, is that most languages are not finite

## Regular Languages

These languages are built using finite languages, and their:

- Union
- Concatenation
- Repitition

### Example

```
l1 = {dog, cat}
l2 = {fish, <empty>}

// concatenation
l1.l2 = {dogfish, dog, catfish, cat}

// repitition
L* = {L} U {xy | x belongs to i*, y belongs to i}

L = {a, b}
L* = {<empty>, a, b, aa, ab, bb, ...}
```

### Concatenation

Let's say we have a language described by `R1.R2` where `R1` and `R2` are regular expressions. `R1` matches the first part of the word, and `R2` matches the second part of the word.

### Union/Choice

Contains the words matched by either `R1` or `R2`. This is written using `R1|R2`

### Repitition

0 or more repitions of words matched by R. Represented as `R*`

--

One way to define a language could be by using Regex. They are useful for defining a language, but not very convenient for recognition. :robot:

## Deterministic Finite Automata (DFA)

Another way to specify/recognize regular languages, is to use a DFA (Deterministic Finite Automata) (Basically bubble diagrams)

A DFA has 5 components:

1. `\sigma`: alphabet
2. `Q`: set of states
3. `q0`: Initial state
4. `A`: set of accepting states
5. `\delta`: `f: Q, \sigma -> Q`

_Check pictures taken on May 24, 2018_

Regular languages are often used in scanning phase.

DFA for labels in MIPS assembly:

_Check picture takenon May 24, 2018_

DFA for valid registers in MIPS assembly:

Incorrect:

_Check picture taken on May 24, 2018_

Correct:

_Check picture taken on May 24, 2018_

Input word `w = a1, a2, a3, ...`

Output is `w belongs to L(DFA)`?

```
state <- q_0

for i in 1 to n:
    state <- \delta(state, a_i)

if state in A:
    return true

return false
```

We assume the presence of a hidden/implicit error state.

- If a transition is not defined in a given symbol, you transition to the error state
- Once in the error state, you always stay there

> A language is regular if there exists a DFA specifying it. (Kleene Theorem)

### DFAs with actions (Finite Transducers)

DFA to represent all binary numbers:

_Check picture taken on May 24, 2018_

DFA to represent binary numbers and calculate their decimal value

_Check picture taken on May 24, 2018_

DFAs for decimal numbers and hexadecimal numbers

_Check picture taken on May 24, 2018_

How do we make a union of these two?

_Check picture taken on May 24, 2018_

We can't! This will not be deterministic as we have a choice on which path to take. This is a Nondeterministic Finite Automata `NFA`

We are going to look for any path to an accept state. And we'll keep looking until we find a valid path.

## Nondeterministic Finite Automata `NFA`

An NFA has 5 components:

1. `\sigma`: alphabet
2. `Q`: set of states
3. `q0`: Initial state
4. `A`: set of accepting states
5. `\delta`: `f: Q, \sigma -> {Q}` 
