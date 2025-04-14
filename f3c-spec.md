# Definition of concept and representation of f3c

> <!NOTE>
>
> This project is under development. Do not expect finished order.

## Meta

### About

_f3c_ stands for _Fine Format For Configuration_. An overview can be found [here...].

This document defines the of the conceptual aspect of _Fine Format For Configuration_ (_f3c_). For a mapping to the concrete representation, see [f3c-rep.md](f3c-rep.md).

### Table of contents

<toc here>

### Structure of the document

This specification is based in the ontology of [_Structure of conceptual models_](https://github.com/fmxsh/socm-ontology) which is an ontology for conceptual modeling. This is not essential for understanding the specification, but it explains the format of the document.

## Identity

The _concept of f3c_ is a concept of a configuration and data serialization format with the following properties:

- with minimal set of tokens, support as much functionality as possible,
- key-value pairs,
- nesting,
- escapeless - no escaping of characters,
- allow for data serialization,
- allow for custom type checking,
- ...

## Constraints

### Scope

Only the concepts are described, not the concrete representation. In other words: _concept of f3c_ does not describe what the different concepts map to, for example <newline> may be discussed, but its representation `\n` is not.

Concepts may be exemplified by representations to better illustrate the concept.

### Context

A conceptual sequence must be present. That is a sequence of undifferentiated perceptual units, awaiting differentiation. (For example a sequence of UTF-8 characters.)

### The format

No escaping.

Minimal set of tokens.

Must support nesting.

Must support custom type checking.

...

## Constitution

### Conceptual syntax

_Conceptual syntax_ is used to denote concepts and express combinations of them to reflect meaningful structural relationships.

- A _conceptual element_ is denoted by `<` and `>` like `<identifier>` or `<literal>`.
- An expression of _conceptual syntax_ may involve one or several _conceptual elements_ such as in `<identifier> <assignment> <literal>`. The meaning here is that `identifier` is followed by `assignment`, which is followed by `literal`. In this case, the representation could, for example, be `distance: 10`.
- Spaces between the brackets are only used for readability and serve no other purpose. For example, `<identifier> <assignment> <literal>` may translate to `key:value`, meaning that the whitespace in the conceptual expression does not imply whitespace in the possible representational counterpart.
- Grouping is possible with `(` and `)`. For example, `(<identifier> <assignment> <literal>)` means that the whole expression is grouped together.
- Grouping is used to group elements of logical operators.
- Logical operators `&&` and `||` are possible. The example `( <custom> || <default terminator> )` means either `<custom terminator>` or `<default terminator>`.
- Nested groups can be expressed: `( ( <a> && <b> ) || ( <x> || <y> ) )`.
- `[]` means optional, so `<a> [<b>]` means that `<a>` is required, but `<b>` is optional.
- `...` is a placeholder meaning _subsequent content of relevant kind_, or it means _the rest of the expected syntax_.

### Representation

_Definition of concept and representation of f3c_ consists also of definitions of the specific representations, meaning the specific characters representing the concepts of _f3c_.

### Sequence of characters

A sequence of characters, typically a file of data, but can be any data stream, is needed to discern meaningful structure relating to _f3c_.

The term _data_ refers simply to a sequence of characters.

### Basic Tokens (conceptual definitions)

Certain characters are given special meaning as tokens, in order to be able to discern _f3c_ related structures within the given data.

Starting with a sequence of undifferentiated conceptual units (the series of characters being the configuration data), some of the units will be designated as _tokens_ for the purpose of differentiation and discernment of meaningful structure (for example, what is an identifier and what is the related literal).

These are called _basic tokens_ and the most fundamental building blocks of the format.

#### Basic standalone tokens

The entire concept of _f3c_, without exception, is built using these four _basic tokens_.

| Semantic label | Token | Description                                            |
| -------------- | ----- | ------------------------------------------------------ |
| <introducer>   | `:`   | Always indicates something follows.                    |
| <implicator>   | `.`   | Denotes an implicit value in place of an explicit one. |
| <delimiter>    | `"`   | Always delimits single-line data.                      |
| <segmenter>    | `\n`  | Separates elements and parts within an element.        |

These represent the basic units of the format, and each also connotes the very essence of their general meaning within the format.

The configuration format processes data segment by segment, meaning line by line.

#### Basic composite tokens

_Composite tokens_ are built using the `basic standalone tokens`, such that changing a standalone token would change the related composite token.

| Label                   | Composition               | Representation | Description                                                                                                                             |
| ----------------------- | ------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| <meta level introducer> | <introducer> <introducer> | `::`           | Same as _introducer_ but a higher level of abstraction.                                                                                 |
| <emptier>               | <delimiter> <delimiter>   | `""`           | TODO: This doesnt apply anymore: Used to indicate that no identifier exists for the multi-line literal that follows. (Nameless element) |

> <!IMPORTANT>
>
> The composite token `<meta level introducer>` is not the same as two individual <introducer> in a row (`<introducer> <introducer>`), even if they may seem identical. <metal level introducer> is considered its own indivisible unit.

## Fields

In the sequence conceptual units (think sequence of UTF8-characters), the presence of a field can be discerned by locating the token _<introducer>_ or _<meta level introducer>_, in conjunction with a <segmenter>. The <segmenter> typically maps to the newline character (`\n`) and thus, data is parsed line by line looking for <introducer> or <meta level introducer> in order to discern fields.

A field is at either side of the _<introducer>_ or _<meta level introducer>_ and is a delineation within the sequence of conceptual units. All the conceptual units within the delineation is the content of the field. The field can also be empty if there are no conceptual units within its delineation.

A field always have two aspects: _data format_ and _data type_. The aspect of _data format_ can be either _inline_ or _block_, and the second aspect of _data type_ can be either _identifier_ or _literal_. The two aspects together yields four possible combinations: inline identifier, inline literal, block identifier, block literal. These are four different _data constructs_. This is the structural identity of a field.

A _field_'s structural identity is a _data construct_, which is the emergent property of the combination of _data format_ and _data type_.

Properties:

- A field has two required properties:
  - Data Format: Defines how the field is delineated (inline / block).
  - Data Type: Defines the field’s functional role (identifier / literal).

Nesting is possible: a field may contain other fields, depending on its construct.

A general rule applying to all _fields_ is that none of its contained conceptual units will ever be interpreted as a token, and consequently, no conceptual unit is ever escaped.

As a summary of the three following sections: Data is categorized into _fields_. A _field_ can be one of several _data constructs_. What particular _data constructs_ a that a particular _filed_ consists of, is determined by analyzing the field by aspects of _data formats_ and _data types_. Combining _data formats_ and _data types_ in different ways, yields different _data constructs_ which then defines the type of field.

### Data formats

Recognition: By the way the field is delineated.

_Inline deliniation_ is where the data is deliniated to start and end on the same line.

_Block deliniation_ is where the data is deliniated to start and end on different lines.

Modes: All types of data fall into either of the two categories of data format: _inline_ or _block_.

| Data type | Description                                          |
| --------- | ---------------------------------------------------- |
| Inline    | Field content that exists only inside a single line. |
| Block     | Field content that must be multi-line data.          |

Interface labels:

| Label             | Description                                                                 |
| ----------------- | --------------------------------------------------------------------------- |
| `<inline>`        | Indicating inline data format.                                              |
| `<block>`         | Indicating block data format.                                               |
| `<inline format>` | Refers to the structure made up by the syntax, including the data it wraps. |
| `<block format>`  | Refers to the structure made up by the syntax, including the data it wraps. |
| `<inline data>`   | Refers to the data delineated by the syntax, excluding the syntax itself.   |
| `<block data>`    | Refers to the data delineated by the syntax, excluding the syntax itself.   |

### Data types

Recognition: By the function of the data.
Modes: Data can be either of two categories of data types: _identifier_ or _literal_.

| Data type  | Function    | Description                                 |
| ---------- | ----------- | ------------------------------------------- |
| Identifier | Identifying | A name used to identify a specific literal. |
| Literal    | Storing     | The content associated with an identifier.  |

Interface labels:

| Label          | Description                            |
| -------------- | -------------------------------------- |
| `<identifier>` | Label indicating identifier data type. |
| `<literal>`    | Label indicating literal data type.    |

### Data constructs (conceptual definitions)

Recognition: By the combination of _data format_ and _data type_.

Modes and interface labels:

| Label               | Description                                          |
| ------------------- | ---------------------------------------------------- |
| <inline identifier> | An identifier that exists only inside a single line. |
| <block identifier>  | An identifier that must be multi-line data.          |
| <inline literal>    | A literal that exists only inside a single line.     |
| <block literal>     | A literal that must be multi-line data.              |

The details of each As described in subsequent dedicated section.

## Inline identifier

An _inline identifier_ can be implicit or explicit.

Represented by _<inline identifier>_ is a sequence of conceptual units on a single line, with the purpose of identifying something.

## Inline literal

Represented by _<inline literal>_ is a sequence of characters on a single line, that is the data associated with an implicit or explicit identifier.

Possible literals are:

| #   | Label                   | Category  | Quality    | Determined by | Literal type |
| --- | ----------------------- | --------- | ---------- | ------------- | ------------ |
| 1   | <string>                | String    | Atomic     | context       | value        |
| 2   | <fragment>              | String    | Non-atomic | content       | value        |
| 3   | <number>                | Primitive | Atomic     | content       | value        |
| 4   | <bool>                  | Primitive | Atomic     | content       | value        |
| 5   | <null>                  | Primitive | Atomic     | content       | value        |
| 6   | <terminator definition> |           |            | context       | marker       |
| 7   | <terminator expression> |           |            | context       | marker       |

The _data_ of 1-5 are considered being _content_. _Content_ is delineated and known trough by syntax, but has no purpose to the syntax itself.

The _data_ of 6-7 are considered _context_. They define data that then is part of the syntax. They are syntactic markers, meaning they are literals that are being part of the syntax and serves a syntactical function.

The term _atomic_ is used, meaning the data is taken as given. Non-atomic data is a such that is not taken as given, but is processed in some way.

## Block identifier

Represented by _<block identifier>_ is a sequence of characters spaning multiple lines, with the purpose of identifying something.

Block identifiers can be of two types: `raw` `array` including, in a certain way, `object`.

| Label   | Description                                                                  |
| ------- | ---------------------------------------------------------------------------- |
| <raw>   | Raw data exactly as it occurs, nothing is altered. Any character is allowed. |
| <array> | One or several literals.                                                     |

In case of _<raw>_, the data is considered as it is.

- <x> In _f3c_, identifiers can be single-line, multi-line or array.

## Block literal

Represented by _<block literal>_ is a sequence of characters spaning multiple lines, that is the data associated with an identifier.

Block literals can be of three types: `raw` `array` and `object`.

| Label    | Description                                                                 |
| -------- | --------------------------------------------------------------------------- |
| <raw>    | Raw data exactly as it occurs, nothing is altered. Any character is allowed |
| <array>  | One or several literals.                                                    |
| <object> | One or several identifier-literal pairs.                                    |

## Raw

The acceptable character sequence is: Any binary and non-binary character.

## Array

An <array> must consist of only one or more literals with _implicit identifiers_. Can not contain any _<literal>_ with _explicit identifiers_. The literal, as long as having _implicit identifier_ can be <inline literal> and <block literal>, and an arrays content can be a mix of these two. The array does not concern itself with the type of literal, but with how it is identified, requiring the _implicit identifier_.

## Object

An <object> must consist of one or more _<literal>_ with _explicit identifiers_. Can not contain any _<literal>_ with _implicit identifiers_.

## Meta level bindings (conceptual definitions)

Using the <meta level introducer> it is possible to bind an <identifier> to a <literal> in the following way:

`<identifier> <meta level introducer> <literal>`

Everything after the <meta level introducer> is considered to be the <literal>. This means anything, including ordinary bindings can occur after the <meta level introducer>. This opens for several possibilities.

- Pre-processing of the associated literal which may be an ordinary bindings. For example, type checking can be done.
- Post-processing of the literal, such as Base64 decoding.
- Directives to the parser. For example tell the parser to include a file.
- Even control flow statements. For example, return the literal only if another literal is set to a certain value.
- Dynamic literals, a literal that has its value set dynamically, where the parser makes a system call getting the data before the literal is returned to the querier.

Meta level bindings can also hold block literals, by stripping itself out and allowing the parser to parse the lines as usual, with the directive part of the parser capturing the block data.

## Delineation of data (conceptual definitions)

Delineation of the _data_ of the two _data formats_ <inline format> and <block format> is possible by _tokens_ occurring in the patterns recognized by the structures outlined in the two following sections, and hereby given corresponding structural meaning.

### Inline data

The <inline format> centers around _<introducer>_ such that:

`<identifier> <introducer> <literal> <segmenter>`

and:

`<identifier> <meta level introducer> <literal> <segmenter>`

By the knowledge of the _pre-fundamental_ understanding of what a _line_ is, how it starts and ends, and together with the concept of the <introducer>, it is known where one inline _field_ of data starts and ends. At the left side of the <introducer> is the eventual <inline identifier> and at the right side is the eventual <inline literal>. It is said 'eventual' because either side can be a <block>, but the point is that any inline data is known and delineated in the way described.

### Block deliniation

The <block format> has a start and an end in a multi-line space. The start is either of _<explicit inline identifier>_ or _<implicit | explicit block identifier>_. In either case, an associated _<terminator definition>_ is given or assumed, and the end, at a later line, is _<terminator expression>_.

A block can be terminated in one of two ways:

| Type             | Description                                                                  |
| ---------------- | ---------------------------------------------------------------------------- |
| soft termination | `<terminator expression>` on a line of its own at the end of the block data. |
| hard termination | `<literalizer> <terminator expression>` after the intended end of the data.  |

A terminator definition, and consequently also a terminator expression, is always _inline_.

The _<terminator definition>_ is defined at the start boundary of the block, where the _<terminator expression>_ is used at the end boundary of the block, such that:

```
<...> <terminator definition>
<block data>}
<...> <terminator expression>
```

The use of the two literal types _<terminator definition>_ and _<terminator expression>_ makes it clear they are entierly dependent on context (where and how they are used) to be identified as such literals and not fall back on other forms of literals. Also, the _<inline data>_ of these two literal types is never evaluated to determine its type.

## Binding structures (conceptual definitions)

Based in _basic tokens_ and _data_ the highest organizing structures are built. They are called _binding structure_.

### General properties

A binding structure binds an _<identifier>_ to a _<literal>_ into an _<entry>_ using _<introducer>_. All entries must have an, at its level of nesting, unique identifier to separate it from other entries at the same level of nesting. There is no literal with identifier absent.

All structural meaning of the format arises from this basic concept of binding the two fields of _<identifier>_ and _<literal>_ together in that particular order. All syntax center around facilitating this binding. What a binding does is to bind an identity to a given set of data which is the literal, for example this valid syntax: `fruit: banana`. The identifier is `fruit` and the literal is `banana`.

### Implicit and explicit identifiers

A binding structure always consist of a _<identifier>_ and a _<literal>_. The _<identifier>_ can be either _implicit identifier_ or _explicit identifier_. In the first case, no identifier is specified alongside the data, for example, only this occurs on a line `The data`, and thus the implicit identity is the zero-based indexed of the literal'sposition in the parent field. An _explicit_ identifier is one that is specified alongside the data, for example `The key: The data`.

An _implicit identifier_ occurs when <implicator> is specified as the identifier. The identifier is then implied and determined by the ordered sequence in which the fields occur, based in zero-based indexing. This applies for both <inline literal> and <block literal>.

In case of _explicit identifier_: a _sequence_ is specified as the identifier to associate with the literal.

## The four specific binding structures (conceptual definitions)

The _binding structure_ is the fundamental meaning of the format. By analyzing the content and recognizing certain patterns of _basic tokens_ and _data_, meaningful structures of binding _<identifier>_ to _<literal>_ are recognized. Any other content that is not recognized is considered to be _nothing_, which means the content is in the absence of fundamentally meaningful structure.

The following structures of binding are recognized:

| Shorthand | Full Name             | Identifier Type | Literal Type |
| --------- | --------------------- | --------------- | ------------ |
| <iib>     | Inline-Inline Binding | Inline          | Inline       |
| <ibb>     | Inline-Block Binding  | Inline          | Block        |
| <bib>     | Block-Inline Binding  | Block           | Inline       |
| <bbb>     | Block-Block Binding   | Block           | Block        |

An identifier is always to the left, and a literal is always to the right.

The _binding structure_ binds _<identifier>_ to _<literal>_, not purely for organization of data, but to support the function of querying and fetching data within the heirarchy of structures. It is at this level—where the binding structure is recognized—that the format attains functionality. Percieving _binding structure_ allows for querying and fetching.

### Inline-Inline Binding

<inline identifier> bound to <inline literal>

### Inline-Block Binding

<inline identifier> bound to <block literal>. A block literal can be <raw>, <array> or <object>. Because it can be of <object>, it allows for nesting, as an <object> contains other bindings of identifier and literal.

### Block-Inline Binding

<block identifier> bound to <inline literal>. The block identifier can be <raw>, <array> or serialized <object>. A _serialized <object>_ has the structure of an object (looks like an literal with identifier and content that consists of only identifier-literal pairs), but is never parsed as such, because there is no utility. <object> are for querying the contained data, but no querying inside an identifier ever happens. We use identifiers to query associated data. We do not use identifiers to store data to be queried for. If we would query data inside identifiers, we could have another <object> inside that, with data inside its identifier, and so on, for endless recursion and nesting.

### Block-Block Binding

<block identifier> bound to <block literal>. The same principles for each block type applies as described in the two previous sections. The unique feature being that a block idenfities _data_ that is recognized as <block>.

## Emergence and compliance

Based in all things established till this point, an emergent feature, as well as an aspect of compliance with previously defined structure, can be ovserved.

### Nesting

Because, by definition, a literal can be a block of data that can contain one or several other bindings, the fundamental structure allows for recursion and thus nesting of bindings.

Nesting of entries is a feature possible because of the _binding structure_. One entry can be contained within another, and so on.

It is the <literal> part of the `<identifier> <introducer> <literal>` that holds nested entries. The question is if a block identifiers can contain nested entries? An identifier can a contain nested structures, but those structures will never be analyzed as binding structures, but only taken line by line and will be seen as either _<raw>_ or as _strings_ or _primitives_ of _<array>_. A _<block literal>_ on the other hand, has its content analyzed for the purpose of fetching nested data. In the case _<block identifier>_, on the previous hand, data is never fetched from an identifier, only compared. Thus, nesting serves no purpose in identifiers.

_<identifier>_ and _<literal>_ serves different purposes. The identifier is used merely for identification of literal, while the literal is the data itself that can be of a type that can contain other entries meaningful for a parser that may be looking for one entry within another, but the parser will never look within an identifier for the sake of finding a part of it. The parser always uses the whole identifier in matching against a target given identifier for sake of determining if the current entry is desired for futher consideration.

If an identifier can have nested structures that are meaningful as such, it technically could have another entry with another nested identifier and so on for eternal recursion.

### Note on _<inline terminator definition>_

The _<inline terminator definition>_ is recognized by the same principle as any other _binding structure_. An _<inline terminator definition>_ always follows the format of `<identifier> <introducer> <literal>`. Thus, identifier has its ordinary place to the left, and to the right a literal is found. Be aware that the terminator definition is for a block of data, and thus an <introducer> is added at the end: `<identifier> <introducer> <literal> <introducer>`, example: `id: term:`. In this example, the following lines comprise the <block literal> that is terminated, after those lines, by `term`.

As stated, the <inline terminator definition> is recognized by the principle of _binding structures_, but, then, not treated the same as, say, a <string> literal. The aquired literal in question, the terminator definition, is not used for retrieval in a query, unlike what a <string> literal would be used for, but for the need to know where the consequtive set of lines ends, so that that set of lines can be retrieved as the queried-for literal.

## Basic representations (semantic attribution)

Out of all characters of the _pre-fundamental characters_, certain ones will be given special meaning. The selected characters will be the concrete representations of the conceptual tokens outlined earlier, and will be the most fundamental building blocks of the syntax. Characters, that otherwise can represent anything, are given the general meaning of being tokens, where each token has a specific uniquely defined meaning.

Thus, this section outlines the basic concrete representation as linked with the conceptual tokens, making up the syntax.

### Ghostspace and whitespace

All _ghostspace_ is trimmed and ignored. Only what is defined by each _conceptual element_ and thus instantiated by the correpsonding _representational elements_ matters to the parser.

Before and after any _representational element_, there can be any or no amount of ghostspace.

In other words: whitespace in syntax (called _ghostspace_) does not affect the meaning of the syntax or the data (but within the boundary of a <literal> whitespace is kept).

These are the same `id : 123` and `id:123` and `id: 123` and `  id :123`.

### Basic standalone representations

All syntax depends on these.

| Token         | Conceptual label | Description                                            |
| ------------- | ---------------- | ------------------------------------------------------ |
| `:`           | <introducer>     | Always precedes something being defined.               |
| `.`           | <implicator>     | Denotes an implicit value in place of an explicit one. |
| `"`           | <delimiter>      | Always delimits single-line data.                      |
| `\n` or `EOF` | <segmenter>      | Separates elements and parts within an element.        |

> <!NOTE>
>
> The `segmenter`, `\n` or `EOF`, refers strictly to syntax and is part of the syntax. It does not refer to the data contained by the syntax, like the data of a multi-line literal.

In case of `EOF`, it is used to indicate the end of the file, and is considered the same as `\n` in the context of the syntax.

#### Rationale for the choice of dot as implicator

The original term was _finalizer_, but changed to _implicator_ as its function extends from defining the end of a thing to also be able to imply a thing. Thus, the _implicator_ implies _implicit identifier_, _default terminator definition_, it implies data of type _raw_, and it implies the end of multiline data. In its most general sense, it implies that it implies something in a specific context, and in the specific context, in turn, it implies a specific thing. The implicator not only implies a thing, but implies that it implies different things in different contexts.

The colon was choosen as it improves visual clarity and conveys the meaning of something following that which was before, and implying a relationship between the follower and the followed. Initally `:` was intended for ~<finalizer>~ <introducer> to keep the representations mininmal, but would conflict with the same representation used in other context (nested objects for example, we can not determine what ends what.) The `.` also adds a visual distinction, contradicting, in sense, the `:`. The discrepancy between `:` and `.` aligns with their essence of opening `:` and `.` closing. Colon (`:`), having two dots, implies a relatoin between two things as one have to relate to the other merely by exising in the same space. It aligns with the essence of _binding structures_ which is one thing (identifier) relating (bound) to another (a literal). Following up with a dot (`.`) consequently marks the end of a space in which things stand in relation to eachother, as a dot, by its visual content as a character, convays a singular thing. The dot, in _regex_ also symbolizes _any character_ and when we end with a dot, the parser accepts any character, looking for structure again. Thus, `.` symbolizes the end of structure and \_from now on, anything follows, till new structure is recognized. All structure tus involves the `:`.

Conceptually, following the above reasoning, it can be argued that; `.`, `\n` and `EOF` are all, share the functoin of being finalizers, of finalizing data, and logically we should be able to do the following all on the same line: `user: name. pwd: mypass. host: localhost.`

With this in mind, why can we not put `.` in the same category as `\n` and `EOF`? Conceptually, `.` is based in a higher level concept, meant to respond to <block> which is higher than <inline>. Enough are _character_, _sequence_ and _line_ to define an inline syntax, and still knowing how to end it, without introducing any new concept. `\n` and `EOF` are fundamental representations used to define the span of the inline data. <block>, on the other hand, is a higher level construct, in the sense of building on several lines, where the `\n` thus can not uniquely represent the ending of the block, or we would not be able to include the next line of the multi-line data intended to be stored. Thus, a construct at the same level of conceptual emergence, needs to be in place. For this, we have the <inline terminator definition> and <inline terminator expression>. The `.` represents a default <terminator definition>, and if used like `..` or `.myterminator` it means to take the content as <raw>, where the first dot is <literalizer>, indicating the content does not stand in relation to any oppinion of the format, but is taken as it is (not trimmed etc).

In summary, <segmenter> and <finalizer> (whoes functionality is absorbed by <implicator>) exist at different conceptual levels. Using `.` for inline syntax, as in `user: name. pwd: mypass. host: localhost.` would not, as first thought, make effective use of an already existing representation of a higher level (block), but would spawn a new representation for the same essence of putting an end to a thing, but at a lower level (inline). It would add redundancy of an additional representation, having `\n`, `EOF` and `.` serving the same purpose, for the illusion of conceptual and representational coherence, ignoring the reality of conceptually different levels. It may seem conceptually coherent that the `.` at the higher level of <block> should work to also terminate <inline>, if desired, but it is not conceptually coherent, as the `.` would transgress its conceptual domain, trying to solve a lower level thing with a hihger level construct, that in itself was made first to solve the higher level structure that was built on the very lower level things. Conceptually, it would be similar having drawers deliniated by samller peices of wood, and drawers together are collected in a cabinet, which is deliniated with bigger pieces of wood, and then expecting to deliniate a drawer with the big wodden surface of the top of the cabinet. You would have drawers with the size of the cabinet surface, obviously being antagonostic towards the cabinette that then can not contain them.

### Basic composite representations

Representations of composite tokens are built using the `basic standalone representations`, such that changing a standalone token would change the related composite token.

| Token | Name                  | Description                                                         |
| ----- | --------------------- | ------------------------------------------------------------------- |
| `::`  | Meta level introducer | The same meaning as _introducer_ but a higher level of abstraction. |

> <!IMPORTANT>
>
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

<whitespace> is data that may or may not have meaning. As part of the data of a literal, whitespace retains its POSIX definition. In the case it occurs outside a literal's span, it lacks meaning, as no part of the syntax is dependent on whitespace, except in case of `\n`. `\n` has meaning to the syntax of being a <segmenter>. As a <segmenter>, `\n` is not considered whitespace, but considered a representation of a token. A reason to not term it _whitespace_ is because the representation of <segmenter>, can by definition of this format, be redefined to something else.

Whitespace as part of a literal, is having meaning, as it is part of the data. Whitespace outside a literal is considered _nothing_, as it is not part of anything the format regognizes as meaningful. Similarly to comments appearing to have meaning to human perception, but being inherently empty of meaning to the format, and thus called _ghost syntax_, the equally meaningless whitespace, indentation, may convay meaning to human perception, but is meaningless to the configuration format, and thus is called **ghost-space**.

Ghostspace is not just the literal representations of white space (`\t`, ` ` etc...) but such occurence that is regarded as not meaningful to the structure or data of the configuration format, and consequently is ignored.

Ghostspace is any representation (character) regognized as whitespace that is not of _conceptual element_ (eg. <segmenter>, <string> etc...).

## Data (semantic attribution)

### Inline identifier

The acceptable character sequence is: Any character except `<delimiter>` (`"`), and `<segmenter>` (`\n`). Also that `<introducer>` (`:`) can not exist at the start or end of a sequence, and not occur twice or more directly following each other.

### Inline literal

A literal value is returned as-is by the parser, without any trailing newline characters.

As outlined conceptually, different types of literals exist: string, number, multi-line block etc... The exact type of literal is determined by first evaluating the surrounding syntax (context), and, if shown the type is not determined to match a certain set of types, then, as second, evaluation is preformed of the content contained by the syntax. It follows that, in evaluation of literal type, context has precedence over content. A string literal will have additional meaning, or not, depending of in what context it appears, and thus, context should be evaluated first.

Possible literals are:

| #   | Label                   | Literal  | Example         |
| --- | ----------------------- | -------- | --------------- |
| 1   | <string>                | String   | `"abc def"`     |
| 2   | <fragment>              | Fragment | `ghi a123`      |
| 3   | <number>                | Number   | `32, 3.14, -10` |
| 4   | <bool>                  | Bool     | `true, on, yes` |
| 5   | <null>                  | Null     | `null, nil`     |
| 6   | <terminator definition> | <1-5>    | <1-5>           |
| 7   | <terminator expression> | <1-5>    | <1-5>           |

Literal 6-7 are used to determine the end of block. They can be any of the previous 1-5.

All primitive literals (number, bool, null) are case-insensitive.

The data of all literals, except _<string>_, is considered to be the sequence that is with and from the first non-whitespace character to and with the last non-whitespace character. In case of <string> the data is considered that which is delinieated by the <delimiter>.

Inline literals are always trimmed of whitespace, because whitespace is not part of the data (in case of string: it is trimmed outside its delimiter if a delimiter is given).

#### String

A sequence of characters or no characters deliniated by _<delimiter>_ (`"`) such that `<delimiter> (...) <delimiter>`. The surrounding _<delimiter>_ is not part of the literal's data. `"abc"` is `abc` but `""abc""` is `"abc"`, meaning, only the uttermost surrounding _<delimiter>_ is taken as marks of the data's boundary.

The acceptable character sequence is: Any binary and non-binary character except _<segmenter>_ (`\n`) as it would contradict the essence of _inline_ in _<inline literal>_.

The delimiters must be around the data, or it will be seen as part of the data. `id: "data"` gives data `data`, while `id: this "is" my data` gives as data `this "is" my data`. Works the same in all contexts where <string> can occur: <terminator definition>, <terminator expression>, <literal> with <explicit identifier> and with <implicit identifier>.

> <!NOTE> > `<string>` is trimmed left and right, but is of course not trimmed within its two `<delimiter>`, e.g. within `"   this spaced string   "`.

#### Fragment

A sequence of characters or no characters, without surrounding _<delimiter>_, with leading and trailing whitespace is trimmed.

The acceptable character sequence is: Any binary and non-binary character except, but can not start with _<delimiter>_ (`"`), or it will be an _<explicit string>_.

Any unquoted literal that is not a _<number>_, _<bool>_ or _<null>_, is treated as _<fragment>_. In other words, If not a _<string>_ and not a primitive literal, then it is treated as a _<fragment>_. Thus, `&#38;`, for example, is treated as a _<fragment>_.

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

The _maybe_ should return true or false randomly by anything that parses the format. If serialized to another format, it may either be resolved into true or false, or it may be treated as a _<string>_ of the sequence `maybe`.

#### Null

The acceptable character sequence is as inferred from the example in the table above.

#### Terminator definition

The literal _<terminator definition>_ is a type that is used to define the end of a block of data. A terminator definition is always a _literal_ type of _inline_ format. The user defines a sequence of characters that is used as the terminator for the block.

Can be a _<string>_ or a _<fragment>_, and the accepted character sequence is the same as for those.

#### Terminator expression

While a _<terminator definition>_ defines the terminator for a block, a _<terminator expression>_ is the actual use of the terminator to end the corresponding block.

### Block identifier

Block identifier is parsed and compared to user given identifier.

The _<object>_ is considered serialized, as the parser never parses it as an object, but only line by line as any other data. Thus, there is no use in having an object in a block identifier, because it is never treated as an object. The reason is a _<block identifier>_ is used to identify a _<literal>_ and having literals with identifiers inside an identifier serves no purpose as literals are never queried for inside identifiers.

The parser does not assign zero-based index value based on position to the elements of a <block identifier> because no identifier is needed, as literals inside a <block identifier> are never queried for. Thus, no _implicit identifier_ exists for the literals.

### Block literal

Block literals are retrieved if the identifier matches the user given identifier.

In case of <block literal> being <array>, each contained <inline literal> has an _implicit identifier_, meaning the literals are automatically assigned an zero-based index value based on the identifiers position in the array.

An empty <block literal> that is not _<raw>_ is treated as empty array. If it is not <raw> it must be <object> or <array>, and in case it is empty, it is treated as an empty <array> and not empty <object>.

> <!NOTE>
>
> Block literals were introduced to avoid having to encode the data, as for example is commonly done with with base64 when a format's literal type can only store certain characters.

In case of _<array>_, each line is considered _<inline literal>_ , meaning it can be any of _value_, being _<string>_, _<number>_, etc..., (but of course, the markers _<terminator definition>_ and _<terminator expression>_ are not considered part of the arrays content).

If the empty line exists within an <array>; then it is treated as an <empty> <inline literal>.

A <block literal> of type <raw> is returned as-is, with all newline characters (and whitespace) included and none added to the output. In case of the content of <array> being returned, every line, including the last one, should end with a newline character, and only one. An empty line is seen as a literal with value <empty> and is returned as just a newline `\n` and nothing else. For this reason, it is important all lines of an <array> ends with one and only one '\n`. In case of content of <object> being returned, at least one <segmenter> ('\n') must follow each _binding structure_ that is contained, as to separate them.More <segmenters> can be added by the parser, but will not make a difference to the meaning of the content.

## Binding structures (conceptual definitions)

What follows is how different _binding structures_ are recognized.

**A note on _<inline terminator definition>_**: The _<inline terminator definition>_ is recognized by the same principle as any other _binding structure_. An _<inline terminator definition>_ always follows the format of `<identifier> <introducer> <literal>`. Thus, identifier has its ordinary place to the left, and to the right a literal is found. Be aware that the terminator definition is for a block of data, and thus an <introducer> is added at the end: `<identifier> <introducer> <literal> <introducer>`, example: `id: term:`.

## Inline deliniation syntax (semantic attribution)

The _inline_ syntax centers around _<introducer>_ (`:`) such that `<identifier> <introducer> <literal> <segmenter>` gives, for example, `id: my literal` and `<identifier> <meta level introducer> <literal> <segmenter>` gives, for example, `id:: my literal`.

1. An <inline identifier> can consist of any character, including whitespace, except double quotes (`"`), colon (`:`), and newline (`\n`),
2. except it CAN contain colon (`:`) in middle, like `k:e:y` but not in start or end as in `:key:`, and it can not contain two colons subsequently as in `abc::def` (in that case, `::` will be seen as operator syntax).
3. It cannot start with commenting syntax: `--` and `/-`.
4. An <inline identifier> must contain at least one character, which must be a none non-whitespace character and not any of the exceptions in (1).
5. An <inline identifier> starts and ends with a permissible non-whitespace character.
6. Whitespace within the <inline identifier> (between starting and ending non-whitespace characters) is allowed and considered part of the identifier exactly as it occurs. Example: `k e y` is a valid key and is different from `k  e  y`.
7. Leading and trailing whitespace is not considered part of the <inline identifier> and is ignored.

## Block deliniation syntax (semantic attribution)

Unlike _inline_ data, for which the span can be easily determined by recognizing the _pre-fundamental_ structure of a _line_, _block_ data, on the other hand, spans multiple lines and requires higher level artifacts to define its boundaries, in order to allow the desired escapeless flexibility of the format. Those higher level artifacts are _<terminator definition>_ and _<terminator expression>_, as defined earlier. With these, the structure of a block can be defined.

Like _inline_ syntax, _block_ syntax is also understood in its fullness in the _binding structures_ section, but requires additional things for its syntax to be possible.

Here the _basic tokens_ are given unique meaning, in the context of _<block>_, to define the concept of _<block>_.

| Label                 | From basic token | Block syntax | Description                                              |
| --------------------- | ---------------- | ------------ | -------------------------------------------------------- |
| <identifier proxy>    | <introducer>     | `:`          | Introduces a block identifier. `::term:`                 |
| <identifier proxy>    | <introducer>     | `:`          | Introduces a block literal. `id:term:`                   |
| <literalizer>         | <implicator>     | `.`          | Makes block literal of type <raw>.                       |
| <default terminator>  | <implicator>     | `.`          | Default terminator for block literal or identifier.      |
| <implicit identifier> | <implicator>     | `.`          | Implies implicit identifier for <literal>. `.: my value` |

To terminate a block: _<terminator expression>_ is used, which is the replication of the _<terminator definition>_ where the block is desired to end.

The _<terminator expression>_ can occur in two ways: a) `<terminator expression>` on a line of its own at the end of the block data, and b) `<literalizer> <terminator expression>` after the intended end of the data, meaning it can occur at the same line as the last data (thus the data will not end with a newline, as it is taken literally as it occurs within the block span).

