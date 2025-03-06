# f3c - Fine Format For Configuration

> [!NOTE]
> This project is under development. Do not expect finished order.

## TODO

TODO: what about array elements existing outside array. They are given implicit keys but because you can not mix array and object entries, it should fail

TODO: blank line in multi line value that is processed, should be ignored, only "" adds empty line to output (applies to ml identifers too)

NOTE: No, empty key "" will make implicit identifier for block literal. for inline literal, no "" is given, no nothing is given, just the value.

NOTE: When including a declared block, it it adds a newline at the end where it is included, regardless if the block is having explicit literal terminator that excludes a newline.

NOTE: Chained includes are possible

TODO: Is it so: The unprocessed literal terminator must be at end of line with no other data after (wherever it is used).,

TODO: Include the _super-enabler_

This document outlines the `f3c`; an easy to handle configuration and serialization format, designed with the goal of minimal syntax and maximal functionality.

This document is a reference of `f3c` in its totality, suitable for implementing _f3c_ parsers.

## Porupose

Define a format that is _so and so_.

## How to approach this document

Despite this document being written primarily for completeness and exhaustive detail as a reference, efforts have also been made to enhance readability. For the sake of clarity, the most fundamental concept is outlined first, from which all other emergent concepts and designed syntax arise. First all concepts will be described by order of generality, with the most concrete things first, followed by increasingly more abstract structures, where each level of abstraction builds on the previous more concrete level. The highest level of abstraction, building on all previous levels, defines yet another layer of structures, but this time a conclusive one that fulfills of the above mentioned purpose of defining a configuration format with particular characteristics.

This document is structured primarily for completeness and secondarily for readability. It is adviced to skim and focus on the parts that are of interest.

Besides consulting other more reader-friendly documentation [TODO: LINK HERE], it is adviced to skip to the sections with examples to get a sense of how the format works.

## Pre-fundamental things

At this level, we have not yet entered the stage where any meaning specific to this configuration format can be seen. Thus, this is a level prior to the most fundamental level of the configuration format. It is the _pre-fundamental_ level. At this level, we are only concerned with the things (characters) and structure (sequence of characters and sequence of lines), which comprises the very existence of _a_ _something_, which is the percieved, within which specific structures can be defined to have higher meaning that can only arise by combinations and relations among these _pre-fundamental things_. The possibility of such emergent higher meaning, based in patterns of lower level things, is what allows the definition of structures specifically meaningful for the purpose of a configuration format. Meaning is thus contingent on patterns that are built from these lower-level constituents. The contingence of meaning is a fundamental principle that will replicate itself in the relationship between each previous and each current level of abstraction, in the sense that the previous is fundamental to the meaning that emerges at the current level, as a combination of fundamental things that the current level recognizes a structure that is meaningful to the overall purpose. Any fundamental level must be well explicated to allow for the next to percieve meaningful structures among occurences of these fundamental things.

**pre-fundamental characters**: Any valid byte sequence recognized by the host system's encoding configuration, including but not limited to UTF-8, UTF-16, UTF-32, ASCII, extended ANSI (ISO-8859 series), and arbitrary binary data (including null bytes \x00). Character interpretation depends on the system's locale (LC_CTYPE), encoding settings, and application behavior.

**pre-fundamental sequence**: A sequence of _pre-fundamental characters_

**pre-fundamental line**: As by POSIX. A sequence of _pre-fundamental characters_, or an empty sequence, that ends with a newline character (`\n`). A line can contain only one newline character. It follows that lines occur in sequence, one line following another.

These terms will later in the document be simplified to _character_, _sequence_, and _line_ while carrying the same meaning.

## Things - Basic Tokens

Out of all characters of the _pre-fundamental characters_, certain ones will be given special meaning. These are called tokens and will be the most fundamental building blocks of the syntax. At this first level of abstraction, characters, that otherwise can represent anything, are given the general meaning by being tokens, where each token has a specific uniquely defined meaning. This is also called the fundamental level, as these tokens create the foudnation of the syntax.

Thus, this section outlines the basic tokens making up the syntax.

The tokens themselves are given names (_introducer_, _finalizer_ etc...) to strengthen the conceptual understanding of the format and the sense of the underlying logic of the syntax rules.

### Basic standalone tokens

All syntax depends on these.

| Token | Name       | Description                                     |
| ----- | ---------- | ----------------------------------------------- |
| `:`   | Introducer | Always precedes something being defined.        |
| `.`   | Finalizer  | Always puts an end to multi-line data.          |
| `"`   | Delimiter  | Always delimits single-line data.               |
| `\n`  | Segmenter  | Separates elements and parts within an element. |

> [!NOTE]
> The `segmenter`, `\n`, refers strictly to syntax and is part of the syntax. It does not refer to the data contained by the syntax, like the data of a multi-line literal.

### Basic composite tokens

Composite tokens are built using the `basic standalone tokens`, such that changing a standalone token would change the related composite token.

| Token | Name                  | Description                                                                                            |
| ----- | --------------------- | ------------------------------------------------------------------------------------------------------ |
| `::`  | Meta level introducer | same as _introducer_ but a higher level of abstraction.                                                |
| `""`  | Emptier               | Used to indicate that no identifier exists for the multi-line literal that follows. (Nameless element) |

> [!IMPORTANT]
> The composite token `::` is not the same as two individual `introducers` (`:`) in a row (`::`), but they may seem identical. For example: An identifier for a multi-line literal can be defined with a `terminator definition` as in `id:termdef:`, but if we desire to use the `default terminator` we leave it empty as in `id::`. In this case, by the very usage itself, we recognize we are dealing with two separate `introducer` tokens and not one _meta level introducer_. Its even more clear by the fact we can space out the former case like `id: :`. The composite token `::`, on the other hand, is a single token, and always appears as `::` without any whitespace possible between the two colons of the composite.

### Tokens for comments

`--`
`/-`
`-/`

Comments are considered **ghost-tokens**, forming a **ghost-syntax**, because they exist to human perception but do not exist in the parsed structure.

A comment token is a **token that is never interpreted as a token** in the sense of relating to data.

To human perception, it appears that the comment syntax relates to the comment-text itself—i.e., the syntax _contains_ the comment. However, the parser does not merely ignore the comment-text; it also ignores the comment syntax.

In all other cases of syntax, the parser assigns meaning to the content, whether it is empty or not. In the case of a comment, no meaning is assigned to any contained data or to the comment syntax itself.

## Data

This is still at the first level of abstraction, alongside with _basic tokens_. Now, the rest of the _pre-fundamental sequence_, which is non-syntax, is recognized, and put in different categories of meaning.

Besides the _basic tokens_, there is _data_. Data is a deliniated sequence of characters or no characters ('emptiness' exist within a deliniated space, and thus, the very absense of characters is considered to be the very data of that space). Data in its most general form can contain any character.

A general rule applying to all _data_ is that no character will ever be interpreted as a token, and consequently, no character is ever escaped.

Data is categorized into formats and types, which are then combined to form data constructs.

### Data formats

A data format is defined by the way the data is deliniated.

Inline deliniation is where the data is deliniated to start and end on the same line.

Block deliniation is where the data is deliniated to start and end on different lines.

All types of data fall into one of two categories of data format: _inline format_ or _block format_.

| Label           | Data type | Description                                 |
| --------------- | --------- | ------------------------------------------- |
| [inline format] | Inline    | Data that exists only inside a single line. |
| [block format]  | Block     | Data that must be multi-line data.          |

The labels _[inline format]_ and _[block format]_ refers to the corresponding structure as made up by the syntax including the specific data being contained by the syntax.

The labels _[inline data]_ and _[block data]_ refers only to the data within the syntax, not the syntax itself.

### Data types

