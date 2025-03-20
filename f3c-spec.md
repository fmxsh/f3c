# f3c - Fine Format For Configuration

> [!NOTE]
> This project is under development. Do not expect finished order.

# Table of Contents

- [f3c - Fine Format For Configuration](#f3c---fine-format-for-configuration)
  - [Porupose](#porupose)
  - [Introduction](#introduction)
  - [Terminology](#terminology)
    - [&lt;&lt; Concept and representation](#-concept-and-representation)
    - [&gt;&gt;](#)
  - [Basic Definitions](#basic-definitions)
- [Remove array object, put below](#remove-array-object-put-below)
  - [Pre-fundamental things (substrate)](#pre-fundamental-things-substrate)
  - [Basic Tokens (conceptual definitions)](#basic-tokens-conceptual-definitions)
    - [Basic standalone tokens](#basic-standalone-tokens)
    - [Basic composite tokens](#basic-composite-tokens)
  - [Data (conceptual definitions)](#data-conceptual-definitions)
    - [Fields](#fields)
    - [Data formats](#data-formats)
    - [Data types](#data-types)
  - [Data constructs (conceptual definitions)](#data-constructs-conceptual-definitions)
    - [Inline identifier](#inline-identifier)
    - [Inline literal](#inline-literal)
    - [Block identifier](#block-identifier)
    - [Block literal](#block-literal)
  - [Delineation of data (conceptual definitions)](#delineation-of-data-conceptual-definitions)
    - [Inline data](#inline-data)
    - [Block deliniation](#block-deliniation)
  - [Binding structures (conceptual definitions)](#binding-structures-conceptual-definitions)
    - [Inline-Inline Binding](#inline-inline-binding)
    - [Inline-Block Binding](#inline-block-binding)
    - [Block-Inline Binding](#block-inline-binding)
    - [Block-Block Binding](#block-block-binding)
  - [Emergence and compliance](#emergence-and-compliance)
    - [Nesting](#nesting)
    - [Note on <em>[inline terminator definition]</em>](#note-on-inline-terminator-definition)
  - [Format overall](#format-overall)
    - [Identifiers must be unique](#identifiers-must-be-unique)
  - [Basic representations (semantic attribution)](#basic-representations-semantic-attribution)
    - [Ghostspace and whitespace](#ghostspace-and-whitespace)
    - [Basic standalone representations](#basic-standalone-representations)
      - [Rationale for the choice of dot as finalizer](#rationale-for-the-choice-of-dot-as-finalizer)
    - [Basic composite representations](#basic-composite-representations)
    - [Representations for comments](#representations-for-comments)
    - [Ghostspace and whitespace](#ghostspace-and-whitespace-1)
  - [Data (semantic attribution)](#data-semantic-attribution)
    - [Inline identifier](#inline-identifier-1)
    - [Inline literal](#inline-literal-1)
      - [String](#string)
      - [Fragment](#fragment)
      - [Number](#number)
      - [Bool](#bool)
      - [Null](#null)
      - [Terminator definition](#terminator-definition)
      - [Terminator expression](#terminator-expression)
    - [Block identifier](#block-identifier-1)
    - [Block literal](#block-literal-1)
  - [Binding structures (conceptual definitions)](#binding-structures-conceptual-definitions-1)
  - [Inline deliniation syntax (semantic attribution)](#inline-deliniation-syntax-semantic-attribution)
  - [Block deliniation syntax (semantic attribution)](#block-deliniation-syntax-semantic-attribution)
  - [Inline-Inline Binding (iib) (semantic attribution)](#inline-inline-binding-iib-semantic-attribution)
    - [With explicit identifier](#with-explicit-identifier)
    - [With implicit identifier](#with-implicit-identifier)
    - [With empty literal](#with-empty-literal)
  - [Inline-Block Binding (ibb)](#inline-block-binding-ibb)
    - [With explicit identifier with custom terminator definition](#with-explicit-identifier-with-custom-terminator-definition)
    - [With explicit identifier with default terminator definition](#with-explicit-identifier-with-default-terminator-definition)
    - [With implicit identifier with default terminator definition](#with-implicit-identifier-with-default-terminator-definition)
    - [With implicit identifier with custom terminator definition](#with-implicit-identifier-with-custom-terminator-definition)
  - [Block identifiers generally](#block-identifiers-generally)
    - [With default terminator definition for block identifier](#with-default-terminator-definition-for-block-identifier)
    - [With custom terminator definition for block identifier](#with-custom-terminator-definition-for-block-identifier)
  - [Literals following block identifiers](#literals-following-block-identifiers)
    - [Block-Inline Binding](#block-inline-binding-1)
    - [Block-Block Binding with default terminator definition](#block-block-binding-with-default-terminator-definition)
    - [Block-Block binding with custom terminator definition](#block-block-binding-with-custom-terminator-definition)
  - [Directive syntax (semantic attribution)](#directive-syntax-semantic-attribution)
    - [x Defining types](#x-defining-types)
    - [Operator directive](#operator-directive)
      - [Template directives](#template-directives)
      - [Parser controling directives](#parser-controling-directives)
      - [Token configuration directives](#token-configuration-directives)
  - [Parsing d](#parsing-d)
    - [Streaming format](#streaming-format)
    - [Identical identifiers](#identical-identifiers)
    - [Parsing block literal that doesn't match queried identifier](#parsing-block-literal-that-doesnt-match-queried-identifier)
  - [Implementation](#implementation)
    - [Binding codes](#binding-codes)
      - [Inline format](#inline-format)
      - [Block identifiers](#block-identifiers)
      - [Literals following block identifiers](#literals-following-block-identifiers-1)
      - [Directives](#directives)
    - [Custom tokens](#custom-tokens)
  - [Entry](#entry)
    - [Comment syntax](#comment-syntax)
    - [Empty lines](#empty-lines)
  - [Implementation](#implementation-1)
  - [Comment rules](#comment-rules)
  - [Inline identifier with inline literal](#inline-identifier-with-inline-literal)
  - [Inline ...](#inline-)
  - [Inline identifier (not: A Key with a Single-Line Value](#inline-identifier-not-a-key-with-a-single-line-value)
    - [Syntax](#syntax)
    - [Key Placement Rules](#key-placement-rules)
    - [Value Rules](#value-rules)
    - [Trailing Characters Rules](#trailing-characters-rules)
    - [Whitespace rule](#whitespace-rule)
    - [Line Validity](#line-validity)
    - [Examples of parsing](#examples-of-parsing)
  - [Block identifier (not: Key with Multi-Line Value](#block-identifier-not-key-with-multi-line-value)
    - [Span of a Multi-Line Value](#span-of-a-multi-line-value)
      - [Simple Method](#simple-method)
      - [Advanced method](#advanced-method)
    - [Content of a Multi-Line Value](#content-of-a-multi-line-value)
    - [Key Rules](#key-rules)
      - [Single-line Value Rules in Multi-Line value key](#single-line-value-rules-in-multi-line-value-key)
    - [Span of a Multi-Line Value](#span-of-a-multi-line-value-1)
      - [Span Defined by Value Terminator](#span-defined-by-value-terminator)
    - [Value Terminator](#value-terminator)
    - [Trailing Characters Rules](#trailing-characters-rules-1)
    - [Whitespace rule](#whitespace-rule-1)
    - [Multi-Line Value Rules](#multi-line-value-rules)
      - [With non-literal value terminator](#with-non-literal-value-terminator)
      - [With literal value terminator](#with-literal-value-terminator)
  - [Error handling](#error-handling)
  - [Parser options](#parser-options)
    - [Row number of a key](#row-number-of-a-key)
  - [Naming Conventions of f3c Configuration Files](#naming-conventions-of-f3c-configuration-files)
  - [Future possible updates](#future-possible-updates)
    - [1](#1)
    - [2](#2)
  - [Implementation](#implementation-2)
    - [Versioning](#versioning)
    - [Note on supported characters](#note-on-supported-characters)
    - [Redefining syntax](#redefining-syntax)
  - [TODO](#todo)
  - [Removed](#removed)
    - [Print all lines considered elements](#print-all-lines-considered-elements)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->

## Porupose

Define a configuration format with the following properties:

- with minimal set of tokens, support as much functionality as possible,
- key-value pairs,
- nesting,
- escapeless - no escaping of characters,
- allow for data serialization,
- allow for custom type checking,
- ...

## Introduction

This document is written primarily for completeness and exhaustive detail as a reference for implementation of related parsers. 

We start by defining semantics. Semantics are the particular units of meaning, or conceptual definitions, used to fully define this configuration format. Thus we first concern ourselves with the concepts and structures among concepts, and only later focus on how these concepts and structures map to actual characters and character sequences, the most concrete representations, to form the concrete syntax reflecting the formats purpose.

Following the intended approach, conceptual definitions are introduced for _basic tokens_ and _data_. These will yield higher emergent conceptual definitions, that are structures of meaning, called _binding structures_. These _binding structures_, providing the function of associating one field with another, that is; binding an identifier to a literal; represents the highest level of emergent conceptual property that is the complete conceptual response to the purpose of defining this particular configuration format. The reality of concrete representations, as compared to the space of conceptual thought, then undergoes a process of semantic attribution, where the conceptual definitions imbude the representations with conceptual structural meaning. Concepts are mapped to representations. Structureed meaning is determined in the concrete; syntactical meaning is given to to selected characters such as `:`, `.` and `"`), and with these, the concept of contained data of different forms can also be realized in the concrete.

## Terminology

The format of this configuration format has _representational elements_ and _conceptual elements_. An _representation element_ is the character or set of characters being the concrete representation of a given concept. All given concept within this configuration format falls under the category _conceptual elements_.

A _conceptual element_ is indicated by `[` and `]` like `[identifier]` or `[literal]`. A _representational element_ is that concrete data--the character or characters--that aligns with the rules outlined as the concept.

A conceptual discussion may be: `[identifier] [introducer] [literal]`. The representational expression is, for example: `MyIdentifier: my literal`.

### << Concept and representation

The conceptual level deals with the concepts of the configuration format, the generic structures, their relations and principles, and how they respond to the purpose of defining a configuration format. Each concept is a _thing_ created by defining what it is, but without reference to concrete representatoin. As such, the concept is a self containing _thing_ within its own level, in that it neither depends on a concrete representation to be percieved, nor on something more abstract. Its medium of existence is the space of thought. These self containing things form relationships, yielding higher structures that becomes a higher level _thing_, still in the space of thought, emerging from the relationchips between the lower things in the space of thought. The representational level deals with how these _things_ in the space of thought are represented at the concrete non-thought level of characters and parsing. The representation should be in complete alignment with the conceptual.

### >>

When describing the format, syntax elements are written in brackets for clarity, such as `[identifier] [assignment] [literal]`. This notation indicates that the **syntax element** `identifier` is followed by `assignment`, which is followed by `literal`.

However, **the spaces between and within the brackets are not part of the actual syntax**; they are included only to improve readability.

For example, `[identifier] [assignment] [literal]` translates to `key:value`, meaning that the **presentation does not imply whitespace** between these elements in actual use.

Either-or-statement is possible. The example `[custom | default terminator]` means either `custom terminator` or `default terminator`.

`(...)` means optional, where `...` here means that content which is optional.

`...` means "the rest of the expected syntax".

## Basic Definitions

TODO: Not all of them apply anymore...

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

## Pre-fundamental things (substrate)

At this level, we have not yet entered the stage where any _meaning_ specific to this configuration format can be seen. _Meaning_ is defined as an occurrence of a thing or a structure which is a set of things and their relations, in the perceived material, that aligns with our purpose. At this level, without any meaning, without any recognizable structure directly responding to put purpose, we have pre-fundamental _things_. Thus, this is a level prior to the most fundamental level of the configuration format, where the _fundamental level_ is the first _things_ that has meaning relating to our purpose. At that level, things are defined and named as suitable building blocks of our purpose.

At this current _pre-fundamental_ level, for the sake of elaboration, we are only concerned with the things (characters) and structure (sequence of characters and sequence of lines), which comprises the very existence of _a_ _something_, which is the perceived material, within which specific structures can be defined to have higher meaning that can only arise by combinations and relations among these _pre-fundamental things_. The possibility of such emergent higher meaning, based in patterns of lower level things, is what allows the definition of structures specifically meaningful for the purpose of a configuration format. Meaning is thus contingent on patterns that are built from these lower-level constituents. The contingence of meaning is a fundamental principle that will replicate itself in the relationship between each previous and each current level of abstraction, in the sense that the previous is fundamental to the meaning that emerges at the current level, as a combination of fundamental things that the current level recognizes a structure that is meaningful to the overall purpose. Any fundamental level must be well explicated to allow for the next level of abstraction to percieve meaningful structures among occurences of these fundamental things. _The abstraction _ is the very definition of the structure—referred to by its newly created name—that is seen among the particular occurrence of the fundamentals.

**pre-fundamental characters**: Any valid byte sequence recognized by the host system's encoding configuration, including but not limited to UTF-8, UTF-16, UTF-32, ASCII, extended ANSI (ISO-8859 series), and arbitrary binary data (including null bytes \x00). Character interpretation depends on the system's locale (LC_CTYPE), encoding settings, and application behavior.

**pre-fundamental sequence**: A sequence of _pre-fundamental characters_

**pre-fundamental line**: As by POSIX. A sequence of _pre-fundamental characters_, or an empty sequence, that ends with a newline character (`\n`). A line can contain only one newline character. It follows that lines occur in sequence, one line following another.

These terms will later in the document be simplified to _character_, _sequence_, and _line_ while carrying the same meaning.

## Basic Tokens (conceptual definitions)

These are called _basic tokens_ and the most fundamental building blocks of the format.

### Basic standalone tokens

All of the syntax, without exception, is built using these _basic tokens_.

| Label        | description                                     |
| ------------ | ----------------------------------------------- |
| [introducer] | Always indicates something follows.             |
| [finalizer]  | Always puts an end to multi-line data.          |
| [delimiter]  | Always delimits single-line data.               |
| [segmenter]  | Separates elements and parts within an element. |

These represent essence that reoccur in specific forms in the syntax. Each is also bound to a specific representation (character) in the syntax.

### Basic composite tokens

Composite tokens are built using the `basic standalone tokens`, such that changing a standalone token would change the related composite token.

| Label                   | Composition               | Description                                                                                            |
| ----------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------ |
| [meta level introducer] | [introducer] [introducer] | Same as _introducer_ but a higher level of abstraction.                                                |
| [emptier]               | [delimiter] [delimiter]   | Used to indicate that no identifier exists for the multi-line literal that follows. (Nameless element) |

> [!IMPORTANT]
> The composite token `[meta level introducer]` is not the same as two individual [introducer] in a row (`[introducer] [introducer]`), even if they may seem identical. [metal level introducer] is considered its own indivisible unit.

## Data (conceptual definitions)

Besides the _basic tokens_, there is _data_. Data is a debut delineated sequence of characters or no characters ('emptiness' exist within a delineated space, and thus, the very absence of characters is considered to be the very data of that space). Data in its most general form can contain any character.

A general rule applying to all _data_ is that no character will ever be interpreted as a token, and consequently, no character is ever escaped.

Data is categorized into _fields_. A _field_ can be one of several _data constructs_. What particular _data constructs_ a that a particular _filed_ consists of, is determined by analyzing the field by aspects of _data formats_ and _data types_. Combining _data formats_ and _data types_ in different ways, yields different _data constructs_ which then defines the type of field.

### Fields

A _field_ is a delineated space that contains data. Fields can have different purposes. _Field_ thus is a term to refer to, more specifically, a field of data, meaning a deliniated sequence of characters or no characters.

In order to determine the type of a field, the data is analyzed by _data formats_ and _data types_.

### Data formats

A data format is defined by the way the data is deliniated.

_Inline deliniation_ is where the data is deliniated to start and end on the same line.

_Block deliniation_ is where the data is deliniated to start and end on different lines.

All types of data fall into one of two categories of data format: _inline_ or _block_.

| Label    | Data type | Description                                 |
| -------- | --------- | ------------------------------------------- |
| [inline] | Inline    | Data that exists only inside a single line. |
| [block]  | Block     | Data that must be multi-line data.          |

More precicely:

[inline] assumes [inline format] and [block] assumes [block format].

The labels _[inline format]_ and _[block format]_ refers to the corresponding structure as made up by the syntax including the specific data being contained by the syntax.

The labels _[inline data]_ and _[block data]_ refers only to the data within the syntax, not the syntax itself.

### Data types

Data can be one of two categories of data types: _identifier_ or _literal_.

| Label        | Data type  | Purpose    | Description                                 |
| ------------ | ---------- | ---------- | ------------------------------------------- |
| [identifier] | Identifier | Matching   | A name used to identify a specific literal. |
| [literal]    | Literal    | Retrieving | The data associated with an identifier.     |

These two serve different purpose. _Identifier_ identifies a segment of data which is called a _literal_. Functionally, _identifier_ is matched against an user-given identifier-construct when querying. The literal is consequently retrieved or not, depending on the match.

Two modes of identifiers are possible: _implicit identifier_ and _explicit identifier_.

In case of _implicit identifier_: _empty_ is specified as the identifier. Then an identifier should be determined by the ordered sequence of entries, as related to a zero-based indexing, in the parent entry containing the associated literal.

In case of _explicit identifier_: _something_ is specified as the identifier to associate with the literal.

## Data constructs (conceptual definitions)

The _data types_, _[identifier]_ and _[literal]_, can be combined with the data formats _[inline]_ and _[block]_ to form four combinations called _data constructs_.

A _data construct_ refers to the structure of the actual data itself, but not the syntax defining the boundary of the data and tells nothing about the structure containing the _data construct_.

| Label               | Description                                          |
| ------------------- | ---------------------------------------------------- |
| [inline identifier] | An identifier that exists only inside a single line. |
| [block identifier]  | An identifier that must be multi-line data.          |
| [inline literal]    | A literal that exists only inside a single line.     |
| [block literal]     | A literal that must be multi-line data.              |

The four types are descried as follows:

### Inline identifier

Represented by _[inline identifier]_ is a sequence of characters on a single line, with the purpose of identifying something.

### Inline literal

Represented by _[inline literal]_ is a sequence of characters on a single line, that is the data associated with an identifier.

Possible literals are:

| #   | Label                   | Category  | Quality    | Determined by | Literal type |
| --- | ----------------------- | --------- | ---------- | ------------- | ------------ |
| 1   | [string]                | String    | Atomic     | context       | value        |
| 2   | [fragment]              | String    | Non-atomic | content       | value        |
| 3   | [number]                | Primitive | Atomic     | content       | value        |
| 4   | [bool]                  | Primitive | Atomic     | content       | value        |
| 5   | [null]                  | Primitive | Atomic     | content       | value        |
| 6   | [terminator definition] |           |            | context       | marker       |
| 7   | [terminator expression] |           |            | context       | marker       |

1-5 is _data_, meaning it is _content_. 6 and 7 are considered _context_; they are syntactic markers, meaning they are literals part of the syntax and not just data values.

The term _atomic_ is used, meaning the value is taken as given. A non-atomic value is a value that is not taken as given, but is processed in some way.

### Block identifier

Represented by _[block identifier]_ is a sequence of characters spaning multiple lines, with the purpose of identifying something.

Block identifiers can be of two types: `raw` `array` including, in a certain way, `object`.

| Label   | Description                                                                  |
| ------- | ---------------------------------------------------------------------------- |
| [raw]   | Raw data exactly as it occurs, nothing is altered. Any character is allowed. |
| [array] | One or several literals. Surrounding whitespace of each literal is trimmed.  |

In case of _[raw]_, the data is considered as it is.

### Block literal

Represented by _[block literal]_ is a sequence of characters spaning multiple lines, that is the data associated with an identifier.

Block literals can be of three types: `raw` `array` and `object`.

| Label    | Description                                                                 |
| -------- | --------------------------------------------------------------------------- |
| [raw]    | Raw data exactly as it occurs, nothing is altered. Any character is allowed |
| [array]  | One or several literals. Surrounding whitespace of each literal is trimmed. |
| [object] | One or several identifier-literal pairs.                                    |

**Raw**: The acceptable character sequence is: Any binary and non-binary character.

**Array**: Must consist of only one or more literals with _implicit identifiers_. Can not contain any _[literal]_ with _explicit identifiers_.

**Object**: Must consist of one or more _[literal]_ with _explicit identifiers_. Can not contain any _[literal]_ with _implicit identifiers_.

## Delineation of data (conceptual definitions)

### Inline data

The _inline_ syntax centers around _[introducer]_ such that `[identifier] [introducer] [literal] [segmenter]` and `[identifier] [meta level introducer] [literal] [segmenter]`.

By the knowledge of the _pre-fundamental_ understanding of what a _line_ is, how it starts and ends, and together with the concept the concept of the [introducer], it is known where one inline _field_ of data starts and ends. At the left side of the [introducer] is the eventual [inline identifier] and at the right side is the eventual [inline literal]. It is said 'eventual' because either side can be a [block], but the point is that any inline data is known and delineated in the way described.

### Block deliniation

A block has a start and an end in a multi-line space. The start is either of _[explicit inline identifier]_ and _[implicit | explicit block identifier]_. In either case, an associated _[terminator definition]_ is given or assumed, and the end, at a later line, is _[terminator expression]_.

A block can be terminated in one of two ways:

| Type             | Description                                                                  |
| ---------------- | ---------------------------------------------------------------------------- |
| soft termination | `[terminator expression]` on a line of its own at the end of the block data. |
| hard termination | `[literalizer] [terminator expression]` after the intended end of the data.  |

A terminator definition, and consequently also a terminator expression, is always _inline_.

The _[terminator definition]_ is defined at the start boundary of the block, where the _[terminator expression]_ is used at the end boundary of the block, such that:

```
[...] [terminator definition]
[block data]}
[...] [terminator expression]
```

The use of the two literal types _[terminator definition]_ and _[terminator expression]_ makes it clear they are entierly dependent on context (where and how they are used) to be identified as such literals and not fall back on other forms of literals. Also, the _[inline data]_ of these two literal types is never evaluated to determine its type.

## Binding structures (conceptual definitions)

Based in _basic tokens_ and _data_ the highest organizing structures are built. They are called _binding structure_.

A binding structure binds an _[identifier]_ to a _[literal]_ into an _[entry]_. All entries must have an identifier to separate it from another entyry. We can not use the data itself as entry, because then the same data can not occur twice. Thus we identify each separate literal by an identifier that is bound to it. An identifier can be both _implicit_ and _explicit_. In the first case, no identifier is specified alongside the data, for example `The Data`, and thus the implicit identity is zero-based indexed in sequential order of occurence. An _explicit_ identifier is one that is specified alongside the data, for example `The key: The data`.

All literals have corresponding identifiers associated, as explained. There is no literal that has no identity.

All structural meaning of the format arises from this basic concept of binding the two fields of _[identifier]_ and _[literal]_ together in that particular order. All syntax center around facilitating this binding. What a binding does is to bind an identity to a given set of data which is the literal, for example this valid syntax: `fruit: banana`. The identifier is `fruit` and the literal is `banana`.

The _binding structure_ is the fundamental meaning of the format. By analyzing the content and recognizing certain patterns of _basic tokens_ and _data_, meaningful structures of binding _[identifier]_ to _[literal]_ are recognized. Any other content that is not recognized is considered to be _nothing_, which means the content is in the absence of fundamentally meaningful structure.

The following structures of binding are recognized:

| Shorthand | Full Name             | Identifier Type | Literal Type |
| --------- | --------------------- | --------------- | ------------ |
| [iib]     | Inline-Inline Binding | Inline          | Inline       |
| [ibb]     | Inline-Block Binding  | Inline          | Block        |
| [bib]     | Block-Inline Binding  | Block           | Inline       |
| [bbb]     | Block-Block Binding   | Block           | Block        |

An identifier is always to the left, and a literal is always to the right.

The _binding structure_ binds _[identifier]_ to _[literal]_, not purely for organization of data, but to support the function of querying and fetching data within the heirarchy of structures. It is at this level—where the binding structure is recognized—that the format attains functionality. Percieving _binding structure_ allows for querying and fetching.

### Inline-Inline Binding

[inline identifier] bound to [inline literal]

### Inline-Block Binding

[inline identifier] bound to [block literal]. A block literal can be [raw], [array] or [object]. Because it can be of [object], it allows for nesting, as an [object] contains other bindings of identifier and literal.

### Block-Inline Binding

[block identifier] bound to [inline literal]. The block identifier can be [raw], [array] or serialized [object]. A _serialized [object]_ has the structure of an object (looks like an literal with identifier and content that consists of only identifier-literal pairs), but is never parsed as such, because there is no utility. [object] are for querying the contained data, but no querying inside an identifier ever happens. We use identifiers to query associated data. We do not use identifiers to store data to be queried for. If we would query data inside identifiers, we could have another [object] inside that, with data inside its identifier, and so on, for endless recursion and nesting.

### Block-Block Binding

[block identifier] bound to [block literal]. The same principles for each block type applies as described in the two previous sections. The unique feature being that a block idenfities _data_ that is recognized as [block].

## Emergence and compliance

Based in all things established till this point, an emergent feature, as well as an aspect of compliance with previously defined structure, can be ovserved.

### Nesting

Because, by definition, a literal can be a block of data that can contain one or several other bindings, the fundamental structure allows for recursion and thus nesting of bindings.

Nesting of entries is a feature possible because of the _binding structure_. One entry can be contained within another, and so on.

It is the [literal] part of the `[identifier] [introducer] [literal]` that holds nested entries. The question is if a block identifiers can contain nested entries? An identifier can a contain nested structures, but those structures will never be analyzed as binding structures, but only taken line by line and will be seen as either _[raw]_ or as _strings_ or _primitives_ of _[array]_. A _[block literal]_ on the other hand, has its content analyzed for the purpose of fetching nested data. In the case _[block identifier]_, on the previous hand, data is never fetched from an identifier, only compared. Thus, nesting serves no purpose in identifiers.

_[identifier]_ and _[literal]_ serves different purposes. The identifier is used merely for identification of literal, while the literal is the data itself that can be of a type that can contain other entries meaningful for a parser that may be looking for one entry within another, but the parser will never look within an identifier for the sake of finding a part of it. The parser always uses the whole identifier in matching against a target given identifier for sake of determining if the current entry is desired for futher consideration.

If an identifier can have nested structures that are meaningful as such, it technically could have another entry with another nested identifier and so on for eternal recursion.

### Note on _[inline terminator definition]_

The _[inline terminator definition]_ is recognized by the same principle as any other _binding structure_. An _[inline terminator definition]_ always follows the format of `[identifier] [introducer] [literal]`. Thus, identifier has its ordinary place to the left, and to the right a literal is found. Be aware that the terminator definition is for a block of data, and thus an [introducer] is added at the end: `[identifier] [introducer] [literal] [introducer]`, example: `id: term:`. In this example, the following lines comprise the [block literal] that is terminated, after those lines, by `term`.

As stated, the [inline terminator definition] is recognized by the principle of _binding structures_, but, then, not treated the same as, say, a [string] literal. The aquired literal in question, the terminator definition, is not used for retrieval in a query, unlike what a [string] literal would be used for, but for the need to know where the consequtive set of lines ends, so that that set of lines can be retrieved as the queried-for literal.

## Format overall

The configuration format does not define any necessary beginning and end. Thus, the format is considered a stream rather than a file.

### Identifiers must be unique

Identifiers are considered unique identifiers of a set of data, but can still occur more than once in the same stream.

## Basic representations (semantic attribution)

Out of all characters of the _pre-fundamental characters_, certain ones will be given special meaning. The selected characters will be the concrete representations of the conceptual tokens outlined earlier, and will be the most fundamental building blocks of the syntax. Characters, that otherwise can represent anything, are given the general meaning of being tokens, where each token has a specific uniquely defined meaning.

Thus, this section outlines the basic concrete representation as linked with the conceptual tokens, making up the syntax.

### Ghostspace and whitespace

All _ghostspace_ is trimmed and ignored. Only what is defined by each _conceptual element_ and thus instantiated by the correpsonding _representational elements_ matters to the parser.

Before and after any _representational element_, there can be any or no amount of ghostspace.

In other words: whitespace in syntax (called _ghostspace_) does not affect the meaning of the syntax or the data (but within the boundary of a [literal] whitespace is kept).

These are the same `id : 123` and `id:123` and `id: 123` and `  id :123`.

### Basic standalone representations

All syntax depends on these.

| Token         | Conceptual label | Description                                     |
| ------------- | ---------------- | ----------------------------------------------- |
| `:`           | [introducer]     | Always precedes something being defined.        |
| `.`           | [finalizer]      | Always puts an end to multi-line data.          |
| `"`           | [delimiter]      | Always delimits single-line data.               |
| `\n` or `EOF` | [segmenter]      | Separates elements and parts within an element. |

> [!NOTE]
> The `segmenter`, `\n` or `EOF`, refers strictly to syntax and is part of the syntax. It does not refer to the data contained by the syntax, like the data of a multi-line literal.

In case of `EOF`, it is used to indicate the end of the file, and is considered the same as `\n` in the context of the syntax.

#### Rationale for the choice of dot as finalizer

The colon was choosen as it improves visual clarity and conveys the meaning of something following that which was before, and implying a relationship between the follower and the followed. Initally `:` was intended for [finalizer] to keep the representations mininmal, but would conflict with the same representation used in other context (nested objects for example, we can not determine what ends what.) The `.` also adds a visual distinction, contradicting, in sense, the `:`. The discrepancy between `:` and `.` aligns with their essence of opening `:` and `.` closing. Colon (`:`), having two dots, implies a relatoin between two things as one have to relate to the other merely by exising in the same space. It aligns with the essence of _binding structures_ which is one thing (identifier) relating (bound) to another (a literal). Following up with a dot (`.`) consequently marks the end of a space in which things stand in relation to eachother, as a dot, by its visual content as a character, convays a singular thing. The dot, in _regex_ also symbolizes _any character_ and when we end with a dot, the parser accepts any character, looking for structure again. Thus, `.` symbolizes the end of structure and \_from now on, anything follows, till new structure is recognized. All structure tus involves the `:`.

Conceptually, following the above reasoning, it can be argued that; `.`, `\n` and `EOF` are all finalizers and logically we should be able to do the following all on the same line: `user: name. pwd: mypass. host: localhost.`

With this in mind, why can we not put `.` in the same category as `\n` and `EOF`? Conceptually, `.` is based in a higher level concept, meant to respond to [block] which is higher than [inline]. Enough are _character_, _sequence_ and _line_ to define an inline syntax, and still knowing how to end it, without introducing any new concept. `\n` and `EOF` are fundamental representations used to define the span of the inline data. [block], on the other hand, is a higher level construct, in the sense of building on several lines, where the `\n` thus can not uniquely represent the ending of the block, or we would not be able to include the next line of the multi-line data intended to be stored. Thus, a construct at the same level of conceptual emergence, needs to be in place. For this, we have the [inline terminator definition] and [inline terminator expression]. The `.` represents a default [terminator definition], and if used like `..` or `.myterminator` it means to take the content as [raw], where the first dot is [literalizer], indicating the content does not stand in relation to any oppinion of the format, but is taken as it is (not trimmed etc).

In summary, [segmenter] and [finalizer] exist at different conceptual levels. Using `.` for inline syntax, as in `user: name. pwd: mypass. host: localhost.` would not, as first thought, make effective use of an already existing representation of a higher level (block), but would spawn a new representation for the same essence of putting an end to a thing, but at a lower level (inline). It would add redundancy of an additional representation, having `\n`, `EOF` and `.` serving the same purpose, for the illusion of conceptual and representational coherence, ignoring the reality of conceptually different levels. It may seem conceptually coherent that the `.` at the higher level of [block] should work to also terminate [inline], if desired, but it is not conceptually coherent, as the `.` would transgress its conceptual domain, trying to solve a lower level thing with a hihger level construct, that in itself was made first to solve the higher level structure that was built on the very lower level things. Conceptually, it would be similar having drawers deliniated by samller peices of wood, and drawers together are collected in a cabinet, which is deliniated with bigger pieces of wood, and then expecting to deliniate a drawer with the big wodden surface of the top of the cabinet. You would have drawers with the size of the cabinet surface, obviously being antagonostic towards the cabinette that then can not contain them.

### Basic composite representations

Representations of composite tokens are built using the `basic standalone representations`, such that changing a standalone token would change the related composite token.

| Token | Name                  | Description                                                                                            |
| ----- | --------------------- | ------------------------------------------------------------------------------------------------------ |
| `::`  | Meta level introducer | same as _introducer_ but a higher level of abstraction.                                                |
| `""`  | Emptier               | Used to indicate that no identifier exists for the multi-line literal that follows. (Nameless element) |

> [!IMPORTANT]
> The composite representation `::` is not the same as two individual `introducers` (`:`) in a row (`::`), but they may seem identical. For example: An identifier for a multi-line literal can be defined with a `terminator definition` as in `id:termdef:`, but if we desire to use the `default terminator` we leave it empty as in `id::`. In this case, by the very usage itself, we recognize we are dealing with two separate `introducer` representations and not one _meta level introducer_. Its even more clear by the fact we can space out the former case like `id: :`. The composite token `::`, on the other hand, is a single token, and always appears as `::` without any whitespace possible between the two colons of the composite.

### Representations for comments

Note: Comments do not have any conceptual representation, as they are not part of the data, but only part of the syntax. Comments are not structurally relevant to the format. If comments had conceptual counterparts, it would imply they are seen as meaningful by the system, but the comment by essence is used to denote something meaningless, non-existent, to the parser, meaning it is void of meaningful structure and should be ignored.

`--`
`/-`
`-/`

Nested block comments are not supported. A block comment is always terminated by the first occurrence of the terminator expression.

A block comment must start on a line with nothing or nothing but whitespace before it.

A block comment must end on a line with nothing or nothing but whitespace after it.

Comments are considered **ghost-tokens**, forming a **ghost-syntax**, because they exist to human perception but do not exist in the parsed structure.

A comment token is a **token that is never interpreted as a token** in the sense of relating to data.

To human perception, it appears that the comment syntax relates to the comment-text itself—i.e., the syntax _contains_ the comment. However, the parser does not merely ignore the comment-text; it also ignores the comment syntax.

In all other cases of syntax, the parser assigns meaning to the content, whether it is empty or not. In the case of a comment, no meaning is assigned to any contained data or to the comment syntax itself.

### Ghostspace and whitespace

[whitespace] is data that may or may not have meaning. As part of the data of a literal, whitespace retains its POSIX definition. In the case it occurs outside a literal's span, it lacks meaning, as no part of the syntax is dependent on whitespace, except in case of `\n`. `\n` has meaning to the syntax of being a [segmenter]. As a [segmenter], `\n` is not considered whitespace, but considered a representation of a token. A reason to not term it _whitespace_ is because the representation of [segmenter], can by definition of this format, be redefined to something else.

Whitespace as part of a literal, is having meaning, as it is part of the data. Whitespace outside a literal is considered _nothing_, as it is not part of anything the format regognizes as meaningful. Similarly to comments appearing to have meaning to human perception, but being inherently empty of meaning to the format, and thus called _ghost syntax_, the equally meaningless whitespace, indentation, may convay meaning to human perception, but is meaningless to the configuration format, and thus is called **ghost-space**.

Ghostspace is not just the literal representations of white space (`\t`, ` ` etc...) but such occurence that is regarded as not meaningful to the structure or data of the configuration format, and consequently is ignored.

Ghostspace is any representation (character) regognized as whitespace that is not of _conceptual element_ (eg. [segmenter], [string] etc...).

## Data (semantic attribution)

### Inline identifier

The acceptable character sequence is: Any character except `[delimiter]` (`"`), and `[segmenter]` (`\n`). Also that `[introducer]` (`:`) can not exist at the start or end of a sequence, and not occur twice or more directly following each other.

### Inline literal

A literal value is returned as-is by the parser, without any trailing newline characters.

As outlined conceptually, different types of literals exist: string, number, multi-line block etc... The exact type of literal is determined by first evaluating the surrounding syntax (context), and, if shown the type is not determined to match a certain set of types, then, as second, evaluation is preformed of the content contained by the syntax. It follows that, in evaluation of literal type, context has precedence over content. A string literal will have additional meaning, or not, depending of in what context it appears, and thus, context should be evaluated first.

Possible literals are:

| #   | Label                   | Literal  | Example         |
| --- | ----------------------- | -------- | --------------- |
| 1   | [string]                | String   | `"abc def"`     |
| 2   | [fragment]              | Fragment | `ghi a123`      |
| 3   | [number]                | Number   | `32, 3.14, -10` |
| 4   | [bool]                  | Bool     | `true, on, yes` |
| 5   | [null]                  | Null     | `null, nil`     |
| 6   | [terminator definition] | [1-5]    | [1-5]           |
| 7   | [terminator expression] | [1-5]    | [1-5]           |

Literal 6-7 are used to determine the end of block. They can be any of the previous 1-5.

All primitive literals (number, bool, null) are case-insensitive.

The data of all literals, except _[string]_, is considered to be the sequence that is with and from the first non-whitespace character to and with the last non-whitespace character. In case of [string] the data is considered that which is delinieated by the [delimiter].

Inline literals are always trimmed of whitespace, because whitespace is not part of the data (in case of string: it is trimmed outside its delimiter if a delimiter is given).

#### String

A sequence of characters or no characters deliniated by _[delimiter]_ (`"`) such that `[delimiter] (...) [delimiter]`. The surrounding _[delimiter]_ is not part of the literal's data. `"abc"` is `abc` but `""abc""` is `"abc"`, meaning, only the uttermost surrounding _[delimiter]_ is taken as marks of the data's boundary.

The acceptable character sequence is: Any binary and non-binary character except _[segmenter]_ (`\n`) as it would contradict the essence of _inline_ in _[inline literal]_.

The delimiters must be around the data, or it will be seen as part of the data. `id: "data"` gives data `data`, while `id: this "is" my data` gives as data `this "is" my data`. Works the same in all contexts where [string] can occur: [terminator definition], [terminator expression], [literal] with [explicit identifier] and with [implicit identifier].

> [!NOTE] > `[string]` is trimmed left and right, but is of course not trimmed within its two `[delimiter]`, e.g. within `"   this spaced string   "`.

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

Can be a _[string]_ or a _[fragment]_, and the accepted character sequence is the same as for those.

#### Terminator expression

While a _[terminator definition]_ defines the terminator for a block, a _[terminator expression]_ is the actual use of the terminator to end the corresponding block.

### Block identifier

Block identifier is parsed and compared to user given identifier.

The _[object]_ is considered serialized, as the parser never parses it as an object, but only line by line as any other data. Thus, there is no use in having an object in a block identifier, because it is never treated as an object. The reason is a _[block identifier]_ is used to identify a _[literal]_ and having literals with identifiers inside an identifier serves no purpose as literals are never queried for inside identifiers.

The parser does not assign zero-based index value based on position to the elements of a [block identifier] because no identifier is needed, as literals inside a [block identifier] are never queried for. Thus, no _implicit identifier_ exists for the literals.

### Block literal

Block literals are retrieved if the identifier matches the user given identifier.

In case of [block literal] being [array], each contained [inline literal] has an _implicit identifier_, meaning the literals are automatically assigned an zero-based index value based on the identifiers position in the array.

An empty [block literal] that is not _[raw]_ is treated as empty array. If it is not [raw] it must be [object] or [array], and in case it is empty, it is treated as an empty [array] and not empty [object].

> [!NOTE]
> Block literals were introduced to avoid having to encode the data, as for example is commonly done with with base64 when a format's literal type can only store certain characters.

In case of _[array]_, each line is considered _[inline literal]_ , meaning it can be any of _value_, being _[string]_, _[number]_, etc..., (but of course, the markers _[terminator definition]_ and _[terminator expression]_ are not considered part of the arrays content).

If the empty line exists within an [array]; then it is treated as an [empty] [inline literal].

A [block literal] of type [raw] is returned as-is, with all newline characters (and whitespace) included and none added to the output. In case of the content of [array] being returned, every line, including the last one, should end with a newline character, and only one. An empty line is seen as a literal with value [empty] and is returned as just a newline `\n` and nothing else. For this reason, it is important all lines of an [array] ends with one and only one '\n`. In case of content of [object] being returned, at least one [segmenter] ('\n') must follow each _binding structure_ that is contained, as to separate them.More [segmenters] can be added by the parser, but will not make a difference to the meaning of the content.

## Binding structures (conceptual definitions)

What follows is how different _binding structures_ are recognized.

**A note on _[inline terminator definition]_**: The _[inline terminator definition]_ is recognized by the same principle as any other _binding structure_. An _[inline terminator definition]_ always follows the format of `[identifier] [introducer] [literal]`. Thus, identifier has its ordinary place to the left, and to the right a literal is found. Be aware that the terminator definition is for a block of data, and thus an [introducer] is added at the end: `[identifier] [introducer] [literal] [introducer]`, example: `id: term:`.

## Inline deliniation syntax (semantic attribution)

The _inline_ syntax centers around _[introducer]_ (`:`) such that `[identifier] [introducer] [literal] [segmenter]` gives, for example, `id: my literal` and `[identifier] [meta level introducer] [literal] [segmenter]` gives, for example, `id:: my literal`.

1. An [inline identifier] can consist of any character, including whitespace, except double quotes (`"`), colon (`:`), and newline (`\n`),
2. except it CAN contain colon (`:`) in middle, like `k:e:y` but not in start or end as in `:key:`, and it can not contain two colons subsequently as in `abc::def` (in that case, `::` will be seen as operator syntax).
3. It cannot start with commenting syntax: `--` and `/-`.
4. An [inline identifier] must contain at least one character, which must be a none non-whitespace character and not any of the exceptions in (1).
5. An [inline identifier] starts and ends with a permissible non-whitespace character.
6. Whitespace within the [inline identifier] (between starting and ending non-whitespace characters) is allowed and considered part of the identifier exactly as it occurs. Example: `k e y` is a valid key and is different from `k  e  y`.
7. Leading and trailing whitespace is not considered part of the [inline identifier] and is ignored.

## Block deliniation syntax (semantic attribution)

Unlike _inline_ data, for which the span can be easily determined by recognizing the _pre-fundamental_ structure of a _line_, _block_ data, on the other hand, spans multiple lines and requires higher level artifacts to define its boundaries, in order to allow the desired escapeless flexibility of the format. Those higher level artifacts are _[terminator definition]_ and _[terminator expression]_, as defined earlier. With these, the structure of a block can be defined.

Like _inline_ syntax, _block_ syntax is also understood in its fullness in the _binding structures_ section, but requires additional things for its syntax to be possible.

Here the _basic tokens_ are given unique meaning, in the context of _[block]_, to define the concept of _[block]_.

| Label                | From basic token | Block syntax | Description                                                |
| -------------------- | ---------------- | ------------ | ---------------------------------------------------------- |
| [identifier proxy]   | [introducer]     | `:`          | To make block identifier.                                  |
| [literalizer]        | [finalizer]      | `.`          | Makes terminator explicit for block literal or identifier. |
| [default terminator] | [finalizer]      | `.`          | Default terminator for block literal or identifier.        |
| [empty identifier]   | [emptier]        | `""`         | Empty identifier when used in place of identifier.         |

To terminate a block: _[terminator expression]_ is used, which is the replication of the _[terminator definition]_ where the block is desired to end.

The _[terminator expression]_ can occur in two ways: a) `[terminator expression]` on a line of its own at the end of the block data, and b) `[literalizer] [terminator expression]` after the intended end of the data, meaning it can occur at the same line as the last data (thus the data will not end with a newline, as it is taken literally as it occurs within the block span).

In case _a_, the block is treated as _[array]_ or _[object]_ depending on the content. In case _b_, the block is treated as _[raw]_.

Because a terminator definition is part of the syntax to define the boundary of a block, the terminator definition itself can not be a block. It would conceptually lead to endless recursion if every nested _block terminator definition_ in turn define a _block terminator definition_ to define the end of the parent _block terminator definition_.

## Inline-Inline Binding (iib) (semantic attribution)

Identifier and literal both exist within the same line, and are thus _[inline format]_.

### With explicit identifier

Concept: `[identifier] [introducer] [literal]`

Code: `icl`

Example: `fruit: apple`

### With implicit identifier

Concept: `[literal]`

Code: `l`

Example: `apple`

> [!NOTE]
> Using _[empty identifier]_ (`""`) syntax is not possible in _[inline identifier]_. It would have the same meaning as simply stating _[inline literal]_ which is normal for stating an element of _[array]_. It would simply be a literal without a _[explicit identifier]_. It can be argued, `"": banana` should have been possible for sake of consistency as we use `""::` for _[block literal]_ with empty identifier, but it is not allowed. 1. There is no use for it. 2. Our available tokens are limited to align with the goal of a minimal syntax as stated by the purpose. 3) "":"" would not be possible, as data is deliniated by the outermost `"` to allow for an escape free format, which also aligns with the purpose of the format. 4. Defining another syntax for _empty inline identifier_ is not possible, as other aspects of the format need their own use of tokens, which would conflict with the attempt to define another syntax for _[empty]_ for _[inline identifier]_.

> [!NOTE]
> It is not said _without identifier_. It is said _with empty identifier_. It means it is empty of explicit identifier, but it will have an _implicit identifier_, which is the zero-based index of the entry in the parent entry.

To state _implicit identifier_ for _[inline literal]_, the _[empty identifier]_ is not used, unlike in the case of _[block literal]_. _implicit identifiers_ are assigned automatically for [inline literal] by just stating [inline literal].

### With empty literal

Concept: `[identifier] [introducer]`

Code: `ic`

Example: `fruit:`

Literal is assumed to be _[empty]_. It is the same as:

Code: `ice`

Example: `fruit: ""`

Note: the parser is adviced to normalize `ic` to `ice` before parsing.

## Inline-Block Binding (ibb)

Identifier exists within the same line, but the literal is _[block format]_ meaning the data of the literal spans multiple subsequent lines.

### With explicit identifier with custom terminator definition

Concept: `[identifier] [introducer] [literal] [introducer]`

Code: `iclc`

Example:

```
id: "vt":
    ml-data1
    ml-data2
vt
```

### With explicit identifier with default terminator definition

Concept: `[identifier] [introducer] [introducer]`

Code: `icc`

Example:

```
id::
    val1
    val2
.
```

### With implicit identifier with default terminator definition

To state _implicit identifier_ in _[inline identifier]_, the _[empty identifier]_ is used.

Concept: `[empty identifier] [introducer] [introducer]`

Code: `ecc`

Example:

```
""::
    val1
    val2
.
```

### With implicit identifier with custom terminator definition

Concept: `[empty identifier] [introducer] [literal] [introducer]`

Code: `eclc`

Example:

```
"":term:
    val1
    val2
term
```

## Block identifiers generally

The _[block identifier]_ is the same in case of both _[inline literal]_ and _[block literal]_.

There are the possible _[block identifier]_, but without the literal part, as the literal part is described in subsequent sections.

The identifier is always without identifier, as the identifier is the identifier for the subsequent literal. An additional initial [introducer] represents the start of the _[block identifier]_ structure.

### With default terminator definition for block identifier

Concept: `[introducer] [introducer] [introducer]`

Code: `ccc`

Example:

```
:::
    val1
    val2
.
```

Thus, the case `:::` is understood as: `[identifier proxy] [assignment] (terminator definition) [assignment]`. With the optional terminator definition, it would be `::term:`. This is differnt from operator syntax, which is also `::`, but requires text before the `::`. The operator syntax is considered a token in itself (the `::`), whereas the `identifier proxy` syntax consists of two tokens: `identifier proxy` and `assignment`. Thus, locically in the latter case, whitespace can exist between as in ` : :`, because the first `:` is in place of an `identifier` which naturally can be surrounded by whitespace.

### With custom terminator definition for block identifier

Same as previous case, but with a custom terminator definition for the literal.

Concept: `[introducer] [introducer] [literal] [introducer]`

Code: `cclc`

Example:

```
:::
    val1
    val2
.
:term:
    val3
    val4
.
```

## Literals following block identifiers

With _[block identifier]_ both _block-block binding_ (bbb) and _block-inline binding_ (bib) are possible.

Following the _[block identifier]_ is the literal, which then is specified on a line below the _[block identifier]_, and then, without any _[inline identifier]_, as the previously defined _[block identifier]_ is used as the identifier for the literal.

Thus, the literals, both _[block literal]_ and _[inline literal]_, are defined in the same way as previously described, but without any _[inline identifier]_.

### Block-Inline Binding

Concept: `[introducer] [literal]`

Code: `cl`

Example:

```
:::
    val1
    val2
.
: val3
```

### Block-Block Binding with default terminator definition

Concept: `[introducer] [introducer]`

Code: `cc`

Example:

```
:::
    val1
    val2
.
::
    val3
    val4
.
```

### Block-Block binding with custom terminator definition

Concept: `[introducer] [literal] [introducer]`

Code: `clc`

Example:

```
:::
    val1
    val2
.
:term:
    val3
    val4
term
```

## Directive syntax (semantic attribution)

Based on the same principle of _binding structures_, a _directive_ is defined, not with `:` as _introducer_, but with `::` as _meta level introducer_.

A directive is a command that is used to control the parser. A the structure of a directive is based on the highest organizing structure of the format, the _binding structure_. From this, as is the case for any other binding, a directive involves a kind of identifier bound to a kind of literal. The syntax in its most general form is `[field] [meta level introducer] [field]`.

To define the directive syntax, the _[operator]_ is introduced. The _[operator]_ is a _[meta level introducer]_ gaining a certain function and purpose.

| Label      | From basic token        | Block syntax | Description                |
| ---------- | ----------------------- | ------------ | -------------------------- |
| [operator] | [meta level introducer] | `::`         | Used to define directives. |

With the _[operator]_, the two surrounding _[field]_ are then defined as follows:

| type            | Allowed characters                                                        | Description            |
| --------------- | ------------------------------------------------------------------------- | ---------------------- |
| [directive]     | `a-z0-9_-`                                                                | Name of the directive. |
| [operator data] | Any possible binary and non-binary character except `[segmenter]` (`\n`). | Data associated.       |

With the above, the directive syntax is defined as: `[directive] [operator] [operator data]`.

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

## Parsing d

### Streaming format

The parser can have different modes of parsing: non-continious and continious. When in the first, the first occurence is returned, and no further checks are done. The parser quits parsing.

`EOF` must not mark the end of a stream, as the same daemon can be fed multiple files. In non-continious mode, the parser would likely consider it the end, but must not, but can continue parsing more files (say all files in a directory are piped to the parser. What makes the parser non-continious or continious is a matter of if the parser stops at the first matching identifier.

A parser may listen, or query for one or more identifiers.

The parser should process the the bindings in the order they occur in the input data, from top to bottom. The parser must process the lines them as they occur line by line, without having to collect all lines before allowing for querying of data.

### Identical identifiers

In the second case of continious parsing, the stream of configuration data is parsed continiously. In case the same identifier occurs again, it is considered the same unit of data being updated with the associated literal. An example is a daemon waiting for data in a stream, and updating some aspects of a system based on certain identifiers.

### Parsing block literal that doesn't match queried identifier

Nested structures implies that a deeper level is not of interest if the identifier of a particular scope doesn't match the queried identifier. However, the embeded content can be of interest depending on the purpose of the parser, if it for example is to make a map of "breadcrub" paths.

If a given line is a multi-line value key, but doesn't match the key being queried for, the parser must skip parsing the lines of the span belonging to that key, and continue parsing after the span.

Thus if we query for `keyx` and the following is the configuration data:

```plaintext
key::
    keyx "not this"
    value part2
    value part3
.
```

It should not return `not this`. That line with key `keyx` is inside a multi-line span, being the value of another key named `key` and is thus not parsed.

## Implementation

### Binding codes

For sake of implementing a parser, these codes are suggested, but must not be used.

| Code | Description        |
| ---- | ------------------ |
| i    | Identifier         |
| l    | Literal            |
| c    | introducer (colon) |
| e    | empty              |

#### Inline format

| Token  | Identifier Type | Terminator Definition | Example             |
| ------ | --------------- | --------------------- | ------------------- |
| `icl`  | Explicit        |                       | `fruit: apple`      |
| `ice`  | Explicit        |                       | `fruit:`            |
| `l`    | Implicit        |                       | `apple`             |
| `iclc` | Explicit        | Custom                | `fruit:term: apple` |
| `icc`  | Explicit        | Default               | `fruit::`           |
| `ecc`  | Implicit        | Default               | `""::`              |
| `eclc` | Implicit        | Custom                | `"":term:`          |

#### Block identifiers

| Token  | Identifier Type | Terminator Definition | Example   |
| ------ | --------------- | --------------------- | --------- |
| `ccc`  | Explicit        | Default               | `:::`     |
| `cclc` | Explicit        | Custom                | `::term:` |

#### Literals following block identifiers

| Token | Identifier Type | Terminator Definition | Example   |
| ----- | --------------- | --------------------- | --------- |
| `cl`  | Explicit        |                       | `: apple` |
| `cc`  | Explicit        | Default               | `::`      |
| `clc` | Explicit        | Custom                | `:term:`  |

#### Directives

| Token  | Identifier Type | Example             |
| ------ | --------------- | ------------------- |
| `i::l` | Explicit        | `file::/etc/global` |

### Custom tokens

The `basic standalone tokens` can be redefined using `operator directive`. Because `basic composite tokens` are built using `basic standalone tokens`, only the latter can be changed, which consequently changes the former.

This can be used to for example define binary characters as tokens making it easy to handle any text data, for example in transmitting over a network. However, translating the data from one set of tokens to another, by just switching the tokens in the data, is not recommended, as the new set of tokens may collide with the contained data.

---

## Entry

### Comment syntax

Comment syntax is based on `comment tokens`.

| Label                 |      | Description                              |
| --------------------- | ---- | ---------------------------------------- |
| [comment]             | `--` | Comment marker for single-line comments. |
| [block comment start] | `/-` | Start of block comments.                 |
| [block comment end]   | `-/` | End of block comments.                   |

Block comment can start and end on same line. In case of block comments: Must not have anything other than whitespace before the starting marker and after the ending marker.

Line comment (`--`) must be on a line of its own. No data other than whitespace is allowed to precede it on the same line.

### Empty lines

Empty are those with only whitespace or only a newline; these are ignored, with one exception.

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

# Single line identifier with single line literal

# icl identifier: literal

# fruit: apple

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

## Error handling

The parser should return the value of the key matched.

If a key is **not** found, and thus no value printed:

1. the parser should not output anything.
2. the parser should return an error code of 1.

If the key is found and a value is printed, even if the value is empty:

1. the parser should output the value of the key (but technically it outputs empty, which is no output.)
2. the parser should return an error code of 0.

If the value of the key is empty:

- the parser should not output anything. However, the parser still considered that it did print something, just that it was an empty value. The parser returns error code of 0, indicating the key was found, and the empty response in fact is the value.

## Parser options

### Row number of a key

The parser should have option to return row number of a key. The option should:

- return the row number of a single-line value key. Format: `ROW`
- return the row number of a multi-line value key and the number of the last line of the corresponding multi-line value span. Format `ROW1:ROW2`

Purpose of the above: With a row number of a key, a specific key and value in the config file can easily be edited with standard command-line tools.

## Naming Conventions of f3c Configuration Files

The preferred file extension for f3c configuration files is `.f3c`. Using this extension reduces the likelihood of conflicts with other configuration files.

## Future possible updates

### 1

Support for changing the syntax characters. Magic keys to configure the parser on the fly, to change syntax characters.

```
...-value-delimiter "/"
```

Define a common value terminator and able to do like `key "$" :` to have the parser replace `$` with the predefined common value terminator.

If settings like above, make a default config file that is always parsed before the target file. A settings file in ~/.config.

### 2

- Allow for both `'` and `"`?

- To use only one character as delimiter, do like `key " text`. That is; without any ending quote.

## Implementation

### Versioning

Uses [Variable Classification Format](https://github.com/fmxsh/vcf).

### Note on supported characters

It is up to the specific implementation to determine which characters are supported. Implementations should strive to support as many characters as possible.

- A Bash-based implementation supports all binary and textual characters, except for the null byte (`\x00`), as Bash cannot handle data containing null bytes.
- A C-based implementation, however, can fully support null bytes (`\x00`), as well as all other possible byte values.

### Redefining syntax

An implementation can change the characters, the representations, the `basic standalone tokens` are connected with. `:` for [introducer] can be changed to `-` for example.

The reason for the ability to redefine the `basic standalone tokens` is because, for example, a dedicated network transmission format may desire to use other characters, such as binary ones for the syntax.

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

## Removed

### Print all lines considered elements

The parser should have an option to print all lines considered elements. Purpose: easy review the correctness of the parsed config so no keys are mistyped and taken as elements.