In case _a_, the block is treated as _<array>_ or _<object>_ depending on the content. In case _b_, the block is treated as _<raw>_.

Because a terminator definition is part of the syntax to define the boundary of a block, the terminator definition itself can not be a block. It would conceptually lead to endless recursion if every nested _block terminator definition_ in turn define a _block terminator definition_ to define the end of the parent _block terminator definition_.

## Inline-Inline Binding (iib) (semantic attribution)

Identifier and literal both exist within the same line, and are thus _<inline format>_.

### With explicit identifier

Concept: `<identifier> <introducer> <literal>`

Code: `icl`

Example: `fruit: apple`

### With implicit identifier

Concept: `<literal>`

Code: `l`

Example: `apple`

The above is suggested to be normalized to the following, which has the same meaning:

Concept: `<implicator> <introducer> <literal>`

Code: `xcl`

Example: `.: apple`

> <!NOTE>
>
> ~Using _<empty identifier>_ (`""`) syntax is not possible in _<inline identifier>_. It would have the same meaning as simply stating _<inline literal>_ which is normal for stating an element of _<array>_. It would simply be a literal without a _<explicit identifier>_. It can be argued, `"": banana` should have been possible for sake of consistency as we use `""::` for _<block literal>_ with empty identifier, but it is not allowed. 1. There is no use for it. 2. Our available tokens are limited to align with the goal of a minimal syntax as stated by the purpose. 3) "":"" would not be possible, as data is deliniated by the outermost `"` to allow for an escape free format, which also aligns with the purpose of the format. 4. Defining another syntax for _empty inline identifier_ is not possible, as other aspects of the format need their own use of tokens, which would conflict with the attempt to define another syntax for _<empty>_ for _<inline identifier>_.~