Data can be one of two categories of data types: `literal` and `identifier`:

| Label        | Data type  | Description                                 |
| ------------ | ---------- | ------------------------------------------- |
| [identifier] | Identifier | A name used to identify a specific literal. |
| [literal]    | Literal    | The data associated with an identifier.     |

### Emergent data constructs

The data types, _[identifier]_ and _[literal]_ can be combined with the data formats _[inline format]_ and _[block format]_ to form 4 combinations:

| Label               | Description                                          |
| ------------------- | ---------------------------------------------------- |
| [inline identifier] | An identifier that exists only inside a single line. |
| [block identifier]  | An identifier that must be multi-line data.          |
| [inline literal]    | A literal that exists only inside a single line.     |
| [block literal]     | A literal that must be multi-line data.              |

## The data of each data constructs

Four possible _data constructs_ were defined by combining _data format_ with _data type_. Each of them will be described in detail. 

### Inline literal

Different types of literals exist: string, number, multi-line block etc... The exact type of literal is determined by first evaluating the surrounding syntax (context), and, if shown the type is not  determined to match a certain set of types, then, as second, evaluation is preformed of the content contained by the syntax. It follows that, in evaluation of literal type, context has precedence over content. A string literal will have additional meaning, or not, depending of in what context it appears, and thus, context should be evaluated first.

Possible literals are:

| #   | Label                   | Literal  | Example         | Category  | Quality    | Determined by |
| --- | ----------------------- | -------- | --------------- | --------- | ---------- | ------------- |
| 1   | [string]                | String   | `"abc def"`     | String    | Atomic     | context       |
| 2   | [fragment]              | Fragment | `ghi a123`      | String    | Non-atomic | content       |
| 3   | [number]                | Number   | `32, 3.14, -10` | Primitive | Atomic     | content       |
| 4   | [bool]                  | Bool     | `true, on, yes` | Primitive | Atomic     | content       |
| 5   | [null]                  | Null     | `null, nil`     | Primitive | Atomic     | content       |
| 6   | [terminator definition] | [1-5]    | [1-5]           |           |            | context       |
| 7   | [terminator expression] | [1-5]    | [1-5]           |           |            | context       |

1-5 is _data_, meaning it is _content_. 6 and 7 are part of the syntax as user defined syntax, and thus is _context_.

The term _atomic_ is used, meaning the value is taken as given. A non-atomic value is a value that is not taken as given, but is processed in some way.

Inline literals are always trimmed of whitespace, because whitespace is not part of the data.

All primitive literals are case-insensitive.

The data of all literals, except _[string]_, is considered to be the sequence that is with and from the first non-whitespace character to and with the last non-whitespace character. In case of _[string]_, see the section _String_ below.

#### String

A sequence of characters or no characters deliniated by _[delimiter]_ (`"`) such that `[delimiter] (...) [delimiter]`. The surrounding _[delimiter]_ is not part of the literal's data. `"abc"` is `abc` but `""abc""` is `"abc"`, meaning, only the uttermost surrounding _[delimiter]_ is taken as marks of the data's boundary.

The acceptable character sequence is: Any binary and non-binary character except _[segmenter]_ (`\n`) as it would contradict the essence of _inline_ in _[inline literal]_.

#### Fragment

A sequence of characters or no characters, without surrounding _[delimiter]_, with leading and trailing whitespace is trimmed.

The acceptable character sequence is: Any binary and non-binary character except, but can not start with _[delimiter]_ (`"`), or it will be an _[explicit string]_.

Any unquoted literal that is not a _[number]_, _[bool]_ or _[null]_, is treated as _[fragment]_. In other words, If not a _[string]_ and not a primitive literal, then it is treated as a _[fragment]_. Thus, `&#38;`, for example, is treated as a _[fragment]_.

#### Number

The acceptable character sequence is as inferred from the example in the table above.

#### Bool

The acceptable character sequence are: `true`, `on`, `yes`, `false`, `off`, `no`, `maybe`.

| Data  | Meaning       |
| ----- | ------------- |
| true  | true          |
| on    | true          |
| yes   | true          |
| false | false         |
| off   | false         |
| no    | false         |
| maybe | true or false |

The _maybe_ should return true or false randomly by anything that parses the format. If serialized to another format, it may either be resolved into true or false, or it may be treated as a _[string]_ of the sequence `maybe`.

#### Null

The acceptable character sequence is as inferred from the example in the table above.

#### Terminator definition

The literal _[terminator definition]_ is a type that is used to define the end of a block of data. A terminator definition is always a _literal_ type of _inline_ format. The user defines a sequence of characters that is used as the terminator for the block.

#### Terminator expression

While a _[terminator definition]_ defines the terminator for a block, a _[terminator expression]_ is the actual use of the terminator to end the corresponding block. A _[terminator expressoin]_ is not a literal

### Block literal and block identifier

All things under this section applies to both _[block literal]_ and _[block identifier]_.

Block literals can be of three types: `raw` `array` and `object`.

| Label    | Description                                                                 |
| -------- | --------------------------------------------------------------------------- |
| [raw]    | Raw data exactly as it occurs, nothing is altered.                          |
| [array]  | One or several literals. Surrounding whitespace of each literal is trimmed. |
| [object] | One or several identifier-literal pairs.                                    |

An empty block that is not _[raw]_ is treated as empty array.

**Raw**: The acceptable character sequence is: Any binary and non-binary character.

**Array**: The acceptable character sequence is: Data is separated by _[segmenter]_ (`\n`). Each line is treated as a _[inline literal]_ and leading and trailing whitespace is trimmed

#### Terminating a block

A block has a start and an end in a multi-line space. The start is _[inline | block identifier]_ and the end, at a later line, is _[terminator expression]_.

To terminate a block: _[terminator expression]_ is used, which is the replication of the _[terminator definition]_.

The _[terminator expression]_ can occur in two ways: a) `[terminator expression]` on a line of its own at the end of the block data, and b) `_[literalizer] [terminator expression]` after the intended end of the data, meaning it can occur at the same line as the last data (thus the data will not end with a newline, as it is taken literally as it it occurs within the block span).

In case _a_, the block is treated as _[array]_ or _[object]_ depending on the content. In case _b_, the block is treated as _[raw]_.

A terminator definition is always _inline_. Because a terminator definition is part of the syntax to define the boundary of a block, the terminator definition itself can not be a block. It would conceptually lead to endless recursion if every nested _block terminator definition_ in turn define a _block terminator definition_ to define the end of the parent _block terminator definition_.

#### Definition and expression together to deliniate a block

The _[terminator definition]_ is defined at the start boundary of the block, where the _[terminator expression]_ is used at the end boundary of the block, such that:

```
[...] [terminator definition]
[block data]}
[...] [terminator expression]
```

The use of the two literal types _[terminator definition]_ and _[terminator expression]_ makes it clear they are entierly dependent on context (where and how they are used) to be identified as such literals and not fall back on other forms of literals. The _[inline data]_ of these two literal types is never evaluated like to determine its type.

### Inline identifier

The acceptable character sequence is: Any character except `[delimiter]` (`"`), and `[segmenter]` (`\n`). Also that `[introducer]` (`:`) can not exist at the start or end of a sequence, and not occur twice or more directly following eachother.

TODO: Make the super-enabler, remove the : inside

### Block identifier

Can be _[raw]_ or _[array]_. It can contain data that matches the definition of _[object]_, but each line of the object will then be parsed as a _[fragment]_ of an array.

### Inline terminator definition

Can be a _[string]_ or a _[fragment]_, and the accepted character sequence is the same as for those.

## Binding structures - the highest organizing structures

_Basic tokens_ and _data_ the highest organizing structures are built. They are called _binding structure_.

### Binding codes:

