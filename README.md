# F3C - Fine Format For Configuration

This document serves as a quick introduction to _f3c_.

The complete [F3C specification](/f3c-spec.md).

A streaming parser for _f3c_ can be found [here].

## What is _f3c_?

_f3c_ is a format for configuration files.

Properties:

- Stream based
- Escapeless
- Easy to read and write
- Minimalistic syntax

_f3c_ supports:

- Implementation in Bash
- Key-value pairs
- Nested structures
- Comments
- Multiline values
- Multiline literals
- Data serialization
- Templating
- Custom type checking

_f3c_ consists of only four basic representations:

- `:`
- `.`
- `"`
- `\n`

And one composite representation:

- `::`

//TODO: add use cases of the parser to exemplify the exmaples. Using the parser to parse the examples.

## Check map

    - Inline-Inline Binding (iib) (semantic attribution)
      x- With explicit identifier
      x- With implicit identifier
      x- With explicit identifier and empty literal
      x- With implicit identifier and empty literal
    - Inline-Block Binding (ibb)
      x- With explicit identifier with custom terminator definition
      x- With explicit identifier with default terminator definition
      x- With implicit identifier with default terminator definition
      - With implicit identifier with custom terminator definition
    - Block identifiers generally
      x- With default terminator definition for block identifier
      x- With custom terminator definition for block identifier
    - Literals following block identifiers
      - Block-Inline Binding
      x- Block-Block Binding with default terminator definition for identifier and literal
      x- Block-Block binding with custom terminator definition for literal
    - Directive syntax (semantic attribution)
      - x Defining types
      - `Operator directive`
        - Template directives
        - Parser controling directives
        - Token configuration directives

## Explanation by examples

A complete showcase of all features by example from simple to complex.

> [!NOTE]
> Terminology: _key_ and _value_ are common in daily talk, and in _f3c_ corresponds to _identifier_ and _literal_, respectively. For simplicity, we will use _key_ and _value_ in this document.

### Key value pair.

```f3c
username: root
password: 123
```

### Array

An zero-based ordered list of values.

```f3c
fruits::
    apple
    banana
    orange
.
```

### Object

An unordered list of key-value pairs.

```f3c
fruits::
    apple: 1
    banana: 2
    orange: 3
.
```

### Raw text

```f3c
some text::
This text is
as it is.
..
```

You can also cut the last newline:

```f3c
some text::
This text is
as it is...
```

//TODO: Bug: It checks for the `..` from start, must check from end for the `..`, or the first `.` will be taken as part of the raw terminator syntax.

### Nested structure

```f3c
plants::
    fruits::
        apple: 1
        banana: 2
        orange: 3
    berries::
        strawberry: 1
        blueberry: 2
        raspberry: 3
    .
    total: 12
.
```

### With explicit identifier and empty literal

Both `apple` and `banana` are empty literals, even if strictly speaking, we know `apple` is not just empty, but an empty string.

```f3c
fruits::
        apple: ""
        banana:
.
```

### With implicit identifier and literal including empty literal

TODO: Is not in parser yet.

This syntax is for sake of syntactical consistency, but the practical way is to just write "apple" and "banana" in this case.

The last case is empty literal.

```f3c
fruits::
    .:apple
    .:banana
    .:
.
```

### Implicit identifier for object

TODO: Does not work yet.

```f3c
basket::
    .::
        type: fruit
        apple: 1
        color: red
        weight: 55
    .
    .::
        type: frut
        apple: 3
        color: green
        weight: 76
    .
    .::
        type: fruit
        apple: 1
        color: red
        weight: 55
    .
.

```

### Multiline identifier

```f3c
:::
abc
123
.
: xyz
```

//example cat 8 | ../src/lkcp2 "$(printf 'abc\n123')" returns `xyz`

Works also with raw text for the identifier:

```f3c
:::
abc
123..
: xyz
```

In the above cases, the identifier spans multiple lines. The value comes after the multiline identifier block. The value is stated with the typical syntax, except that the inline identifier part is exluded.

Thus, a single line literal follows like this:

```f3c
:::
abc
123
.
: xyz
```

And a multiline literal follows an multiline identifier like this:

```
:::
abc
123
.
::
line1
line2
line3
.
```

### Custom terminator definition

A multi line block, either identifier or literal, must end with a terminator.

A terminator can be the default or a custom one.

The default terminator of a block is `.`.

```f3c
key::
    value1
    value2
.
```

A custom terminator is one you define yourself, perhaps if the contained data conflicts with the default.

``

```f3c
punctuation marks:END:
    ,
    .
    ;
    :
END
```

A terminator occurs on a line of its own at the intended end of a block for arrays and objects, as seen in the examples above.
A terminator can also occur after a `.` where the inteded end is for a block of raw text where all text is taken literally.

For exmaple: if we want raw text and do not want a trailing newline:

```f3c
key:MYEND:
    value1
    value2.MYEND
```

or if we prefer the default terminator, simply use `..`.

```f3c
key::
    value1
    value2..
```

The same works for multiline identifiers:

```
::END:
    ,
    .
END
::
    comma
    dot
.
```

//Example: cat 10 | ../src/lkcp2 "$(printf ',\n.')"

And of course you can do:

```
::END:
    ,
    .
END
:END:
    comma
    dot
END
```

Or different terminators:

```
::END1:
    ,
    .
END1
:END2:
    comma
    dot
END2
```

### Templates

```

```