> <!NOTE>
>
> It is not said _without identifier_. It is said _with empty identifier_. It means it is empty of explicit identifier, but it will have an _implicit identifier_, which is the zero-based index of the entry in the parent entry.

To state _implicit identifier_ for _<inline literal>_, the _<empty identifier>_ is not used, unlike in the case of _<block literal>_. _implicit identifiers_ are assigned automatically for <inline literal> by just stating <inline literal>.

### With explicit identifier and empty literal

Concept: `<identifier> <introducer>`

Code: `ic`

Example: `fruit:`

Literal is assumed to be _<empty>_. It is the same as:

Code: `ice`

Example: `fruit: ""

Note: the parser is adviced to normalize `ic` to `ice` before parsing.

### With implicit identifier and empty literal

Concept: `<implicator> <introducer>`

Code: `xce`

Example: `.:`

## Inline-Block Binding (ibb)

Identifier exists within the same line, but the literal is _<block format>_ meaning the data of the literal spans multiple subsequent lines.

### With explicit identifier with custom terminator definition

Concept: `<identifier> <introducer> <literal> <introducer>`

Code: `iclc`

Example:

```
id: "vt":
    ml-data1
    ml-data2
vt
```

### With explicit identifier with default terminator definition

Concept: `<identifier> <introducer> <introducer>`

Code: `icc`

Example:

```
id::
    val1
    val2