For sake of implementing a parser, these codes are suggested, but must not be used.

| Code | Meaning            |
| ---- | ------------------ |
| i    | [identifier]       |
| c    | [introducer]       |
| l    | [literal]          |
| e    | [empty identifier] |

### The _binding structure_

A binding structure binds an _[identifier]_ to a _[literal]_ into an _[entry]_. All entries must have an identifier to separate it from another entyry. We can not use the data itself as entry, because then the same data can not occur twice. Thus we identify each separate literal by an identifier that is bound to it. An identifier can be both _implicit_ and _explicit_. In the first case, no identifier is specified alongside the data, for example `The Data`, and thus the implicit identity is zero-based indexed in sequential order of occurence. An _explicit_ identifier is one that is specified alongside the data, for example `The key: The data`.

All literals have corresponding identifiers associated, as explained. There is no literal that has no identity.

All structural meaning of the format arises from this basic concept of binding the two fields of _[identifier]_ and _[literal]_ together in that particular order. All syntax center around facilitating this binding. What a binding does is to bind an identity to a given set of data which is the literal, for example this valid syntax: `fruit: banana`. The identifier is `fruit` and the literal is `banana`.

The _binding structure_ is the fundamental meaning of the format. By analyzing the content and recognizing certain patterns of _basic tokens_ and _data_, meaningful structures of binding _[identifier]_ to _[literal]_ are recognized. Any other content that is not recognized is considered to be _nothing_, which means the content is in the absence of fundamentally meaningful structure.

The following structures of binding are recognized:

| Shorthand    | Full Name             | Identifier Type | Literal Type |
| ------------ | --------------------- | --------------- | ------------ |
| [ii-binding] | Inline-Inline Binding | Inline          | Inline       |
| [ib-binding] | Inline-Block Binding  | Inline          | Block        |
| [bb-binding] | Block-Block Binding   | Block           | Block        |
| [bi-binding] | Block-Inline Binding  | Block           | Inline       |

Nesting is a natural function of this strucutre, as a literal can contain any data, including one or several other bindings.

An identifier is always to the left, and a literal is always to the right.

At a conceptual level,

Because, by definition, a literal can be a block of data that can contain one or several other bindings, the fundamental structure allows for recursion and thus nesting of bindings.

The _binding structure_ binds _[identifier]_ to _[literal]_, not purely for organization of data, but to support the function of querying and fetching data within the heirarchy of structures. It is at this level—where the binding structure is recognized—that the format attains functionality. Percieving _binding structure_ allows for querying and fetching.

What follows is how different _binding structures_ are recognized.

#### Nesting

Nesting of entries is a feature possible because of the _binding structure_. One entry can be contained within another, and so on.

It is the [literal] part of the `[identifier] [introducer] [literal]` that holds nested entries. The question is if a block identifiers can contain nested entries? An identifier can a contain nested structures, but those structures will never be analyzed as binding structures, but only taken line by line and will be seen as either _[raw]_ or as _strings_ or _primitives_ of _[array]_. A _[block literal]_ on the other hand, has its content analyzed for the purpose of fetching nested data. In the case _[block identifier]_, on the previous hand, data is never fetched from an identifier, only compared. Thus, nesting serves no purpose in identifiers.

_[identifier]_ and _[literal]_ serves different purposes. The identifier is used merly for identification of literal, while the literal is the data itself that can be of a type that can contain other entries meaningful for a parser that may be looking for one entry within another, but the parser will never look within an identifier for the sake of finding a part of it. The parser always uses the whole identifier in matching against a target given identifier for sake of determining if the current entry is desired for futher consideration.

If an identifier can have nested structures that are meaningful as such, it technically could have another entry with another nested identifier and so on for eternal recursion.

### Note on _[inline terminator definition]_

The _[inline terminator definition]_ is recognized by the same principle as any other _binding structure_. An _[inline terminator definition]_ always follows the format of `[identifier] [introducer] [literal]`. Thus, identifier has its ordinary place to the left, and to the right a literal is found.

### Inline-Inline Binding

Identifier and literal both exist within the same line, and are thus _[inline format]_.

[follow up with syntax, etc...]

### Inline-Block Binding

Identifier exists within the same line, but the literal is _[block format]_ meaning the data of the literal spans multiple subsequent lines.

### Block-Block Binding

Both identifier and literal are _[block format]_, meaning both has data spanning multiple subsequent lines.

### Block-Inline Binding

While the identifier is _[block format]_, the literal is _[inline format]_, meaning the identifier spans multiple subsequent lines, but the literal is contained within a single line following the _[block format]_ of the identifier.

---

From the tokens, a core syntax is created.

Having described _basic tokens_ and _data_, we can now define the _core syntax_.

| Label                | Syntax                         | Description                                                |
| -------------------- | ------------------------------ | ---------------------------------------------------------- |
| [identifier proxy]   | `:`                            | To make block identifier.                                  |
| [custom terminator]  | [inline terminator definition] | Terminator for block literali or identifier.               |
| [literalizer]        | `.`                            | Makes terminator explicit for block literal or identifier. |
| [default terminator] | `.`                            | Default terminator for block literal or identifier.        |
| [empty identifier]   | `""`                           | Empty identifier when used in place of identifier.         |

For multi-line identifier: using `:` _as the identifier_ tells that whatever is defined after is the identifier, in this case, the multi-line span.

Thus, the case `:::` is understood as: `[identifier proxy] [assignment] (terminator definition) [assignment]`. With the optional terminator definition, it would be `::term:`. This is differnt from operator syntax, which is also `::`, but requires text before the `::`. The operator syntax is considered a token in itself (the `::`), whereas the `identifier proxy` syntax consists of two tokens: `identifier proxy` and `assignment`. Thus, locically in the latter case, whitespace can exist between as in ` : :`, because the first `:` is in place of an `identifier` which naturally can be surrounded by whitespace.

## Operator syntax

Operator syntax add functional and structural properties to the format.

| Label           |                  | Description                                                    |
| --------------- | ---------------- | -------------------------------------------------------------- |
| [directive]     | [directive name] | Name of the directive. Predefined.                             |
| [operator]      | `::`             | Separates directive from associated data that what comes after |
| [operator data] | rest of line     | Data associated with the directive.                            |

Use: `[directive] [operator] (...)`

Example: `file::/home/user/config.f4c`

### x Defining types

For type validation etc,

`type::date "regex"`
`date::key: 1999-12-31`

### `Operator directive`

#### Template directives

| Label     | Directive | Operator | Data             | Description               |
| --------- | --------- | -------- | ---------------- | ------------------------- |
| [file]    | `file`    | ::       | file path        | File to include.          |
| [declare] | `declare` | ::       | sl or ml literal | Declare a block.          |
| [include] | `include` | ::       | `name`           | Include a declared block. |

#### Parser controling directives

| Label    | Directive | Data             | Description        |
| -------- | --------- | ---------------- | ------------------ |
| [parser] | `parser`  | [sleep \| awake] | Modifying parsing. |
| [print]  | `print`   | (...)            | Print mode.        |

#### Token configuration directives

The following must be single character.

| Label             | Directive | Default | Description               |
| ----------------- | --------- | ------- | ------------------------- |
| `[tb-delimiter]`  | `tbd`     | `"`     | Basic token _delimiter_.  |
| `[tb-introducer]` | `tbi`     | `:`     | Basic token _introducer_. |
| `[tb-finalizer]`  | `tbf`     | `.`     | Basic token _finalizer_.  |
| `[tb-segmeenter]` | `tbs`     | `\n`    | Basic token _segmenter_.  |

The following can be one or more characters.

