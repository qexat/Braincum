<!-- markdownlint-disable MD024 MD033 -->

# Braincum Language Specifications

[[Go to bottom](#bottom-notes) | [Home](README.md)]

---

## Summary

- [Braincum Language Specifications](#braincum-language-specifications)
  - [Summary](#summary)
  - [Notes](#notes)
  - [Specifiers](#specifiers)
    - [Subject specifiers](#subject-specifiers)
    - [Context specifiers](#context-specifiers)
  - [Operations](#operations)
    - [Memory operations](#memory-operations)
      - [With subject required](#with-subject-required)
      - [Without subject required](#without-subject-required)
    - [I/O operations](#io-operations)
      - [With subject required](#with-subject-required-1)
      - [Without subject required](#without-subject-required-1)
  - [Functions](#functions)
    - [Utilitaries](#utilitaries)
      - [With subject required](#with-subject-required-2)
  - [Extras](#extras)
  - [Algorithms](#algorithms)
    - [Char algorithm](#char-algorithm)
  - [Bottom Notes](#bottom-notes)
    - [Bottom Note 1](#bottom-note-1)
    - [Bottom Note 2](#bottom-note-2)
    - [Bottom Note 3](#bottom-note-3)

---

## Notes

- Cell array is theorically infinite - this means that every time the pointer reaches the end, the cell array should be extended.

## Specifiers

<sub>[[Summary](#summary)]</sub>

A specifier indicates the execution context of the following operations until another specifier is encountered.

### Subject specifiers

- `&` (ref): the following operations will be applied to the address.
- `$` (value): the following operations will be applied to the value.

### Context specifiers

- `[`...`]` (loop): the enclosed operations will be repeated until the value of the entering cell drops to 0.

- `{`...`}` (sliced array scope): the enclosed operations will only be applied in a subarray whose size is given by the entering cell value.

## Operations

<sub>[[Summary](#summary)]</sub>

### Memory operations

#### With subject required

- `+`: increments value/address by 1.
- `-`: decrements value/address by 1.
- `^`: resets value/address to 0.
- `~`: opposes value/address to bound<sup>[[1](#bottom-note-1)]</sup>.
- `@`:
  - value: sets to address modulo 255.
  - address: sets to value.
- `'`: applies the [Char algorithm](#char-algorithm).
- `"`: stringifies the value/address and changes to its ordinal<sup>[[2](#bottom-note-2)]</sup>.

#### Without subject required

- `>`: shifts all the cells to the right<sup>[[3](#bottom-note-3)]</sup>.
- `<`: shifts all the cells to the left<sup>[[3](#bottom-note-3)]</sup>.

### I/O operations

#### With subject required

- `.`: prints the current value/address as an ASCII character.
- `,`: asks for an integer input between 0 and 255.
- `!`: throws an error. The error code is determined by the value/address.
- `#`: prints value/address as an integer.

#### Without subject required

- `:`: prints the `n`-th next cells as a string with `n` as the value of the current cell.
- `;`: prints the last input if not empty, else asksfor another one and prints it.
- `?`: asks for an ASCII string and stores it in following cells starting at current one.

## Functions

<sub>[[Summary](#summary)]</sub>

### Utilitaries

#### With subject required

- `r`: adds a random integer between 0 and 255 to the value/address.
- `s`: sums all the values before the current cell (included) and puts the result as new value/address.
- `m`:
  - value: multiplies by address.
  - address: multiplies by value.

## Extras

<sub>[[Summary](#summary)]</sub>

- `(` Opens a comment.
- `)` Closes a comment.

---

## Algorithms

<sub>[[Summary](#summary)]</sub>

### Char algorithm

Ensures that the value is in extended ASCII letter boundaries.

- If the value is below 32: adds 32.
- If the value is between 32 and 255: does nothing.
- If the value is above 256: The value clamps down to `value % 255`

A retranscription in Python of this algorithm would be as following:

```py
if value < 32:
    value += 32
elif value >= 256:
    value %= 255
```

---

## Bottom Notes

<sub>[[Summary](#summary)]</sub>

### Bottom Note 1

Example: 3 would be opposed to 252 (0->3 becomes 255->3).
[[Go back up](#with-subject-required)]

### Bottom Note 2

Example: 3 would be stringified to "3", which has an ordinal of 51. If the value is more than 10, the leftmost digit ordinal will be stored in the current cell, and the next digits in the following cells, in a similar behavior as `:`.
[[Go back up](#with-subject-required)]

### Bottom Note 3

- In the context of the global scope, it essentially loses the first cell value or adds a 0 to the left according to the shift direction, as the array is infinite.

- In the context of a array scope, the bounds are joined on the shift as it was a loop. For example, applying `>` to `[3, 5, 2, 1]` would give `[1, 3, 5, 2]`.

  [[Go back up](#without-subject-required)]

[[Go to top](#braincum-language-specifications) | [Home](README.md)]