.
```

### With implicit identifier with default terminator definition

To state _implicit identifier_ in _<inline identifier>_, the _<empty identifier>_ is used.

Concept: `<empty identifier> <introducer> <introducer>`

Code: `xcc`

Example:

```
.::
    val1
    val2
.
```

### With implicit identifier with custom terminator definition

Concept: `<empty identifier> <introducer> <literal> <introducer>`

Code: `xclc`

Example:

```
.:term:
    val1
    val2
term
```

## Block identifiers generally

The _<block identifier>_ is the same in case of both _<inline literal>_ and _<block literal>_.

There are the possible _<block identifier>_, but without the literal part, as the literal part is described in subsequent sections.

The identifier is always without identifier, as the identifier is the identifier for the subsequent literal. An additional initial <introducer> represents the start of the _<block identifier>_ structure.

### With default terminator definition for block identifier

Concept: `<introducer> <introducer> <introducer>`

Code: `ccc`

Example:

```
:::
    val1
    val2
.
```

Thus, the case `:::` is understood as: `<identifier proxy> <assignment> (terminator definition) <assignment>`. With the optional terminator definition, it would be `::term:`. This is differnt from operator syntax, which is also `::`, but requires text before the `::`. The operator syntax is considered a token in itself (the `::`), whereas the `identifier proxy` syntax consists of two tokens: `identifier proxy` and `assignment`. Thus, locically in the latter case, whitespace can exist between as in ` : :`, because the first `:` is in place of an `identifier` which naturally can be surrounded by whitespace.

### With custom terminator definition for block identifier

Same as previous case, but with a custom terminator definition for the literal.

Concept: `<introducer> <introducer> <literal> <introducer>`

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

With _<block identifier>_ both _block-block binding_ (bbb) and _block-inline binding_ (bib) are possible.

Following the _<block identifier>_ is the literal, which then is specified on a line below the _<block identifier>_, and then, without any _<inline identifier>_, as the previously defined _<block identifier>_ is used as the identifier for the literal.

Thus, the literals, both _<block literal>_ and _<inline literal>_, are defined in the same way as previously described, but without any _<inline identifier>_.

### Block-Inline Binding

Concept: `<introducer> <literal>`

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

Concept: `<introducer> <introducer>`

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

Concept: `<introducer> <literal> <introducer>`

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

A directive is a command that is used to control the parser. A the structure of a directive is based on the highest organizing structure of the format, the _binding structure_. From this, as is the case for any other binding, a directive involves a kind of identifier bound to a kind of literal. The syntax in its most general form is `<field> <meta level introducer> <field>`.

To define the directive syntax, the _<operator>_ is introduced. The _<operator>_ is a _<meta level introducer>_ gaining a certain function and purpose.

| Label      | From basic token        | Block syntax | Description                |
| ---------- | ----------------------- | ------------ | -------------------------- |
| <operator> | <meta level introducer> | `::`         | Used to define directives. |

With the _<operator>_, the two surrounding _<field>_ are then defined as follows:

| type            | Allowed characters                                                        | Description            |
| --------------- | ------------------------------------------------------------------------- | ---------------------- |
| <directive>     | `a-z0-9_-`                                                                | Name of the directive. |
| <operator data> | Any possible binary and non-binary character except `<segmenter>` (`\n`). | Data associated.       |

With the above, the directive syntax is defined as: `<directive> <operator> <operator data>`.

Use: `<directive> <operator> (...)`

Example: `file::/home/user/config.f4c`

### x Defining types

For type validation etc,

`type::date "regex"`
`date::key: 1999-12-31`

### `Operator directive`

#### Template directives

| Label     | Directive | Operator | Data             | Description               |
| --------- | --------- | -------- | ---------------- | ------------------------- |
| <file>    | `file`    | ::       | file path        | File to include.          |
| <declare> | `declare` | ::       | sl or ml literal | Declare a block.          |
| <include> | `include` | ::       | `name`           | Include a declared block. |

#### Parser controling directives

| Label    | Directive | Data             | Description        |
| -------- | --------- | ---------------- | ------------------ |
| <parser> | `parser`  | <sleep \| awake> | Modifying parsing. |
| <print>  | `print`   | (...)            | Print mode.        |

#### Token configuration directives

The following must be single character.

| Label             | Directive | Default | Description               |
| ----------------- | --------- | ------- | ------------------------- |
| `<tb-delimiter>`  | `tbd`     | `"`     | Basic token _delimiter_.  |
| `<tb-introducer>` | `tbi`     | `:`     | Basic token _introducer_. |
| `<tb-implicator>` | `tbf`     | `.`     | Basic token _implicator_. |
| `<tb-segmeenter>` | `tbs`     | `\n`    | Basic token _segmenter_.  |