| Label        | Directive | Default | Description          |
| ------------ | --------- | ------- | -------------------- |
| `[tc-dash]`  | `tcd`     | `--`    | Comment dash.        |
| `[tc-start]` | `tcs`     | `/-`    | Comment block start. |
| `[tc-end]`   | `tce`     | `-/`    | Comment block end.   |

### Syntax for data types

#### Inline identifier

`[identifier] [introducer] ...`

#### Block identifier

`[identifier] [introducer] (...)`

#### Inline literal

`... [literal]`

#### Block literal

`... [] [introduer] (...)`

## Inline and block

The basic tokens form a syntax of inline and block structures.

### Types of data

_Data_ is a generic term capturing all parts of the format that is not syntax.

A data's type is defined by the syntax it is part of.

Different types of data have different rules for what characters.

Data types can be one of two categories: `inline`, `block` or `unspecified`.

The term _inline_ means the data exists only inside a single line.
The term _unspecified_ means the data can occur inside a single line, or consist of multi-line data.
The term _block_ means the data must be multi-line data. Even if multi-line data consists only of one line (has only one newline character), it is still considered block data.

#### Inline

| Data type             | Rules      | Example      | Description                        |
| --------------------- | ---------- | ------------ | ---------------------------------- |
| `[any inline]`        | [0]        | `abc`        | Generic catch-all inline category. |
| `[terminator inline]` | [2]        | `(...):abc:` | Used to end multi-line data.       |
| `[directive inline]`  | `a-z0-9_-` | `abc::(...)` | Name of a directive.               |
| `[operator inline]`   | [1]        | `(...)::abc` | Data being used for the operation. |

[0] Any possible binary and non-binary character except `[segmenter]` (`\n`).
[1] `[any data]` except non-ending `\n`.
[2] `[any data]` except `[delimiter]` (`"`), `[introducer]` (`:`) and `[segmenter]` (`\n`).

#### unspecified

| Data type                  | Rules     | Example                          | Description                                                                           |
| -------------------------- | --------- | -------------------------------- | ------------------------------------------------------------------------------------- |
| `[any unspecified]`        | [0]       | `abc`                            | Generic catch-all unspecified category.                                               |
| `[comment unspecified]`    | [0]       | `-- abc`                         | Considered _nothing_. Thus, conceptually, there is no such thing as `[comment data]`. |
| `[identifier unspecified]` | See below | `id::`, `:::\n1\n2\n.`           | Used to identify a specific literal.                                                  |
| `[literal unspecified]`    | See below | `(...): abc`, `(...)::\n1\n2\n.` | The data associated with a key.                                                       |

[0] Any possible binary and non-binary character.

