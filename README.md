# F3C - Fine Format For Configuration

This document serves as a quick introduction to _F3C_.

The complete [F3C specification](/F3C-spec.md).

A streaming parser written in Bash for _F3C_ can be found [bfq](https://github.com/fmxsh/bfq).

## Background

I set out to write a minimalistic configuration format by my own preference. It was a side project planned for 2 days but spanned two months of getting to bottom with my personal ontology of everything, upon which the specification of _F3C_ came to be framed by. More information about the background has been added [at the end](#Elaboration of the background) of this document.

## What is _F3C_?

_F3C_ is a format for configuration files.

Properties:

- Stream based
- Escapeless
- Easy to read and write
- Minimalistic syntax

_F3C_ supports:

- Implementation in Bash
- Key-value pairs
- Nested structures
- Comments
- Multiline values
- Multiline literals
- Data serialization
- Templating

_F3C_ consists of only four basic representations:

- `:`
- `.`
- `"`
- `\n`

And one composite representation:

- `::`

Whitespace is NOT part of the syntax and is ignored.

And for comments:

- `--` - Inline comment
- `/-` - Start of block comment
- `-/` - End of block comment

## Explanation by examples

A complete showcase of all features by example from simple to complex.

> [!NOTE]
> Terminology: _key_ and _value_ are common in daily talk, and in _F3C_ corresponds to _identifier_ and _literal_, respectively. For simplicity, we will use _key_ and _value_ in this document.

### Key value pair.

```F3C
username: root
password: 123
```

### Comments

#### Inline comments

Inline comments are on lines of their own and can not be trailing.

```
-- This is a comment

fruit: banana

```

### Block comments

Block comments must start on a line of their own and end on a line of their own.

```
/- a block
   comment -/
```

### Array

An zero-based ordered list of values.

```F3C
fruits::
    apple
    banana
    orange
.
```

### Object

An unordered list of key-value pairs.

```F3C
fruits::
    apple: 1
    banana: 2
    orange: 3
.
```

### Raw text

```F3C
some text::
This text is
as it is.
..
```

You can also cut the last newline:

```F3C
some text::
This text is
as it is...
```

//TODO: Bug: It checks for the `..` from start, must check from end for the `..`, or the first `.` will be taken as part of the raw terminator syntax.

### Nested structure

```F3C
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

```F3C
fruits::
        apple: ""
        banana:
.
```

### With implicit identifier and literal including empty literal

TODO: Is not in parser yet.

This syntax is for sake of syntactical consistency, but the practical way is to just write "apple" and "banana" in this case.

The last case is empty literal.

```F3C
fruits::
    .:apple
    .:banana
    .:
.
```

### Implicit identifier for object

TODO: Does not work yet.

```F3C
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

```F3C
:::
abc
123
.
: xyz
```

//example cat 8 | ../src/lkcp2 "$(printf 'abc\n123')" returns `xyz`

Works also with raw text for the identifier:

```F3C
:::
abc
123..
: xyz
```

In the above cases, the identifier spans multiple lines. The value comes after the multiline identifier block. The value is stated with the typical syntax, except that the inline identifier part is exluded.

Thus, a single line literal follows like this:

```F3C
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

```F3C
key::
    value1
    value2
.
```

A custom terminator is one you define yourself, perhaps if the contained data conflicts with the default.

``

```F3C
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

```F3C
key:MYEND:
    value1
    value2.MYEND
```

or if we prefer the default terminator, simply use `..`.

```F3C
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

A template can be defined like:

```F3C
declare::my template::
    key1: value1
    key2: value2
.

include::my template

```

Nested templates are also possible:

```F3C
declare::my template1::
    key1: value1
    key2: value2
.

declare::my template2::
    include::my template1
    key3: value1
    key4: value2
.

include::my template2
```

The above will map out to:

```F3C
    key1: value1
    key2: value2
    key3: value1
    key4: value2
```

It will NOT map out to:

```F3C
my template2::
    my template1::
        key1: value1
        key2: value2
    .
    key3: value1
    key4: value2
.
```

The template name is only used as reference to for the template for inclusion elsewhere. Inside the template, you can define whatever construct you want.

### File inclusion

A file can also be included:

```F3C
file::/path/to/file
```

And nested file inclusion is also possible:

File 1:

```F3C
file::./file2
```

File 2:

```F3C
file::./file3
```

File 3:

```F3C
fruit: apple
```

### Whitespace

Whitespace does not matter between and around syntax.

## Elaboration of the background

What initially seemed like obviously simple task opened for typical but subtle complexities of configuration formats, forcing me to go from quick intuitive pussle solving to outright rigorious analytical thinking. My desire of symetry, consistency, and a sense of perfection, demanded of me nothing less than systematic approach of outlining the concepts of the format, as well as the very conceptual underpinnings of why the choosen systematic approach would be justified. Thus, I defined a configuration format in [F3C specification](/f3c-spec.md), and I defined the way I define a configuration format by outlining [Structure of conceptual models](https://github.com/fmxsh/socm) (_SoCM_). The _F3C_ specification is structured by the postulation of _SoCM_, and the justification for doing such, is found in _SoCM_ itself, which is ontologically self-justifying by definition.

Writing this configuration format has thus been a great experience in the conceptual analysis of building such a format, but it has also been an ontological adventure as I suddenly found myself compelled to discover a single super-fundamental concept that underlies all possible conceptual analysis---be it of configuration format or anything else. As my intellect always sought the answer to what is beneath, within, behind or above a given construct, my awareness were exposed to the fog of increasing abstractions in relentless pursuit of trying to capture the seemingly ineffable, by the structuring force of language. With many attempts and much frustration, I succeeded in avoiding the many traps in the fog of abstraction, such as meta for the same of meta or meta that mystifies conceptual shortcomings, and I found the contours of what came to be a pragmatic and functional super-foundational ontology able to structure perception of anything for sake of analysis. I defined an ontology I further use in many more things.

## Todo

- add use cases of the parser to exemplify the exmaples. Using the parser to parse the examples.