The following can be one or more characters.

| Label        | Directive | Default | Description          |
| ------------ | --------- | ------- | -------------------- |
| `<tc-dash>`  | `tcd`     | `--`    | Comment dash.        |
| `<tc-start>` | `tcs`     | `/-`    | Comment block start. |
| `<tc-end>`   | `tce`     | `-/`    | Comment block end.   |

## Emergence

Addapt this text, having the concepts, we now can have as emergent feature the mapping to representations: ... At this current _pre-fundamental_ level, for the sake of elaboration, we are only concerned with the things (characters) and structure (sequence of characters and sequence of lines), which comprises the very existence of _a_ _something_, which is the perceived reality, our conditions, within which specific structures can be defined to have higher meaning that can only arise by combinations and relations among these _pre-fundamental things_. The possibility of such emergent higher meaning, based in patterns of lower level things, is what allows the definition of structures specifically meaningful for the purpose of this configuration format. Meaning is thus contingent on patterns that are built from these lower-level constituents.

With the constitutional _basic tokens_ along with the contextual constraint of several tokens being present in sequence, the sequence is read from left to right and analyzed token-wise. In the sequence of tokens, several distinct and meaningful structures emerge.

### Syntax as emerging from structures of tokens

A _sequence_ holds several tokens in sequence. Thus, patterns among tokens can be defied. The tokens and the patterns both form the syntax. Syntax is not any one particular token, but the combinations of tokens in such a way that we can discern where in the _sequence_ particular content, _data_, with particular meaning, occurs.