Identifier unspecified:
In case `identifier unspecified` occures inside a single line, it can contain any character except `[segmenter]` (`\n`) and `[delimiter]` (`"`). It can contain `[introducer]` (`:`), but it must not be part of the identifier as leading or trailing (but `k:e:y: value` is valid, because the last : is not part of the identifier, but part of the `core syntax`. It must not occur in the identifier twice or more directly following eachother, as in `k::ey`.

In case `identifier unspecified` is having multiple lines, it can contain `[any unspecified]` data.

#### Block

There are cases where the possible data can be described only as block data. It can not be either inline or block, but it can only be block. For example: A literal that specifically is multi-line data is termed block data (and it follows we know it is not single-line data).

> [!NOTE]
> It is primarily the format that is considered a block, not the contained data itself, which anyway can be absent. The syntax (format) allows for a block, thus it is a block.

| Data type            | Rules | Example            | Description                          |
| -------------------- | ----- | ------------------ | ------------------------------------ |
| `[identifier block]` | [0]   | `:::\n1\n2\n.`     | Used to identify a specific literal. |
| `[literal block]`    | [0]   | `(...)::\n1\n2\n.` | The data associated with a key.      |

[0] Any possible binary and non-binary character.

### Custom tokens

The `basic standalone tokens` can be redefined using `operator directive`. Because `basic composite tokens` are built using `basic standalone tokens`, only the latter can be changed, which consequently changes the former.

This can be used to for example define binary characters as tokens making it easy to handle any text data, for example in transmitting over a network. However, translating the data from one set of tokens to another, by just switching the tokens in the data, is not recommended, as the new set of tokens may collide with the contained data.

---

## Basic Definitions

Definitions of general terms as they apply in the context of _f3c_:

**Whitespace**: All characters matched by the POSIX character class `[:space:]` (e.g., space, tab, etc.), **excluding newline characters**.

**Line**: As by POSIX. A sequence of characters, or an empty sequence, that ends with a newline character (`\n`). A line can contain only one newline character.

**Character**: Any valid byte sequence recognized by the host system's encoding configuration, including but not limited to UTF-8, UTF-16, UTF-32, ASCII, extended ANSI (ISO-8859 series), and arbitrary binary data (including null bytes \x00). Character interpretation depends on the system's locale (LC_CTYPE), encoding settings, and application behavior. See also `Note on supported characters`.

**Data**: The `f3c` format consists of several lines making up the configuration data.

**Parser**: Any program that reads the `f3c` data, evaluates it and preforms desired actions.

**Nesting**: One group can have a subgroup which in turn can have a subgroup, and so on. This is called nesting.

**Identifier**: A identifier is used to identify a specific literal.

**Literal**: A literal consists of a series of characters or no characters (empty).

**Identifier-literal pair**: The identifier and its corresponding value form an identifier-literal pair. An identifier-literal pair can be one of two types: an identifier with a _single-line literal_ or a identifier with a _multi-line literal_.

**Element**: A single-line literal or a multi-line literal.

**Nameless element**: An element without an identifier.

**Named element**: An element with an identifier.

# Remove array object, put below

**Array**: A multi-line literal that is a collection of `array` elements. An `array` element consists of anything but an identifier-literal pair. Thus, an `array` must not have elements classified as `object`.

**Object**: A multi-line literal that is a collection of `object` elements. An `object` element is an identifier-literal pair. The identifier-literal can be a `nameless element` or a `named element`. Content of object must not have elements classified as `array elements`, meaning no content that lacks identifier (empty identifier is still considered an identifier).

**Empty**: _Empty_ is a thing, syntax, that is empty of content. `""` is empty. It is absence of content, contained within `"` and `"`.

**Nothing**: _Nothing_ is not even a thing that is empty. It is the absence of syntax.

## How to read the description of the syntax

When describing the format, syntax elements are written in brackets for clarity, such as `[identifier] [assignment] [literal]`. This notation indicates that the **syntax element** `identifier` is followed by `assignment`, which is followed by `literal`.

However, **the spaces between and within the brackets are not part of the actual syntax**; they are included only to improve readability.

For example, `[identifier] [assignment] [literal]` translates to `key:value`, meaning that the **presentation does not imply whitespace** between these elements in actual use.

Either-or-statement is possible. The example `[custom | default terminator]` means either `custom terminator` or `default terminator`.

`(...)` means optional, where `...` here means that content which is optional.

`...` means "the rest of the expected syntax".

## Entry

### Comment syntax

Comment syntax is based on `comment tokens`.

| Label                 |      | Description                              |
| --------------------- | ---- | ---------------------------------------- |
| [comment]             | `--` | Comment marker for single-line comments. |
| [block comment start] | `/-` | Start of block comments.                 |
| [block comment end]   | `-/` | End of block comments.                   |

## Whitespace rules

Whitespace is trimmed before and after all and any occurence of _core syntax_ and _operator syntax_ with these exceptions:

### Exceptions for _core syntax_

- Whitespace is not trimmed at left side of `[literalizer] [default | custom terminator]` (because it is `[raw]` data, meant to be taken as it is).

> [!NOTE] > `[quote]` is trimmed left and right, but is of course not not trimmed within its two `[delimiter]`, e.g. within `"   this spaced string   "`.

### Exception for _operator syntax_

- Whitespace in the `[data]` part of the operator syntax is not necessarily trimmed. It depends on the directive. Include for example falls back on general parsing rules of _core syntax_, and thus the `[data]` part in that case is trimmed.

> [!NOTE]
> For whatever reason, a future directive may depend on trailing and leading whitespace, and it should be up to the directive to decide. The `core syntax`, however, is designed to be fully insensitive to whitespace.

### Empty lines

Empty are those with only whitespace or only a newline; these are ignored, with one exception.

Exception: if the empty line exists within an `[array]`; then it is treated as an empty _element_.

## Implementation

```
## Types

    # Single line identifier with multi-line assignment with fixed VT.
    # icc	identifier::
    readonly MAPPING_is=0

    # id ::
    # 	val1
    # 	val2
    # :

    # NOTE:
    # `ec` not possible. To literal without key, just do `l` instead.
    # `ecc` however is possible as multi-line literal without key.

    # Multi-line identifier and fixed VT.
    # ccc	:::
    readonly MAPPING_i=1

    # :::
    # 	ml-id-val1
    # 	ml-id-val2
    # :

    # Multi-line literal with empty identifier.
    # `e` stands for empty, and means two double quotes.
    # ecc	""::
    #
    # ""::
    # 	data1
    # 	data2
    # :
    #
    # Note:
    # No `ec` is possible, as that would be for single line, and simply `l` is used for that.

    # TODO: Insert this
    # Multi-line literal without identifier with assignment as LT.
    # eclc	"":lt:
    #

    # "":lt:
    # 	ea
    # 	eb
    # lt

    # Multi-line assignment without identifier and fixed VT. (No other thing on line allowed)
    # cc	::
    readonly MAPPING_i=1

    # [...]
    # ::
    # 	assignment-data1
    # 	assignment-data2
    # :

    # Extended functionality mode:
    # cc	::[directive]

    # Single line identifier with multi-line assignment with single-line assignment as VT.
    # iclc	identifier "assignment" :
    readonly MAPPING_ias=3

    # id: "vt":
    # 	ml-data1
    # 	ml-data2
    # :vt

    # NO, NOT TRUE: Multi-line identifier (naturally without identifier on the header line) with single-line assignment as VT.
    # or, depending on context:
    # NOT ANYMORE: Multi-line assignment without identifier and single-line assignment as VT.
    # Follows a multi-line identifier.
    # Follows only a multi-line identifier and is for defining multi-line literal, with assignment as LT.
    # NOTE: which one depends on what comes after/was before. If found without previous identifier, it is identifier, else it is assignment.
    #
    # clc	:"assignment":
    readonly MAPPING_as=4

    # Case 1
    # :"vt":
    # 	ml-id-val1
    # 	ml-id-val2
    # :vt
    #

    # Not true: Multi-line assignment without identifier and single-line assignment as VT. (Object)
    # clc	: assignment:
    readonly MAPPING_sA=7

    # Multi-line identifier with fixed LT.
    # ccc	"::

    # ::"abc":
    # 	ml-id-val1
    # 	ml-id-val2
    # :
    # ::
    # 	value1
    # 	value2
    # :

    # Multi-line identifier with literal as LT
    # cclc	::lt:

    # ::"lt":
    # or
    # ::lt:

    # 	ml-id-val1
    # 	ml-id-val2
    # lt

    # Single line assignment without identifier.
    # l	"assignment"
    readonly MAPPING_a=8

    # Single line assignment without identifier. (Object)
    # l	assignment
    readonly MAPPING_A=9

    # Fixed terminators
    #
    # :	parsed value terminator (array/object)
    # ::	literal terminator
    #

    # Comments
    # B	/- other text
    # b    other text -/
    # BB	// other text

    # Technically:
    # ic	identifier:	# Empty assignment
    # cl	:"assignment"	# Empty identifier, just prints the value in an array same as "assignment" would
    # cl	:assignment	# would print it as object (a trimmed version).
```

## Comment rules

Block comment can start and end on same line. In case of block comments: Must not have anything other than whitespace before the starting marker and after the ending marker.

Line comment (`--`) must be on a line of its own. No data other than whitespace is allowed to precede it on the same line.

## Inline identifier with inline literal

## Inline ...

## Inline identifier (not: A Key with a Single-Line Value

The key and the value exist on the same line.

### Syntax

1. A key (required)
2. A value (required)

```

key name "the value"

```

### Identifier Rules

1. A key can consist of any character, including whitespace, except double quotes (`"`), colon (`:`), and newline (`\n`),
2. except it CAN contain colon (`:`) in middle, like `k:e:y` but not in start or end as in `:key:`, and it can not contain two colons subsequently as in `abc::def` (it will be seen as functional syntax).
3. It cannot start with commenting syntax: `#` or `--` and `/-`.
4. A key must contain at least one character, which must be a none non-whitespace character and not any of the exceptions in (1) or `#`.
5. A key starts and ends with a permissible non-whitespace character.
6. Whitespace within the key (between starting and ending non-whitespace characters) is allowed and considered part of the key exactly as it occurs. Example: `k e y` is a valid key.
7. Leading and trailing whitespace is not considered part of the key and is ignored.

> [!NOTE]
> Use `multi-line identifier` to be able to use keys with any characters.

### Key Placement Rules

1. The key must be at the start of the line.
1. After the key, a double quote (`"`) must be present.

```

my key "...

```

### Value Rules

1. The value starts at the first double quote (`"`) on the line.
2. The value ends at the last double quote (`"`) on the line.
3. If only one double quote is present, it is assumed the rest of the line is part of the value. (The parser corrects the syntax by adding a double quote at the end of the line.)

   - In case of syntax correction, the value is trimmed of leading and trailing whitespace.

> [!NOTE]
> The syntax correction allows for other types like true, bool, null, etc.

4. The value consists of all characters between the first and last double quotes on the line, in the exact in the order they occur. Values are treated literally.

These two are not the same value:

```

    my key "my value"...
    my key "  my value  "...

```

The value is as it occurs between the double quotes.

5. Characters between the first and last `"` **do not need to be escaped**.

```

    my key "my value is the "best" example"...

```

The above produces the value `my value is the "best" example`.

> [!NOTE]
> This simplifies the inclusion of quotes in values. The value can contain double quotes without needing to escape them.

### Trailing Characters Rules

1. If the pattern `"[[:space}]*:[[:space]]*$"`, that is `:`, is matched after the value, the line is considered a key with a _multi-line value_ (see further down).

Example of a key with multi-line value:

```

    my key "my value" :
    ...

```

2. If the line does not match the format of (1), then any characters after the last double quote (") and before the newline (\n) will be ignored.
3. Any character after `:` is ignored.

```

    my key "my value" this text is ignored
    my key "my value": this too

```

> [!TIP]
> Trailing characters can be used for comments or additional information.

### Whitespace rule

- Whitespace before and after any segment of the syntax is ignored.

These are parsed to have the same meaning:

```

my key "my value"
my key"my value"

```

### Line Validity

1. A line must include a valid key and at least two double quotes (`"`).
2. Lines without a valid key to the right and two double quotes anywhere to the left of the line are treated as _elements_ (see below).

These are not valid key-value pairs:

```

my key "my value
my key my value"
my key my value
"my value"
"my value" key

```

### Examples of parsing

The following will be parsed as:

**Key:** `key`
**Value:** `value`

```plaintext
key "value"
   key "value"
   key    "value"
   key"value"
```

The following will be parsed as:

**Key:** `key`  
**Value:** `value`

```plaintext
key "value" this text is ignored
key "value"this text is ignored
key"value"this text is ignored
```

The following will be parsed as:

**Key:** `key with spaces`  
**Value:** `value`

```plaintext
key with spaces "value"
    key with spaces    "value"
    key with spaces    "value" this text will be ignored
```

The following will be parsed as:

**Key:** `key`  
**Value:** `value with "quotes" that don't need to be escaped`

```plaintext
key "value with "quotes" that don't need to be escaped"
```

The following will be parsed as:

**Key:** `key with spaces`  
**Value:** `value with "quotes" that don't need to be escaped`

```plaintext
key with spaces "value with "quotes" that don't need to be escaped" this text is ignored
key with spaces"value with "quotes" that don't need to be escaped"this text is ignored
       key with spaces           "value with "quotes" that don't need to be escaped" this text is ignored
```

## Block identifier (not: Key with Multi-Line Value

A multi-line value consists of a key on one line, followed by its value spread across multiple consecutive lines.

No escaping is needed for any characters in the multi-line value.

### Span of a Multi-Line Value

A span can be defined in one of two ways: 1) Simple for quick use, and 2) advanced for more demanding cases.