`var = 1 + 1`

In the above example, none of the parts in themselves are considered syntax. That is, because in isolation, or being rearranged, they can not serve the function of inferring _data_. Is `+` part of data or is it not, for example. The function of syntax is to imply content of some type meaning. Thus, syntax is that, whatever, by which content can be discerned in a _sequence_. In the above example, data is understood by the syntax to be the evaluated expression resulting in the sum 2 which doesn't have any direct representation on in the sequence, but nonetheless is understood to exist by virtue of the rules of the syntax. Thus, while syntax can delineate a part of a sequence and we can see it concretely represented, content can also be abstract. Data, as content, is thus an emergent feature of syntax, by the function of having rules for perception, and, as a result of such a function, resides as a consequence of the tokens, the patterns and the syntax. Tokens gives patterns, gives, syntax, causes data.

This configuration deals with only concrete data, not abstract.

## Parsing

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

TODO: Needs update.

For sake of implementing a parser, these codes are suggested, but must not be used.

| Code | Description        |
| ---- | ------------------ |
| i    | Identifier         |
| l    | Literal            |
| c    | introducer (colon) |
| e    | empty              |
| x    | implicator         |

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
| <comment>             | `--` | Comment marker for single-line comments. |
| <block comment start> | `/-` | Start of block comments.                 |
| <block comment end>   | `-/` | End of block comments.                   |