Muti-line values can be introduced and ended in different ways.

#### Simple Method

For simplicity, do the following:

```
key:
```

End the span with the keyname on a consecutive line of its own:

```
[KEY NAME]
```

#### Advanced method

Define a custom value terminator:

```
key "[CUSTOM VALUE TERMINATOR]" :
```

The value span is ended like this on a consecutive line of its own:

```
[CUSTOM VALUE TERMINATOR]
```

Example:

```
my-key "end"
    value line1
    value line2
end
```

### Content of a Multi-Line Value

Each line of the content is trimmed by default.

To retain the literal string, end with `/` as follows:

```
key:
    ...
/key
```

If last newline is undesired, do this:

```
key:
    .../key
```

This last example returns `    ...` without a newline character at the end.

The following consequently returns nothing:

```
key:
/key
```

### Key Rules

For a key of a multi-line value, the same rules apply as for a key with a single-line value (see above).

#### Single-line Value Rules in Multi-Line value key

The value field of a key line with a multi-line value have two modes:

1. The value field contains the **value terminator**. It is a sequence of characters that determines the end of the multi-line value. If a value terminator is present, the span number is ignored.

**Example with span number:**

```plaintext
key "" :3
    value part1
    value part2
    value part3
```

**Example with value terminator:**

```plaintext
key "end"
    value part1
    value part2
    value part3
end
```

2. A key can be written with just keyname followd by colon and newline, and the span is ended by the keyname on a consecutive line.

```
key:
    ...
key
```

### Span of a Multi-Line Value

The span of a multi-line value is defined by the key line and the value terminator.

#### Span Defined by Value Terminator

The value terminator (see below) defineds the end of the multi-line value.

1. If having a value terminator, the span number, if given, will be ignored.
2. The span number can be omitted, and the syntax is `key "end" :`.

Example of ignoring the span number:

```plaintext
key "end" :
    value part1
    ...
    end
```

### Value Terminator

A value terminator is used to mark the end of a multi-line value span.

1. A custom value terminator can be used to specify the end of the multi-line value, and is specified in the value field of the key line.

```plaintext
key "end" :
    value part1
    ...
    end
```

The value terminator is a string that is used to mark the end of the multi-line value.

2. The value terminator must be on a line by itself.
3. The value terminator is not part of the value and is not returned by the parser.
4. The value terminator is:
   - matched exactly as defined in the value field of the key line,
   - the matched-against value terminator, which is supposed to be the ending of the multi-line value, is matched:
     - exactly as given on the line against the defined value terminator,
     - and if that fails; is trimmed and matched against the defined value terminator.

> [!NOTE]
> Point (4) thus allows visual structuring by indenting the value terminator, but the parser will still match it as if it was not indented.

5. If the key is defined without custom value terminator, but is entered with just colon, like `key:`, then the keyname itself (`key`) becomes the value terminator and is used on a consecutive line to end the span..
6. The value terminator can occur:

- On a line by itself.
- On the same line as data, and then `/` is used before value terminator, like `/[value terminator]`.

7. when `/` syntax is used, the multiline value is treated literally, otherwise each line is trimmed in the final result of querying for data.

### Trailing Characters Rules

Any trailing characters are ignored:

```plaintext
    key "" :3 this text is ignored
    key "" :3:2 this text is ignored
```

- A double quote in the trailing part will make everything prior, back to the first double quote, part of the value, thus disabling any span number and selector syntax. This `:3 "three" is the number` makes `:3 "three" is the number` part of the value..

Thus, this disables the span number and selector syntax:

```
    key "" :3:1,2 "three" is the number
```

### Whitespace rule

- Whitespace before and after any segment of the syntax is ignored.

These are parsed to have the same meaning:

```plaintext
key "":3
key "":3abc
key "" :3
key "" : 3
key "": 3
key "": 3abc
key "": 3 abc
```

These are the same:

```
    key "":3:2this text is ignored
    key "" : 3 : 2 this text is ignored
```

### Multi-Line Value Rules

#### With non-literal value terminator

1. The ending newline character of each line of the multi-line value is considered part of the value.
2. Leading and trailing whitespace are trimmed.

Thus, the following:

```plaintext
key:
    value part1
    value part2
    value part3
key
```

gives the following value:

```plaintext
value part1
value part2
value part3
```

3. If only one line is returned of a multi-line value, for whatever reason, a newline is present at end of line.

#### With literal value terminator

- The line is returend litearlly as it is. The `/` syntax marks the end, and if is on a line of its own beneath data, then a newline is returned. If it is on same line as data, then no newline is returned.

Returns newline:

```
key:
data
/key
```

Does not return newline:

```
key:
data/key
```

> NOTE:
> This is another method: If surrounding whitespace is desired to be preserved, it should be included by using the same key multiple times. See the section on returning the value of identical keys under "Parsing Rules" below.

### Element

A line is treated as an element as long as it does not match the format of a key-value pair, either for a _key with a single-line value_ or the first line of a _key with a multi-line value_.

Elements can be used for comments or structural annotations (e.g., grouping with labels like `[Group 1]`). These annotations are solely for human readability and are not intended to be processed by default. However, parsers can process elements as desired, allowing to define custom behaviors for how elements are handled.

By using elements and whitespace, a visual hierarchy can be created to improve the readability of the configuration file.

#### Example with Elements

```plaintext
Group 1
    key1 "value1"
        key2.1 "value2.1"
        key2.2 "value2.2"
    # Comment
    key3 "value3"

Group 2
    key4 "value4"
    key5 "value5"
```

In this example:

- `Group 1` and `Group 2` are elements used as structural labels. They hold no meaning to the parser.
- The line `# Comment` is an element used as a comment. It holds no meaning to the parser.
- The remaining lines are key-value pairs. They are meaningful to the parser.

## Parsing Rules

## Nesting

Nesting is possible.

1. Any key-value pair can be nested within another multi-line value span.

```
top level "abc" :
	key "abc" :
		key2 "abc" :
			key3 "xx"
				xa
				xb
				xc
    			key4 "test"
			xx
		abc
	abc
abc
```

Or:

```
top level:
	key:
		key2 :
            key3:
				xa
				xb
				xc
    			key4 "test"
		    key3
	    key2
    key
top level
```

The command to the parser would be:

```bash
echo -n "$data" | _parser "top level" "key" "key2" "key3"
```

and would output:

```
xa
xb
xc
```

### Pipelining Parsing of Nested Key-Value Pairs

The parser is built to handle this by accepting several keys as a list of arguments to traverse a nested heirarchy. However, this method can also be used.

> [!NOTE]
> This is not necessary as traversing is already a built in feature of the parser.

The parser returns the value of a key, and because the value in turn can contain further key-value pairs, the output can be piped back to the parser with the argument of the desired key at the next nesting level. In short: The nesting feature enables group-selectors through pipelined parsing.