Block comment can start and end on same line. In case of block comments: Must not have anything other than whitespace before the starting marker and after the ending marker.

Line comment (`--`) must be on a line of its own. No data other than whitespace is allowed to precede it on the same line.

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

Uses <Variable Classification Format>(https://github.com/fmxsh/vcf).

### Note on supported characters

It is up to the specific implementation to determine which characters are supported. Implementations should strive to support as many characters as possible.

- A Bash-based implementation supports all binary and textual characters, except for the null byte (`\x00`), as Bash cannot handle data containing null bytes.
- A C-based implementation, however, can fully support null bytes (`\x00`), as well as all other possible byte values.

### Redefining syntax

An implementation can change the characters, the representations, the `basic standalone tokens` are connected with. `:` for <introducer> can be changed to `-` for example.

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

These already ocvur

##

**Empty**: _Empty_ is a thing, syntax, that is empty of content. `""` is empty. It is absence of content, contained within `"` and `"`.

**Nothing**: _Nothing_ is not even a thing that is empty. It is the absence of syntax.

# Stuff not fitting the new outlining

## Introduction

We start by defining semantics. Semantics are the particular units of meaning, or conceptual definitions, used to fully define this configuration format. Thus we first concern ourselves with the concepts and structures among concepts, and only later focus on how these concepts and structures map to actual characters and character sequences, the most concrete representations, to form the concrete syntax reflecting the formats purpose.

Following the intended approach, conceptual definitions are introduced for _basic tokens_ and _data_. These will yield higher emergent conceptual definitions, that are structures of meaning, called _binding structures_. These _binding structures_, providing the function of associating one field with another, that is; binding an identifier to a literal; represents the highest level of emergent conceptual property that is the complete conceptual response to the purpose of defining this particular configuration format. The reality of concrete representations, as compared to the space of conceptual thought, then undergoes a process of semantic attribution, where the conceptual definitions imbude the representations with conceptual structural meaning. Concepts are mapped to representations. Structureed meaning is determined in the concrete; syntactical meaning is given to to selected characters such as `:`, `.` and `"`), and with these, the concept of contained data of different forms can also be realized in the concrete.