Observe how, in the example below, the same keys (`user` and `password`) are used for both `local` and `remote` groups:

`example-data.lkc`:

```plaintext
local "" :2
    user "name1"
    password "pwd1"

remote "" :2
    user "name2"
    password "pwd2"
```

We can extract, for example, the `user` value for the `remote` group by first selecting the `remote` group and then selecting the `user` key. This is achieved through pipelining:

```bash
cat example-data.lkc | ./_parser "remote" | ./_parser "user"
```

The above outputs `name2`.

### Parsing Multi-Line Value Rule

If a given line is a multi-line value key, but doesn't match the key being queried for, the parser must skip parsing the lines of the span belonging to that key, and continue parsing after the span.

Thus if we query for `keyx` and the following is the configuration data:

```plaintext
key ""
    keyx "not this"
    value part2
    value part3
/key
```

It should not return "not this". That line with key `keyx` is inside a multi-line span, being the value of another key named `key` and is thus not parsed.

### Returning the Value of a Key

A single-line value is returned as-is, without any trailing newline characters. Only the content between the double quotes is returned.

A multi-line value is returned as-is, with all newline characters included. The parser must neither add nor remove any newline characters. Every line, including the last one, should end with a newline character.

If only a single line is returned regardless of where it comes from (single- or multi-line value), no newline character is added even if the value comes from a multi-line value span.

A multi-line value with literal value terminator is returend ias it is, with whatver amount or no amount of newline characters as it is in the span.

### Returning the Value of Multiple Identical Keys

- If the same key occurs multiple times in the configuration data being parsed, the parser should return all values for all occurences of the key.
- Each value is separated by a newline character.
- The parser should return the values in the order the corresponding keys appear in the configuration data.
- Single-line values are not trimmed of leading and trailing whitespace, but multi-line values are.

Use his feature to include surrounding whitespace in the value, as shown in the example below:

````plaintext
key "  value part1"
key "    value part2"
key "       value part3"
```

Then, querying for `key` will return:

```plaintext
  value part1
    value part2
      value part3
````

The identical keys will be read and printed as they occur, and any other unrelated keys will be ignored. In the following example, `otherkey` is ignored if queriying for `key`:

```plaintext
key "value1"
otherkey "othervalue"
key "value2"
key "value3"
```

The output is:

```plaintext
value1
value2
value3
```

### Disabling a key by double quote

Adding a double quote to the start of the line will invalidate the syntax thus disabling the key.

```plaintext
"key "value"
```

> [!WARNING]
> Disabling a multi-line value key this way will not prevent the parser from reading and trying to parse the lines within the span of the multi-line value. Any nested keys will become available.

## Behaviour on non-match

If a line does not match the format of a key-value pair, it is treated as an _element_.

## Error handling

The parser should return the value of the key matched.

If a key is **not** and thus no value printed:

1. the parser should not output anything.
2. the parser should return an error code of 1.

If the key is found and a value is printed, even if the value is empty:

1. the parser should output the value of the key.
2. the parser should return an error code of 0.

> [!NOTE]
> As long as the parser printed something (even if an empty single line value, which effectively prints nothing), the error code should be 0.

If the key is found, but is a multi-line value key that has content in the key-line's value part (`"example content"`):

1. the parser should output the value as a singl line value.
2. the parser should return an error code of 0.

If the same key is encountered again after the above case, but this time is considered valid (either single-line or multi-line value key):

1. the parser should output the value of the key.
2. the parser should return an error code of 0.

If the value of the key is empty:

- the parser should not output anything. However, the parser still considered that it did print something, just that it was an empty value. The parser returns error code of 0, indicating the key was found, and the empty response in fact is the value.

If the span number is 0:

- the parser should not output anything. The parser should return an error code of 0, indicating the key was found, even if nothing was printed.

The same applies to empty multi-line value:

```
key "abc" :
abc
```

The error code should be 0.

## Parser options

### Row number of a key

The parser should have option to return row number of a key. The option should:

- return the row number of a single-line value key. Format: `ROW`
- return the row number of a multi-line value key and the number of the last line of the corresponding multi-line value span. Format `ROW1:ROW2`

Purpose of the above: With a row number of a key, a specific key and value in the config file can easily be edited with standard command-line tools.

### Print all lines considered elements

The parser should have an option to print all lines considered elements. Purpose: easy review the correctness of the parsed config so no keys are mistyped and taken as elements.

## Naming Conventions of f3c Configuration Files

The preferred file extension for f3c configuration files is `.lkc`. Using this extension reduces the likelihood of conflicts with other configuration files.

## Design choices

1. Whitespace before and after any segment of the syntax is ignored, allowing for minor formatting inconsistencies without causing errors. Howvever, all spaces inside "" on the key line are considered part of the value.

2. A key-value pair can be preceded by whitespace, such as spaces or tab characters, allowing visual structuring for better readability.

### For key with single-line value

- The format `key "value"` was chosen over `key = value` because, in the latter, it is not possible to determine whether any trailing whitespace is part of the value. In the former, the `"` clearly marks the end of the value.
- To use only one character as delimiter, do like `key " text`. That is; without any ending quote.

### For key with multi-line value

1. Multi-line values were introduced to avoid having to encode the string, as for example with base64.
   The syntax is:

```plaintext
key
    value line1
    value line2
    ...
    value lineN
/key
```

4. The colon was choosen as it improves visual clarity and conveys the meaning of a subsequent sequence (of lines).

## Data Serialization

### Control characters

## Future possible updates

Support for changing the syntax characters. Magic keys to configure the parser on the fly, to change syntax characters.

```
...-value-delimiter "/"
```

Define a common value terminator and able to do like `key "$" :` to have the parser replace `$` with the predefined common value terminator.

If settings like above, make a default config file that is always parsed before the target file. A settings file in ~/.config.

## Considerations for update

Allow for both `'` and `"`?

## Implementation

About the implementation of anything that handles data in the form of this format.

An implementation is classified as one, and only one, of the following:

A _feature_ of the format is defined as anything that the specification explicitly states, e.g. if leading and trailing whitespace is ignored, then that is a feature, and if a newline (`\n`) is used to separate lines, then that is a feature.

| Level    | Description                                                                                                     |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| Addapted | Lesser adhearance to the format than _standard_.                                                                |
| Standard | All features and charactersitics as defined by the format, except those explicitly stated to be for _complete_. |
| Complete | Same as _standard_ together with all aspects explicitly stated to be for _complete_.                            |

| Mode       | Description                                                                                                           |
| ---------- | --------------------------------------------------------------------------------------------------------------------- |
| Faithful   | An _adapted_, _standard_, or _complete_ implementation that strictly adheres to the format without any modifications. |
| Functional | An _addapted_, _standard_ or _complete_ implementation, but only to the extent that the target environment supports.  |
| Prefered   | An _addapted_, _standard_ or _complete_ implementation, but with additions and modifications as prefered              |

### **Faithful Mode**

- Faithful Adapted - Lesser features than _standard_, but implements them exactly as defined in _standard_.
- Faithful Standard - Exactly as _standard_.
- Faithful Complete - Exactly as _complete_.

### **Functional Mode**

- Functional Adapted - Some features of _standard_ are excluded because they are not supported by the target environment. Additionally, features of _standard_ that can be implemented, are not required to be implemented. However, any _standard_ feature that are implemented must be implemented exactly as defined in _standard_.
- Functional Standard - Features of _standard_ are implemented exactly as defined in _standard_, but only to the extent that the target environment supports. Features of _standard_ that can be implemented must be implemented.
- Functional Complete - Features of _complete_ are implemented exactly as defined in _complete_, but only to the extent that the target environment supports. Features of _complete_ that can be implemented must be implemented.

### **Preferred Mode**

- Preferred Adapted - Lesser features than _standard_, and with modification of those features possible, and additions of other features possible, as preferred.
- Preferred Standard - All features of _standard_, with modifications of those features possible, and with additions possible, as prefered.
- Preferred Complete - All features of _complete_, with modifications of those features possible, and with additions possible, as prefered.

### Note on supported characters

It is up to the specific implementation to determine which characters are supported. Implementations should strive to support as many characters as possible.

- A **Bash-based implementation** supports **all binary and textual characters**, except for the **null byte (`\x00`)**, as Bash cannot handle data containing null bytes.
- A **C-based implementation**, however, **can fully support null bytes (`\x00`)**, as well as all other possible byte values.

### Purely binary parser

A parser designed to process data as binary considers all data binary, and does not need to consider any newline characters. Newline characters are treated as any other byte. However, the newline character that is part of multi-line syntax itself, telling where the literal-content starts, is still used, and, is, by definition, not part of the data being contained).

It always only have `[literalizer] [default | custom terminator]` as the end of a multi-line value (not only `[default | custom terminator]`).

### Redefining syntax

An implementation can redefine all `basic standalone tokens`. These make up the all possible syntax. The defaults are `:`, `.`, and `"`.

The reason for the ability to redefine the `basic standalone tokens` is because, for example, a dedicated network transmission format may desire to use other characters, such as binary ones for the syntax.

### Comments

An implementation can skip support for comments.

### Operator directives

An implementation may choose to not implement the _token configuration directives_.

A full implementation must include support for the _template directives_.

## Conceptual analysis

The syntax is relying on an iderlying conceptual coherence.

### Start

_f3c_ exists within a space of bytes, structured as a two-dimensional sequence of sequential bytes separated by newline characters, forming distinct lines. These lines, in turn, occur sequentially, creating a structured two-dimensional byte space. Within this structured space, f3c organizes its structured data, defining relationships and meaning within the sequential byte representation.

_f3c_ recognizes structured data by determining the nature of the current line. Either it matches the expactations of _f3c_, and in such case, `something` is recognized to exist. A line that does not match the expactations is considered `nothing`. A `something` can consist of several lines. If one or more lines have been identified to match `something`, but not all lines that defines that `something`, then `something` is only assumed to exist. If `something` is still only assumed to exist, and the next line does not align with what is expected, then the assumed `something` is considered `nothing`.

`Structured data` of f3c is not a file, but is a sequence lines recognized as `something`. The `structured data` starts at the line where the `something` is first recognizes, and ends at the line where the `something` is no longer recognized (excluding whitespace and empty newlines).

### Rationale for `empty identifier`

An `empty identifier` (`""`) is still an identifier. An identifier consists of form. Form is syntax that encapsulates content. An identifier without content is still an identifier by virtue of existing as form. An empty identifier is still an unique identifier.

### Form, absence, presence, emptiness and nothingness

If there is no form (no syntax), then there is `nothing`. Nothing is not defined merely as the absense of content but as the absence of the very form (syntax) itself. Nothingness is different from emptiness. Emptiness exists when there is a boundaried space in which there is nothing. All there is, is the boundaries. Absence and presence is in relation to a containing form. In nothingness, on the other hand, the boundaries, form, does not exist. Consequently, neither absence or presence can be known.

### Meaning of content

It follows that the meaning of content is derived from the boundary encapsulating it. The content is meaningless without the boundary, the form, the syntax.

While `nothing` is the absence of boundary, it is not necessarily the absence of content. However, by statement (1), the presence or absence of any content, can only be percieved if there is a containing form, boundaries. Thus, if the content is not complying with an expected form, it must still comply with a higher order form in which the content is present in relation to. (For example: The content is not in our expected format, but it is in the higher form of a sequential stream of bytes. Because we percieve the existence of "sequential stream of bytes" we know there is content.

We can not know if a form is present or absent, because that would require a higher level form standing in relation to the form that is deemed absent or present--the form in question.

Absolute nothingness is the absolute non-perception of any form, so that there is no content possible to be known to be present or absent. In absolute nothingness, it is also impossible to know the presence or absence of any form.

Knowing is the awareness of a thing to have a distinct state with an unique set of characteristics, different from every other possible state of characteristics. Two identical forks still have different characteristics of location in space. An object, content, thus is not limited to physichal deliniation by matter, but is defined also by every other factor such as location in space and time. The fork that resides on the table for 1 hour is not the same fork at minute 30 as it was on minute 0, because they have different attributes of time. Only an object that is percieved (content within form) is known to exist and it exists as it is percieved. An object is always percieved in completeness. If half an object is percieved, then the object is defined as a complete half object. The "halfness" is part of the objects attribute.

The parser recognizes `nothing` to be anything that is not recognized by the expected format (form).

Because nothingness is the absense of form, there is no way to percieve nothingness. Form is needed to percieve absence and presence. Only by relating to a boundary, do we know if something exist or not within that boundary. Without form, boundary, we can not determine if content is present or absent. The parser can however determine if form is present or absent. This is because the parser is aware of the higher level format that dictates that a stream of bytes can be piped to the parser and that those bytes being read are exactly that--bytes being piped in a stream. The act of reading the stream is itself a structured process. Assuming the bytes are being streamed sequentially, it can conclude weather the form (syntax) exist or not, within that stream, and if the syntax exist, it can determine the presence or absence of its content, and the parser know the meaning of that content due to the form, the syntax, the boundary. The form gives meaning. Without form, the content is meaningless, nothingness. Nothingness is the absence of form, but not necessarily the absence of content. Meaningless content is percieved as content because there is a higher level form (bytes are streamed sequentially), by which we can analyze the content to determine if it is meaningless or if it falls within the syntax and thus is meaningful. Nothingness is not an absolute trancendental state, but is an assumption based in the perception of meaningless content, due to failure to have it comply to the expected format.

## Leftover

Third category _[unspecified]_: The syntax only allows for _inline_ and _block_ data, but a third category _unspecified_ (`[unspecified]`) is possible, but limited only for the parser to identify a state at which it yet does not know what type of data it is. Thus, whereas _inline_ and _block_ data are part of the syntax, _unspecified_ data is not part of the syntax, but a state of the parser.

Used by literalized block data: _[[literalizer] [default \| custom terminator]]_ as in block identifier
\*\*

#### Inline identifier - sequence types

Different data formats are defined with different rules for character sequence.
Identifiers use these sequence types for its possible data.

A sequence type defines what characters are allowed in a sequence of characters.

A sequence type is one of the following sequence types: `any`, `any line`, `specific 1`, `specific 2`.

| Label        | Rules      | Description                                                                                                                                                                            |
| ------------ | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [any]        | No rule    | Any possible binary and non-binary character. Used by literalized block data: _[[literalizer] [default \| custom terminator]]_ as in block identifier (and the same for block literal) |
| [any line]   | \*         | Used by [terminator definition] and [quoted string] literal.                                                                                                                           |
| [specific 1] | `a-z0-9_-` | Used by [directive] in with [operator].                                                                                                                                                |
| [specific 2] | \*\*       | Used by [inline identifier]                                                                                                                                                            |

If a _[string]_, then it can contain any character except `[delimiter]` (`"`), `[introducer]` (`:`) and `[segmenter]` (`\n`).

If a _[fragment]_, then it can contain any character except `[delimiter]` (`"`), `[introducer]` (`:`) and `[segmenter]` (`\n`).

Any character except `[introducer]` (`:`) and `[segmenter]` (`\n`). But if the sequence is deliniated by surounding `[delimiter]` (`"`), then then it can contain `[introducer]` (`:`) and even `[delimiter]` (`"`), meaning any character except `[segmenter]` (`\n`).
