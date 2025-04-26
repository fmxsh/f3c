# Fine Format For Configuration (_f3c_)

Note of reasoning, in most recent plan: I put the semantic attribution of the emergent binding structures in identity, binding structs is the highest emergence, and makes sense semantically attribute it in identity, but basic tokens and their semantic attribution to representation i see as part of constitution, however, i wonder if the semantic attribution then actually doesnt fall under constraint, as the conceptual definition is constrainted to the representation... the constituting part can be seen to hold concepts only, and semantic is the constraining to representation of them, constitution gives emergence of binding structures concept, and constraint comes to inform identity about where the emergence lands as identity

//TODO: On empty literals, "" is empty string, not just any empty literal, how is that distinquished in the specification.
//TODO: if array has `"abc"` and `def` and queried for, it will return both, but does it strip the "" from the `abc`

> \<!NOTE>
>
> This project is under development. Do not expect finished order.

## Meta

### About

_f3c_ stands for _Fine Format For Configuration_ and is a configuration and data serialization format.

The subtle beauty of the format, which can easily be felt, but hardly explained, has here been brought from the ineffable, to the explicit, by rigorous and equally beautiful analysis by this ontological approach of excellent stringency.

### Table of contents

\<toc here>

### Structure of the document

This specification is based in the ontology of [_Structure of conceptual models_](https://github.com/fmxsh/socm-ontology) which is an ontology for conceptual modeling. This is not essential for understanding the specification, but it explains the format of the document and the ontological frame behind.

### How to read this document

The entirety of format can be understood by the _binding structures_ in the section _Identity_, because all conceptual understanding can be inferred from the examples of the concrete representations alone. Everything else are conceptual and representational details underpinning the _binding structures_.

### Intended audience

This document is intended for software developers or system administrators seeking to implement or just understand the _f3c_.

## Identity

The _concept of f3c_ is a concept of a configuration and data serialization format with the following properties:

- with minimal set of tokens, support as much functionality as possible,
- key-value pairs,
- nesting,
- escapeless - no escaping of characters,
- allow for data serialization,
- allow for custom type checking,
- simple to read and edit.

### Binding structures (conceptual definitions)

This section is the entry point for understanding the _f3c_. Everything else are details contributing to the culminating _binding structures_ of the _f3c_.

The _f3c_ essentially is the ability to bind an identifier on the left side, to a literal on the right side, such as `counter: 10`, where `counter` is the identifier, and `10` is the literal. To address both simple and complex use cases, a total of four different _binding structures_ are possible.

The different binding structures are:

- _inline-inline binding_ (iib)
- _inline-block binding_ (ibb)
- _block-inline binding_ (bbb)
- _block-block binding_ (bib)

Thus, as seen, both the identifier and the literal can be either inline (existing on a single line) or block (spanning multiple lines).

All of these are explained in the following sections (but with a slightly different categorization to avoid overlapping descriptions). This section reflects the functioning totality of the _f3c_, in semantically attributing the conceptual _binding structures_ concrete syntax---patterns of tokens---as discerned in the configuration data. This is where the emergent concepts of the _f3c_ is mapped to the concrete reality of a sequence of characters, and thus the identity of _f3c_ as a unique format is fully established and is evident.

### Inline-Inline Binding (iib) (semantic attribution)

Identifier and literal both exist within the same line, and are thus _\<inline format>_.

#### With explicit identifier

Concept: `\<identifier> \<introducer> \<literal>`

Code: `icl`

Example: `fruit: apple`

#### With implicit identifier

Concept: `\<literal>`

Code: `l`

Example: `apple`

Suggestion: Either, the above is suggested to be normalized to the following, or the following is normalized to the above. Both cases have the same meaning.

Concept: `\<implicator> \<introducer> \<literal>`

Code: `xcl`

Example: `.: apple`

##### Normalization

`xc` is normalized to `xcl` before parsing.

The parser should normalize as stated above, but also in case of implicit identifier with empty literal:

```
items::
    .:
    .:
.
```

The above should be normalized to:

```
items::
    .: ""
    .: ""
.
```

There is no thing in the format to mark an empty general literal. The only way is to make an empty string, which is a string, that is empty.

> \<!NOTE>
>
> ~Using _\<empty identifier>_ (`""`) syntax is not possible in _\<inline identifier>_. It would have the same meaning as simply stating _\<inline literal>_ which is normal for stating an element of _\<array>_. It would simply be a literal without a _\<explicit identifier>_. It can be argued, `"": banana` should have been possible for sake of consistency as we use `""::` for _\<block literal>_ with empty identifier, but it is not allowed. 1. There is no use for it. 2. Our available tokens are limited to align with the goal of a minimal syntax as stated by the purpose. 3) "":"" would not be possible, as data is deliniated by the outermost `"` to allow for an escape free format, which also aligns with the purpose of the format. 4. Defining another syntax for _empty inline identifier_ is not possible, as other aspects of the format need their own use of tokens, which would conflict with the attempt to define another syntax for _\<empty>_ for _\<inline identifier>_.~

> \<!NOTE>
>
> It is not said _without identifier_. It is said _with empty identifier_. It means it is empty of explicit identifier, but it will have an _implicit identifier_, which is the zero-based index of the entry in the parent entry.

To state _implicit identifier_ for _\<inline literal>_, the _\<empty identifier>_ is not used, unlike in the case of _\<block literal>_. _implicit identifiers_ are assigned automatically for \<inline literal> by just stating \<inline literal>.

#### With explicit identifier and empty literal

Concept: `\<identifier> \<introducer>`

Code: `ic`

Example: `fruit:`

Literal is assumed to be _\<empty>_. It is the same as:

Code: `ice`

Example: `fruit: ""

Note: the parser is adviced to normalize `ic` to `ice` before parsing.

#### With implicit identifier and empty literal

Concept: `\<implicator> \<introducer>`

Code: `xce`

Example: `.:`

### Inline-Block Binding (ibb)

Identifier exists within the same line, but the literal is _\<block format>_ meaning the data of the literal spans multiple subsequent lines.

#### With explicit identifier with custom terminator definition

Concept: `\<identifier> \<introducer> \<literal> \<introducer>`

Code: `iclc`

Example:

```

id: "vt":
ml-data1
ml-data2
vt

```

#### With explicit identifier with default terminator definition

Concept: `\<identifier> \<introducer> \<introducer>`

Code: `icc`

Example:

```

id::
val1
val2
.

```

#### With implicit identifier with default terminator definition

To state _implicit identifier_ in _\<inline identifier>_, the _\<empty identifier>_ is used.

Concept: `\<empty identifier> \<introducer> \<introducer>`

Code: `xcc`

Example:

```

.::
val1
val2
.

```

#### With implicit identifier with custom terminator definition

Concept: `\<empty identifier> \<introducer> \<literal> \<introducer>`

Code: `xclc`

Example:

```

.:term:
val1
val2
term

```

### Block identifiers generally

The _\<block identifier>_ is the same in case of both _\<inline literal>_ and _\<block literal>_.

There are the possible _\<block identifier>_, but without the literal part, as the literal part is described in subsequent sections.

The identifier is always without identifier, as the identifier is the identifier for the subsequent literal. An additional initial \<introducer> represents the start of the _\<block identifier>_ structure.

#### With default terminator definition for block identifier

Concept: `\<introducer> \<introducer> \<introducer>`

Code: `ccc`

Example:

```

:::
val1
val2
.

```

Thus, the case `:::` is understood as: `\<identifier proxy> \<assignment> (terminator definition) \<assignment>`. With the optional terminator definition, it would be `::term:`. This is differnt from operator syntax, which is also `::`, but requires text before the `::`. The operator syntax is considered a token in itself (the `::`), whereas the `identifier proxy` syntax consists of two tokens: `identifier proxy` and `assignment`. Thus, locically in the latter case, whitespace can exist between as in ` : :`, because the first `:` is in place of an `identifier` which naturally can be surrounded by whitespace.

#### With custom terminator definition for block identifier

Same as previous case, but with a custom terminator definition for the literal.

Concept: `\<introducer> \<introducer> \<literal> \<introducer>`

Code: `cclc`

Example:

```

::term:
    val1
    val2
term
::
    val3
    val4
.

```

### Literals following block identifiers

With _\<block identifier>_ both _block-block binding_ (bbb) and _block-inline binding_ (bib) are possible.

Following the _\<block identifier>_ is the literal, which then is specified on a line below the _\<block identifier>_, and then, without any _\<inline identifier>_, as the previously defined _\<block identifier>_ is used as the identifier for the literal.

Thus, the literals, both _\<block literal>_ and _\<inline literal>_, are defined in the same way as previously described, but without any _\<inline identifier>_.

#### Block-Inline Binding

Concept: `\<introducer> \<literal>`

Code: `cl`

Example:

```

:::
val1
val2
.
: val3

```

#### Block-Block Binding with default terminator definition for identifier and literal

Concept: `\<introducer> \<introducer>`

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

#### Block-Block binding with custom terminator definition for identifier and literal

Concept: `\<introducer> \<literal> \<introducer>`

Code: `clc`

Example (both or either can have custom terminator, and terminator can be differently defined for both identifier and literal):

```
::term:
val1
val2
term
:term:
val3
val4
term

```

### Directive syntax (semantic attribution)

Based on the same principle of _binding structures_, a _directive_ is defined, not with `:` as _introducer_, but with `::` as _meta introducer_.

A directive is a command that is used to control the parser. A the structure of a directive is based on the highest organizing structure of the format, the _binding structure_. From this, as is the case for any other binding, a directive involves a kind of identifier bound to a kind of literal. The syntax in its most general form is `\<field> \<meta introducer> \<field>`.

> [!NOTE]
> Directives do not work inside a block literal.

To define the directive syntax, the _\<operator>_ is introduced. The _\<operator>_ is a _\<meta introducer>_ gaining a certain function and purpose.

| Label       | From basic token   | Block syntax | Description                |
| ----------- | ------------------ | ------------ | -------------------------- |
| \<operator> | \<meta introducer> | `::`         | Used to define directives. |

With the _\<operator>_, the two surrounding _\<field>_ are then defined as follows:

| type             | Allowed characters                                                         | Description            |
| ---------------- | -------------------------------------------------------------------------- | ---------------------- |
| \<directive>     | `a-z0-9_-`                                                                 | Name of the directive. |
| \<operator data> | Any possible binary and non-binary character except `\<segmenter>` (`\n`). | Data associated.       |

With the above, the directive syntax is defined as: `\<directive> \<operator> \<operator data>`.

Use: `\<directive> \<operator> (...)`

Example: `file::/home/user/config.f4c`

#### x Defining types

NOTE: Type checking IS REMOVED. This feature is removed for now. The feature unecessarily complicates things and isn't that well of a fit conceptually.

TODO: Because its a streaming format, we would have difficulty type checking multi line literals, as we would have to read the whole literal first, but what if it has nested multi-line literals and we query for an identifier in the nested structure? We would then quit with the result before the parent can be type checked. Typechecking a nested structure is, however, not of practical use, but in princpile, this is bad design, because we possibly can create the mentioned situation by what the design allows for. Alternating between streaming and non-streaming is not an option.

An option is that the type checking is done not on the whole multi-line literal, but the same type checking is done on each line of the multi-line literal, as it is parsed.

```
date::tournaments::
    2021-01-09
    2022-01-12
    2023-02-01
.
```

Here, `date` type checking is preformed on each contained date.

Type checking would be limited to literals themselves. The syntax is never type checked.

```
valid_name::users::
    user1::
        name: a
        pass: 1
    .
.
```

Note for implementation: It must rely on something like an event system, accessing the read literal by event, because for example we have multi-line identifiers and thus we must be ready to evaluate the literal after having read several lines, not just the current one.

However, event system fails as we would need to read the whole literal to fire the event, and this is a streaming format, meaning we may quit before all is read if we find what is queried for.

For type validation etc,

`type::date "regex"`
`date::key: 1999-12-31`

#### `Operator directive`

##### Template directives

| Label      | Directive | Operator | Data             | Description               |
| ---------- | --------- | -------- | ---------------- | ------------------------- |
| \<file>    | `file`    | ::       | file path        | File to include.          |
| \<declare> | `declare` | ::       | sl or ml literal | Declare a block.          |
| \<include> | `include` | ::       | `name`           | Include a declared block. |

##### Parser controling directives

| Label     | Directive | Data              | Description        |
| --------- | --------- | ----------------- | ------------------ |
| \<parser> | `parser`  | \<sleep \| awake> | Modifying parsing. |
| \<print>  | `print`   | (...)             | Print mode.        |

##### Token configuration directives

The following must be single character.

| Label              | Directive | Default | Description               |
| ------------------ | --------- | ------- | ------------------------- |
| `\<tb-delimiter>`  | `tbd`     | `"`     | Basic token _delimiter_.  |
| `\<tb-introducer>` | `tbi`     | `:`     | Basic token _introducer_. |
| `\<tb-implicator>` | `tbf`     | `.`     | Basic token _implicator_. |
| `\<tb-segmeenter>` | `tbs`     | `\n`    | Basic token _segmenter_.  |

The following can be one or more characters.

| Label         | Directive | Default | Description          |
| ------------- | --------- | ------- | -------------------- |
| `\<tc-dash>`  | `tcd`     | `--`    | Comment dash.        |
| `\<tc-start>` | `tcs`     | `/-`    | Comment block start. |
| `\<tc-end>`   | `tce`     | `-/`    | Comment block end.   |

## Constitution

The _definition of concept and representation of f3c_ consists of several things.

- The _f3c_ consist of a concrete representational layer. It is that which the conceptual definitions are semantically attributed to. It means _f3c_ consists of a sequence of characters, which then is subject to semantic attribution, where certain concrete representations---characters---gain certain conceptual meaning as specific tokens. To exemplify with a stop-sign: The concept of a stop-sign (that a car has to stop) is semantically attributed to the concrete representation being the pysichal stop-sign made by certain materials and with specific placement.
- Thus, at its base, the _f3c_ consists of differentiated concrete representations given specific meaning and thus recognized as either _tokens_ or _data_.
- The _f3c_ consists of conceptual definitions of _basic tokens_ and _basic data_.
- The _f3c_ consists of definitions of how the conceptual definitions of _basic tokens_ and _basic data_ are semantically attributed to the concrete representations.
- The _f3c_ consists of a conceptual syntax is used to describe the conceptual definitions.
- The _f3c_ also consists of terminology used to describe the format.

### Conceptual syntax

_Conceptual syntax_ is used to denote concepts and express combinations of them to reflect meaningful structural relationships.

- A _conceptual element_ is denoted by `\<` and `>` like `\<identifier>` or `\<literal>`.
- An expression of _conceptual syntax_ may involve one or several _conceptual elements_ such as in `\<identifier> \<assignment> \<literal>`. The meaning here is that `identifier` is followed by `assignment`, which is followed by `literal`. In this case, the representation could, for example, be `distance: 10`.
- Spaces between the brackets are only used for readability and serve no other purpose. For example, `\<identifier> \<assignment> \<literal>` may translate to `key:value`, meaning that the whitespace in the conceptual expression does not imply whitespace in the possible representational counterpart.
- Grouping is possible with `(` and `)`. For example, `(\<identifier> \<assignment> \<literal>)` means that the whole expression is grouped together.
- Grouping is used to group elements of logical operators.
- Logical operators `&&` and `||` are possible. The example `( \<custom> || \<default terminator> )` means either `\<custom terminator>` or `\<default terminator>`.
- Nested groups can be expressed: `( ( \<a> && \<b> ) || ( \<x> || \<y> ) )`.
- `[]` means optional, so `\<a> [\<b>]` means that `\<a>` is required, but `\<b>` is optional.
- `...` is a placeholder meaning _subsequent content of relevant kind_, or it means _the rest of the expected syntax_.
- `||` and `&&` can be used within `\<>` too.

### Terminology

The term _data_ refers simply to a sequence of characters.

### Concept of basic tokens

This is the outlining of a concept. Thus, we are not interested in concrete representations that are the actual things representing the concepts, but only the meaning of the concept and the label referring to it. For this reason, anything at the representational level is ignored. A _sequence of characters_ is instead termed _a sequence of conceptual units_.

Tokens are conceptual units with special meaning.

The following are called _basic tokens_ and the most fundamental building blocks of the format. They are of two kinds: _standalone tokens_ and _composite tokens_.

#### Conceptual standalone tokens

All structural meaning of _f3c_ depends on these.

| Conceptual label | Description                                                             |
| ---------------- | ----------------------------------------------------------------------- |
| \<introducer>    | Always precedes something being defined.                                |
| \<implicator>    | Denotes an implicit value in place of an explicit one.                  |
| \<delimiter>     | Always delimits data that exists completely in a single single-segment. |
| \<segmenter>     | Separates elements and parts within an element.                         |

These tokens are defined purely at the conceptual level, describing abstract functions that enable structure. How they may or may not occur in syntax is not possible to know merely by these definitions. The terms are general enough allow for similar but different meaning in different syntactical structures.

#### Basic composite tokens

_Composite tokens_ are built using the `standalone tokens`, such that changing a standalone token would change the related composite token.

| Label              | Composition                        | Description                                               | Tip                             |
| ------------------ | ---------------------------------- | --------------------------------------------------------- | ------------------------------- |
| \<meta introducer> | \<introducer> \<introducer>        | Same as _introducer_ but a higher level of abstraction.   | Think a single lines of data.   |
| \<meta segmenter>  | \<[defailt] terminator expression> | Same as _segmenter_ but segments a multi-segment segment. | Think a multiple lines of data. |

Conceptually, the \<meta introducer> must be built using the \<introducer> as it shares the same essence, but at a meta level. Thus, it is not a new essence that is introduced, but the same essence at a higher level.

The two meta concepts \<meta introducer> and \<meta segmenter> are not related to each other in use. It is not that \<meta introducer> introduces and \<meta segmenter> segments that. Instead, \<meta segmenter> segments what is introduced by \<introducer> when it introduces something that spans multiple segments, whereas the ordinary \<segmenter> marks the end of what is just a single segment.

The \<meta segmenter> which is composed of \<[default] terminator expression>, is in turn composed of either \<implicator> or _conceptual data_ that has been given the meaning of \<meta segmenter>. The exact composition of \<meta segemnter> is determined by context.

### Conceptual data

In the sequence of conceptual units, patterns of _basic tokens_ are recognized to form meaningful structures. The meaningful structures are such that they delineate the sequence of otherwise undifferentiated conceptual units, and designates the content within the delineating structures as _conceptual data_. Thus, _conceptual data_ is inferred from _meaningful structure_. _Conceptual data_ together with the _meaningful structure_ constitutes the _f3c_ at the conceptual level.

#### Conceptual fields

A _conceptual field_ is the general term for any given _conceptual data_.

In the sequence conceptual units, the presence of a field can be discerned by locating the token _\<introducer>_ or _\<meta introducer>_, in conjunction with a \<segmenter> or \<meta segmenter>.

A field is at either side of the _\<introducer>_ or _\<meta introducer>_. A _side_ spans all the way to a \<[meta] segmenter> in either direction depending on what side is considered. A field thus is the _conceptual data_ between an \<introducer> and a \<[meta] segmenter>. All the _conceptual data_ within the delineation is the content of the field. The field can also be empty if there are no conceptual units within its delineation.

A field always have two aspects: _data format_ and _data type_. The aspect of _data format_ can be either _inline_ or _block_, and the second aspect of _data type_ can be either _identifier_ or _literal_. The two aspects together yields four possible combinations: _inline identifier_, _inline literal_, _block identifier_, _block literal_. These are four different _data constructs_.

A _field_'s structural identity is a _data construct_, which is a term to refer to any of the four possible combination of _data format_ and _data type_.

Properties:

- A field has two required properties:
  - Data Format: Defines how the field is delineated (inline / block).
  - Data Type: Defines the field’s functional role (identifier / literal).

Nesting is possible: a field may contain other fields, depending on its construct.

A general rule applying to all _fields_ is that nothing of the _conceptual data_ ever be interpreted as a _basic tokens_, and consequently, no conceptual units are ever escaped.

##### Data formats

Recognition: By the way the field is delineated.

_Inline deliniation_ is where the data is deliniated to start and end within the same segment.

_Block deliniation_ is where the data is deliniated to start and end on at different segments.

Modes: All types of data fall into either of the two categories of data format: _inline_ or _block_.

| Label       | Delineation       | Description                    |
| ----------- | ----------------- | ------------------------------ |
| `\<inline>` | Single segment    | Indicating inline data format. |
| `\<block>`  | Multiple segments | Indicating block data format.  |

##### Data types

Recognition: By the function of the data.

Data can be either of two categories of data types: _identifier_ or _literal_.

| Conceptual label | Function    | Description                                 |
| ---------------- | ----------- | ------------------------------------------- |
| \<identifier>    | Identifying | A name used to identify a specific literal. |
| \<literal>       | Storing     | The content associated with an identifier.  |

#### Data constructs (conceptual definitions)

Recognition: By the combination of _data format_ and _data type_.

| Conceptual label     | Description                                          |
| -------------------- | ---------------------------------------------------- |
| \<inline identifier> | An identifier that exists only inside a single line. |
| \<block identifier>  | An identifier that must be multi-line data.          |
| \<inline literal>    | A literal that exists only inside a single line.     |
| \<block literal>     | A literal that must be multi-line data.              |

The details of each are described in subsequent dedicated section.

##### Inline identifier

An _inline identifier_ can be implicit or explicit.

Represented by _\<inline identifier>_ is a sequence of conceptual units within a single segment, with the purpose of identifying something.

##### Inline literal

Represented by _\<inline literal>_ is a sequence of characters within a single segment, that is the _conceptual data_ associated with an implicit or explicit identifier.

Possible literals are:

| #   | Label                    | Category  | Quality    | Determined by | Literal type |
| --- | ------------------------ | --------- | ---------- | ------------- | ------------ |
| 1   | \<string>                | String    | Atomic     | context       | value        |
| 2   | \<fragment>              | String    | Non-atomic | content       | value        |
| 3   | \<number>                | Primitive | Atomic     | content       | value        |
| 4   | \<bool>                  | Primitive | Atomic     | content       | value        |
| 5   | \<null>                  | Primitive | Atomic     | content       | value        |
| 6   | \<terminator definition> |           |            | context       | marker       |
| 7   | \<terminator expression> |           |            | context       | marker       |

The _data_ of 1-5 are considered being _content_. _Content_ is delineated and known trough meaningful structure, but does not affect the meaningful structure itself.

The _data_ of 6-7 are considered _context_. They define data that then is part of the tokens determining the meaningful structure. They are dynamically created tokens.

The term _atomic_ is used, meaning the data is taken as given. Non-atomic data is a such that is not taken as given, but is processed in some way.

##### Block identifier

A _\<block identifier>_ refers to a sequence of conceptual units spaning multiple segments, with the purpose of identifying something.

Block identifiers can be of two types: `raw` `array` including, in a certain way, `object`.

| Label    | Description                                                                  |
| -------- | ---------------------------------------------------------------------------- |
| \<raw>   | Raw data exactly as it occurs, nothing is altered. Any character is allowed. |
| \<array> | One or several literals.                                                     |

In case of _\<raw>_, the data is considered as it is.

##### Block literal

A _\<block literal>_ is a sequence of conceptual units spaning multiple segemnts, that is the data associated with an identifier.

Block literals can be of three types: `raw` `array` and `object`.

| Label     | Description                                                                 |
| --------- | --------------------------------------------------------------------------- |
| \<raw>    | Raw data exactly as it occurs, nothing is altered. Any character is allowed |
| \<array>  | One or several literals.                                                    |
| \<object> | One or several identifier-literal pairs.                                    |

#### Meta bindings (conceptual definitions)

Using the \<meta introducer> it is possible to bind an \<identifier> to a \<literal> in the following way:

`\<identifier> \<meta introducer> \<literal>`

Everything after the \<meta introducer> is considered to be the \<literal>. This means anything, including ordinary bindings can occur after the \<meta introducer>. This opens for several possibilities.

- Pre-processing of the associated literal which may be an ordinary bindings. For example, type checking can be done.
- Post-processing of the literal, such as Base64 decoding.
- Directives to the parser. For example tell the parser to include a file.
- Even control flow statements. For example, return the literal only if another literal is set to a certain value.
- Dynamic literals, a literal that has its value set dynamically, where the parser makes a system call getting the data before the literal is returned to the querier.

Meta level bindings can also hold block literals, by stripping itself out and allowing the parser to parse the lines as usual, with the directive part of the parser capturing the block data.

#### Delineation of data (conceptual definitions)

Delineation of the _data_ of the two _data formats_ \<inline format> and \<block format> is possible by _tokens_ occurring in the patterns recognized by the structures outlined in the two following sections, and hereby given corresponding structural meaning.

##### Inline data

The \<inline format> centers around _\<introducer>_ such that:

`\<segmenter> \<identifier> \<introducer> \<literal> \<segmenter>`

and:

`\<segmenter> \<identifier> \<meta introducer> \<literal> \<segmenter>`

By knowing the concept of \<segmenter> together with the concept of the \<introducer>, it is known where one inline _field_ of data starts and ends. At the left side of the \<introducer> is the eventual \<inline identifier> and at the right side is the eventual \<inline literal>. It is said 'eventual' because either side can be a \<block>, but the point is that any inline data is known and delineated in the way described.

##### Block deliniation

The \<block format> has a start and an end in a multi-segment space. The start is either of _\<explicit inline identifier>_ or _\<implicit || explicit block identifier>_. In either case, an associated _\<terminator definition>_ is given or assumed, and the end, at a later line, is _\<terminator expression>_.

A block can be terminated in one of two ways:

| Type             | Description                                                                   |
| ---------------- | ----------------------------------------------------------------------------- |
| soft termination | `\<terminator expression>` on a line of its own at the end of the block data. |
| hard termination | `\<literalizer> \<terminator expression>` after the intended end of the data. |

A terminator definition, and consequently also a terminator expression, is always _inline_.

The _\<terminator definition>_ is defined at the start boundary of the block, where the _\<terminator expression>_ is used at the end boundary of the block, such that:

```

\<...> \<terminator definition>
\<block data>}
\<...> \<terminator expression>

```

The use of the two literal types _\<terminator definition>_ and _\<terminator expression>_ makes it clear they are entierly dependent on context (where and how they are used) to be identified as such literals and not fall back on other forms of literals. Also, the _\<inline data>_ of these two literal types is never evaluated to determine its type.

## Constraints

The _f3c_ can only be perceived to occur where there is a concrete representational layer of undifferentiated sequential units. This means: a sequence of undifferentiated characters. The _f3c_ is thus constrained to exist in such a place. It can not for example exist as a 3d-matrix, because the representational units of the 3d-matrix do not occur sequentially where one clearly follows another linearly.

The _f3c_ is constrained to the requirement that the sequence of units is processed, meaning the units are considered one by one individually.

The _f3c_ is constrained to the requirement that the sequence of units is processed from left to right.

The _f3c_ is limited to be a queryable format meaning that the format must be able to be queried by an application for specific data.

The _f3c_ is constrained to be a streaming format, meaning it can not require to reach a presumed end of the data before meaningfully parsing the content. This means that the format does not require building an internal model of all configuration data being given to it before parsing the data. Instead, the parser builds an internal model of what it has right in front of itself and up till its current position, and throws away everything of the prior that is not queried for.

No escaping.

Minimal set of tokens.

Must support nesting.

Must support custom type checking.

...

### Basic tokens

**Semantic attribution**

The _basic tokens_ are the characters representing the most fundamental elements of the syntax. All syntax of _f3c_ are based in these.

There are two kinds: _standalone tokens_ and _composite tokens_.

#### Standalone tokens

(Semantic attribution)

The entire concept of _f3c_, without exception, is built using these four _basic tokens_.

| Semantic label | Token | Description                                            |
| -------------- | ----- | ------------------------------------------------------ |
| \<introducer>  | `:`   | Always indicates something follows.                    |
| \<implicator>  | `.`   | Denotes an implicit value in place of an explicit one. |
| \<delimiter>   | `"`   | Always delimits single-line data.                      |
| \<segmenter>   | `\n`  | Separates elements and parts within an element.        |

These represent the basic units of the format, and each also connotes the very essence of their general meaning within the format.

The configuration format processes data segment by segment, meaning line by line.

In case of `EOF`, it is used to indicate the end of the file, and is considered the same as `\n` in the context of the syntax. Think of two files being streamed to the parser (such as over a websocket). The parser reads them the same regardless if the content is split over two files or is all in the same file.

> [!NOTE]
>
> The `segmenter`, `\n` or `EOF`, refers strictly to syntax and is part of the syntax. It does not refer to any occurence of `\n` in the data contained by the syntax, like the data of a multi-line literal. This is because data is never parsed to reflect syntactical concepts, but is just parsed as data regardless of content.

> [!IMPORTANT]
>
> The composite token `\<meta introducer>` is not the same as two individual \<introducer> in a row (`\<introducer> \<introducer>`), even if they may seem identical. \<metal level introducer> is considered its own indivisible unit.

#### Composite tokens

**Semantic attribution**

Composite tokens are built using the `standalone tokens`, such that changing a standalone token would change the related composite token.

| Token | Conceptual label   | Description                                                         |
| ----- | ------------------ | ------------------------------------------------------------------- |
| `::`  | \<meta introducer> | The same meaning as _introducer_ but a higher level of abstraction. |

> \<!IMPORTANT>
>
> The composite representation `::` is not the same as two individual `introducers` (`:`) in a row (`::`), but they may seem identical. For example: An identifier for a multi-line literal can be defined with a `terminator definition` as in `id:termdef:`, but if we desire to use the `default terminator` we leave it empty as in `id::`. In this case, by the very usage itself, we recognize we are dealing with two separate `introducer` representations and not one _meta introducer_. Its even more clear by the fact we can space out the former case like `id: :`. The composite token `::`, on the other hand, is a single token, and always appears as `::` without any whitespace possible between the two colons of the composite.

### Comments, whitespace and ghostspace

#### Comments

Note: The comments syntax does not have any conceptual counterpart. Comments are not structurally relevant to the format and does not contain any meaningful data. If comments had conceptual counterparts, it would imply they are seen as meaningful to the parser, but comments by essence are used to denote something meaningless and non-existent to the parser, meaning it is void of meaningful structure and should be ignored.

`--`
`/-`
`-/`

Nested block comments are not supported. A block comment is always terminated by the first occurrence of the terminator expression.

A block comment must start on a line with nothing or nothing but ghostspace before it.

A block comment must end on a line with nothing or nothing but ghostspace after it.

Comments are considered **ghost-tokens**, forming a **ghost-syntax**, because the comment syntax exist to human perception but does not exist in the parsed structure.

A comment token is a **token that is never interpreted as a token** in the sense of relating to data.

To human perception, it appears that the comment syntax relates to the comment-text itself—i.e., the comment syntax _contains_ the comment. However, the parser does not merely ignore the comment-text; it also ignores the comment syntax in the sense no structural awareness of the comment is kept. This means the parser skips the comment syntax and its data without any analysis or recording either.

In all other cases of syntax, the parser analyses the tokens, perceives structure, and assigns meaning to the content within the structure, whether it is empty or not. In the case of a comment, no meaning is assigned neither to the contained data nor to the comment syntax itself.

#### Ghostspace and whitespace

Whitespace is by the definition of POSIX. Whitespace is treated differently and termed differently depending on where it occurs in the syntax.

#### Ghostspace

_Ghostspace_ is whitespace surrounding the _basic tokens_ and _composite tokens_

Before and after any _basic token_ and _composite token_, there can be **any or no** amount of whitespace. This means that the syntax is not sensitive to whitespace.

In other words: whitespace in syntax does not affect the meaning of the syntax or the data.

These are the same `id : 123` and `id:123` and `id: 123` and `  id :123`.

#### Whitespace

The term _whitespace_ refers to whitespace that part of the content of an \<identifier> or a \<literal>.

An example of whitespace would be: `id: "string with    whitespace"`.

### Basic data

What about these:
| `\<inline format>` | Refers to the structure made up by the syntax, including the data it wraps. |
| `\<block format>` | Refers to the structure made up by the syntax, including the data it wraps. |
| `\<inline data>` | Refers to the data delineated by the syntax, excluding the syntax itself. |
| `\<block data>` | Refers to the data delineated by the syntax, excluding the syntax itself. |

**Semantic attribution**

Using the _basic tokens_, we can discern individual fields of data which are the basis of the assignment of literals to identifiers.

The concept is: `\<identifier> \<introducer> \<literal>` and an example of an representation would be `id: value`.

We have two types of data:

- identifier - a unique sequence of characters used to identify the associated literal.
- literal - the data associated with the identifier.

Both of these can be _inline_ or _block_, meaning they can exist on a single line or span multiple lines.

#### Inline identifier

**Semantic attribution**: The acceptable character sequence is: Any character except `\<delimiter>` (`"`), and `\<segmenter>` (`\n`). Also that `\<introducer>` (`:`) can not exist at the start or end of a sequence, and not occur twice or more directly following each other within the identifier.

#### Inline literal

TODO: Add this: They are syntactic markers, meaning they are literals that are being part of the syntax and serves a syntactical function.

**Semantic attribution**: A literal value is returned as-is by the parser, without any trailing newline characters.

As outlined conceptually, different types of literals exist: string, number, multi-line block etc... The exact type of literal is determined by first evaluating the surrounding syntax (context) to see if it is delineated by \<delimiter> (`"`). If it is, then we know it is a string. If no surrounding \<delimiter> exist, then a second evaluation is preformed of the content contained, for example determining `1` is a number. It follows that, in evaluation of literal type, context has precedence over content in evaluating its type.

Possible literals are:

| #   | Label                    | Literal  | Example         |
| --- | ------------------------ | -------- | --------------- |
| 1   | \<string>                | String   | `"abc def"`     |
| 2   | \<fragment>              | Fragment | `ghi a123`      |
| 3   | \<number>                | Number   | `32, 3.14, -10` |
| 4   | \<bool>                  | Bool     | `true, on, yes` |
| 5   | \<null>                  | Null     | `null, nil`     |
| 6   | \<terminator definition> | \<1-5>   | \<1-5>          |
| 7   | \<terminator expression> | \<1-5>   | \<1-5>          |

Literal 6-7 are used to determine the end of block. They can be any of the previous 1-5.

All primitive literals (number, bool, null) are case-insensitive. (TODO: state which are primitive)

The data of all literals, except _\<string>_, is considered to be the sequence that is with and from the first non-whitespace character to and with the last non-whitespace character. In case of \<string> the data is considered that which is delinieated by the \<delimiter>.

Inline literals are always trimmed of whitespace, because whitespace is not part of the data (in case of string: it is trimmed outside its delimiter if a delimiter is given, and in case of string, the \<delimiter> (`"`) itself is of course not part of the data but is excluded).

##### String

A sequence of characters or no characters deliniated by _\<delimiter>_ (`"`) such that `\<delimiter> (...) \<delimiter>`. The surrounding _\<delimiter>_ is not part of the literal's data. `"abc"` is `abc` but `""abc""` is `"abc"`, meaning, only the uttermost surrounding _\<delimiter>_ is taken as marks of the data's boundary.

The acceptable character sequence is: Any binary and non-binary character except _\<segmenter>_ (`\n`) as it would contradict the essence of _inline_ in _\<inline literal>_.

The delimiters must be around the data, or it will be seen as part of the data. `id: "data"` gives data `data`, while `id: this "is" my data` gives as data `this "is" my data`. Works the same in all contexts where \<string> can occur: \<terminator definition>, \<terminator expression>, \<literal> with \<explicit identifier> and with \<implicit identifier>.

> \<!NOTE> > `\<string>` is trimmed left and right, but is of course not trimmed within its two `\<delimiter>`, e.g. the spaces within `"   this spaced string   "` are kept.

##### Fragment

A sequence of characters or no characters, without surrounding _\<delimiter>_, with leading and trailing whitespace is trimmed.

A fragment must not be any of the other types: \<number>, \<bool>, \<null>, and can never be related to \<terminator definition || expression> because those are outside the syntactical scope of where \<fragment> is able to be recognized.

The acceptable character sequence is: Any binary and non-binary character except, but can not start with _\<delimiter>_ (`"`), or it will be an _\<explicit string>_.

Any unquoted literal that is not a _\<number>_, _\<bool>_ or _\<null>_, is treated as _\<fragment>_. In other words, If not a _\<string>_ and not a primitive literal, then it is treated as a _\<fragment>_. Thus, `&#38;`, for example, is treated as a _\<fragment>_.

##### Number

The acceptable character sequence reflecting a number as inferred by these examples: `-1, 10, 1.2, -1.2`.

##### Bool

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

The _maybe_ should return true or false randomly by anything that parses the format. If serialized to another format, it may either be resolved into true or false, or it may be treated as a _\<string>_ of the sequence `maybe`.

##### Null

The acceptable character sequence is as inferred from the example in the table above.

##### Terminator definition

The literal _\<terminator definition>_ is a type that is used to define the end of a block of data. A terminator definition is always a _literal_ type of _inline_ format. The user defines a sequence of characters that is used as the terminator for the block.

Can be a _\<string>_ or a _\<fragment>_, and the accepted character sequence is the same as for those.

**A note on _\<inline terminator definition>_**: The _\<inline terminator definition>_ is recognized by the same principle as any other _binding structure_. An _\<inline terminator definition>_ always follows the format of `\<identifier> \<introducer> \<literal>`. Thus, identifier has its ordinary place to the left, and to the right a literal is found. Be aware that the terminator definition is for a block of data, and thus an \<introducer> is added at the end: `\<identifier> \<introducer> \<literal> \<introducer>`, example: `id: term:`.

##### Terminator expression

While a _\<terminator definition>_ defines the terminator for a block, a _\<terminator expression>_ is the actual use of the terminator to end the corresponding block.

#### A note on inline fragments

\<number>, \<bool>, \<null> really are just \<fragment> as there is no arithmetic or evaluation in the format, but when serializing to other formats, they are recognized as by their type in order to be serialized correctly.

#### Block identifier

**Semantic attribution**

Block identifier is parsed and compared to user given identifier.

Data of a _block identifier_ is always considered to be a series of _array elements_ or data of type \<raw>. \<object> is never recognized inside a _block identifier_.

Inside the _block identifier_, the _\<object>_ is considered serialized, as the parser never parses it as an object, but only line by line as any other data. Thus, there is no use in having an object in a block identifier for the sake of its function as an object, because it is never treated as an object. The reason is that a _\<block identifier>_ is used to identify a _\<literal>_ and having literals with identifiers inside an identifier serves no purpose as literals are never queried for inside identifiers.

The parser does not assign zero-based index value based on position to the elements of a \<block identifier> because no identifier is needed, as literals inside a \<block identifier> are never queried for. Thus, no _implicit identifier_ exists for the literals.

#### Block literal

**Semantic attribution**

Block literals are retrieved if the associated identifier matches the user given identifier.

In case of \<block literal> being \<array>, each contained \<inline literal> has an _implicit identifier_, meaning the literals are automatically assigned an zero-based index value based on the identifiers position in the array.

An empty \<block literal> that is not _\<raw>_ is treated as empty array. If it is not \<raw> it must be \<object> or \<array>, and in case it is empty, it is treated as an empty \<array> and not empty \<object>.

> \<!NOTE>
>
> Block literals were introduced to avoid having to encode the data, as for example is commonly done with with base64 when a format's literal type can only store certain characters.

In case of _\<array>_, each line is considered _\<inline literal>_ , meaning it can be any of _value_, being _\<string>_, _\<number>_, etc..., (but of course, the markers _\<terminator definition>_ and _\<terminator expression>_ are not considered part of the arrays content).

If the empty line exists within an \<array>; then it is treated as an \<empty> \<inline literal>.

A \<block literal> of type \<raw> is returned as-is, with all newline characters (and whitespace) included and none added to the output. In case of the content of \<array> being returned, every line, including the last one, should end with a newline character, and only one. An empty line is seen as a literal with value \<empty> and is returned as just a newline `\n` and nothing else. For this reason, it is important all lines of an \<array> ends with one and only one '\n`. In case of content of \<object> being returned, at least one \<segmenter> ('\n') must follow each _binding structure_ that is contained, as to separate them.More \<segmenters> can be added by the parser, but will not make a difference to the meaning of the content.

#### Raw

The acceptable character sequence is: Any binary and non-binary character.

#### Array

An \<array> must consist of only one or more literals with _implicit identifiers_. Can not contain any _\<literal>_ with _explicit identifiers_. The literal, as long as having _implicit identifier_ can be \<inline literal> and \<block literal>, and an arrays content can be a mix of these two. The array does not concern itself with the type of literal, but with how it is identified, requiring the _implicit identifier_.

#### Object

An \<object> must consist of one or more _\<literal>_ with _explicit identifiers_. Can not contain any _\<literal>_ with _implicit identifiers_.

### Basic delineation syntax

#### Inline deliniation syntax (semantic attribution)

##### For identifier

The _inline_ syntax centers around _\<introducer>_ (`:`) such that `\<identifier> \<introducer> \<literal> \<segmenter>` gives, for example, `id: my literal` and `\<identifier> \<meta introducer> \<literal> \<segmenter>` gives, for example, `id:: my literal`.

1. An \<inline identifier> can consist of any character, including whitespace, except double quotes (`"`), colon (`:`), and newline (`\n`),
2. except it CAN contain colon (`:`) in middle, like `k:e:y` but not in start or end as in `:key:`, and it can not contain two colons subsequently as in `abc::def` (in that case, `::` will be seen as operator syntax).
3. It cannot start with commenting syntax: `--` and `/-`.
4. An \<inline identifier> must contain at least one character, which must be a none non-whitespace character and not any of the exceptions in (1).
5. An \<inline identifier> starts and ends with a permissible non-whitespace character.
6. Whitespace within the \<inline identifier> (between starting and ending non-whitespace characters) is allowed and considered part of the identifier exactly as it occurs. Example: `k e y` is a valid key and is different from `k  e  y`.
7. Leading and trailing whitespace is not considered part of the \<inline identifier> and is ignored.

##### For literal

Delineation of \<inline literal> is done by localizing data at the right side of the \<introducer>, and that data is delineated according to the rules of the _\<literal>_.

#### Block deliniation syntax (semantic attribution)

This applies to both _\<block identifier>_ and _\<block literal>_.

Unlike _inline_ data, for which the span can be easily determined by recognizing the end of the span, which is the end of the line found by the _basic token_ \<segmenter>, _block_ data, on the other hand, spans multiple lines and requires higher level artifacts to define its boundaries, in order to allow the desired escapeless flexibility of the format. Those higher level artifacts are _\<terminator definition>_ and _\<terminator expression>_, as defined in the conceptual section. With these, the span of a block can be delineated.

Like _inline_ syntax, _block_ syntax is also understood in its fullness in the _binding structures_ section, but requires additional artifacts for its syntax to be possible.

Here the _basic tokens_ are given unique meaning, in the context of _\<block>_, to define the span of a _\<block>_.

| Label                  | From basic token | Block syntax | Description                                               |
| ---------------------- | ---------------- | ------------ | --------------------------------------------------------- |
| \<identifier proxy>    | \<introducer>    | `:`          | Introduces a block identifier. `::term:`                  |
| \<literal proxy>       | \<introducer>    | `:`          | Introduces a block literal. `id:term:`                    |
| \<literalizer>         | \<implicator>    | `.`          | Makes block literal of type \<raw>.                       |
| \<default terminator>  | \<implicator>    | `.`          | Default terminator for block literal or identifier.       |
| \<implicit identifier> | \<implicator>    | `.`          | Implies implicit identifier for \<literal>. `.: my value` |

To terminate a block: _\<terminator expression>_ is used, which is the replication of the _\<terminator definition>_ and put where the block is desired to end.

The _\<terminator expression>_ can occur in two ways: a) `\<terminator expression>` on a line of its own at the end of the block data, and b) `\<literalizer> \<terminator expression>` after the intended end of the data, meaning it can occur at the same line as the last data (thus the data will not end with a newline, as it is taken literally as it occurs within the block span).

In case _a_, the block is treated as _\<array>_ or _\<object>_ depending on the content. In case _b_, the block is treated as _\<raw>_.

Because a terminator definition is part of the syntax to define the boundary of a block, the terminator definition itself can not be a block. It would conceptually lead to endless recursion if every nested _block terminator definition_ in turn define a _block terminator definition_ to define the end of the parent _block terminator definition_.

## Emergence

This is actually emergence...

- The _f3c_ thus naturally also consists of conceptual definitions that are assigned to complex concrete representations, which is are complex patterns of tokens (complex in the sense the tokens and placement form patterns).

### Binding structures (conceptual definitions)

Based in _basic tokens_ and _data_ the highest organizing structures are built. They are called _binding structure_.

#### General properties

A binding structure binds an _\<identifier>_ to a _\<literal>_ into an _\<entry>_ using _\<introducer>_. All entries must have an, at its level of nesting, unique identifier to separate it from other entries at the same level of nesting. There is no literal with identifier absent.

All structural meaning of the format arises from this basic concept of binding the two fields of _\<identifier>_ and _\<literal>_ together in that particular order. All syntax center around facilitating this binding. What a binding does is to bind an identity to a given set of data which is the literal, for example this valid syntax: `fruit: banana`. The identifier is `fruit` and the literal is `banana`.

#### Implicit and explicit identifiers

A binding structure always consist of a _\<identifier>_ and a _\<literal>_. The _\<identifier>_ can be either _implicit identifier_ or _explicit identifier_. In the first case, no identifier is specified alongside the data, for example, only this occurs on a line `The data`, and thus the implicit identity is the zero-based indexed of the literal'sposition in the parent field. An _explicit_ identifier is one that is specified alongside the data, for example `The key: The data`.

An _implicit identifier_ occurs when \<implicator> is specified as the identifier. The identifier is then implied and determined by the ordered sequence in which the fields occur, based in zero-based indexing. This applies for both \<inline literal> and \<block literal>.

In case of _explicit identifier_: a _sequence_ is specified as the identifier to associate with the literal.

### The four specific binding structures (conceptual definitions)

The _binding structure_ is the fundamental meaning of the format. By analyzing the content and recognizing certain patterns of _basic tokens_ and _data_, meaningful structures of binding _\<identifier>_ to _\<literal>_ are recognized. Any other content that is not recognized is considered to be _nothing_, which means the content is in the absence of fundamentally meaningful structure.

The following structures of binding are recognized:

| Shorthand | Full Name             | Identifier Type | Literal Type |
| --------- | --------------------- | --------------- | ------------ |
| \<iib>    | Inline-Inline Binding | Inline          | Inline       |
| \<ibb>    | Inline-Block Binding  | Inline          | Block        |
| \<bib>    | Block-Inline Binding  | Block           | Inline       |
| \<bbb>    | Block-Block Binding   | Block           | Block        |

An identifier is always to the left, and a literal is always to the right.

The _binding structure_ binds _\<identifier>_ to _\<literal>_, not purely for organization of data, but to support the function of querying and fetching data within the heirarchy of structures. It is at this level—where the binding structure is recognized—that the format attains functionality. Percieving _binding structure_ allows for querying and fetching.

#### Inline-Inline Binding

\<inline identifier> bound to \<inline literal>

#### Inline-Block Binding

\<inline identifier> bound to \<block literal>. A block literal can be \<raw>, \<array> or \<object>. Because it can be of \<object>, it allows for nesting, as an \<object> contains other bindings of identifier and literal.

#### Block-Inline Binding

\<block identifier> bound to \<inline literal>. The block identifier can be \<raw>, \<array> or serialized \<object>. A _serialized \<object>_ has the structure of an object (looks like an literal with identifier and content that consists of only identifier-literal pairs), but is never parsed as such, because there is no utility. \<object> are for querying the contained data, but no querying inside an identifier ever happens. We use identifiers to query associated data. We do not use identifiers to store data to be queried for. If we would query data inside identifiers, we could have another \<object> inside that, with data inside its identifier, and so on, for endless recursion and nesting.

#### Block-Block Binding

\<block identifier> bound to \<block literal>. The same principles for each block type applies as described in the two previous sections. The unique feature being that a block idenfities _data_ that is recognized as \<block>.

### Directive bindings

[conceptual description of directive bindings...]

### Emergence and compliance

Based in all things established till this point, an emergent feature, as well as an aspect of compliance with previously defined structure, can be ovserved.

#### Nesting

Because, by definition, a literal can be a block of data that can contain one or several other bindings, the fundamental structure allows for recursion and thus nesting of bindings.

Nesting of entries is a feature possible because of the _binding structure_. One entry can be contained within another, and so on.

It is the \<literal> part of the `\<identifier> \<introducer> \<literal>` that holds nested entries. The question is if a block identifiers can contain nested entries? An identifier can a contain nested structures, but those structures will never be analyzed as binding structures, but only taken line by line and will be seen as either _\<raw>_ or as _strings_ or _primitives_ of _\<array>_. A _\<block literal>_ on the other hand, has its content analyzed for the purpose of fetching nested data. In the case _\<block identifier>_, on the previous hand, data is never fetched from an identifier, only compared. Thus, nesting serves no purpose in identifiers.

_\<identifier>_ and _\<literal>_ serves different purposes. The identifier is used merely for identification of literal, while the literal is the data itself that can be of a type that can contain other entries meaningful for a parser that may be looking for one entry within another, but the parser will never look within an identifier for the sake of finding a part of it. The parser always uses the whole identifier in matching against a target given identifier for sake of determining if the current entry is desired for futher consideration.

If an identifier can have nested structures that are meaningful as such, it technically could have another entry with another nested identifier and so on for eternal recursion.

#### Note on _\<inline terminator definition>_

The _\<inline terminator definition>_ is recognized by the same principle as any other _binding structure_. An _\<inline terminator definition>_ always follows the format of `\<identifier> \<introducer> \<literal>`. Thus, identifier has its ordinary place to the left, and to the right a literal is found. Be aware that the terminator definition is for a block of data, and thus an \<introducer> is added at the end: `\<identifier> \<introducer> \<literal> \<introducer>`, example: `id: term:`. In this example, the following lines comprise the \<block literal> that is terminated, after those lines, by `term`.

As stated, the \<inline terminator definition> is recognized by the principle of _binding structures_, but, then, not treated the same as, say, a \<string> literal. The aquired literal in question, the terminator definition, is not used for retrieval in a query, unlike what a \<string> literal would be used for, but for the need to know where the consequtive set of lines ends, so that that set of lines can be retrieved as the queried-for literal.

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

| Label                  |      | Description                              |
| ---------------------- | ---- | ---------------------------------------- |
| \<comment>             | `--` | Comment marker for single-line comments. |
| \<block comment start> | `/-` | Start of block comments.                 |
| \<block comment end>   | `-/` | End of block comments.                   |

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

Uses \<Variable Classification Format>(https://github.com/fmxsh/vcf).

### Note on supported characters

It is up to the specific implementation to determine which characters are supported. Implementations should strive to support as many characters as possible.

- A Bash-based implementation supports all binary and textual characters, except for the null byte (`\x00`), as Bash cannot handle data containing null bytes.
- A C-based implementation, however, can fully support null bytes (`\x00`), as well as all other possible byte values.

### Redefining syntax

An implementation can change the characters, the representations, the `basic standalone tokens` are connected with. `:` for \<introducer> can be changed to `-` for example.

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

## Meta: reasoning and choices

### Rationale for the choice of dot as implicator

The original term was _finalizer_, but changed to _implicator_ as its function extends from defining the end of a thing to also be able to imply a thing. Thus, the _implicator_ implies _implicit identifier_, _default terminator definition_, it implies data of type _raw_, and it implies the end of multiline data. In its most general sense, it implies that it implies something in a specific context, and in the specific context, in turn, it implies a specific thing. The implicator not only implies a thing, but implies that it implies different things in different contexts.

The colon was choosen as it improves visual clarity and conveys the meaning of something following that which was before, and implying a relationship between the follower and the followed. Initally `:` was intended for ~\<finalizer>~ \<introducer> to keep the representations mininmal, but would conflict with the same representation used in other context (nested objects for example, we can not determine what ends what.) The `.` also adds a visual distinction, contradicting, in sense, the `:`. The discrepancy between `:` and `.` aligns with their essence of opening `:` and `.` closing. Colon (`:`), having two dots, implies a relatoin between two things as one have to relate to the other merely by exising in the same space. It aligns with the essence of _binding structures_ which is one thing (identifier) relating (bound) to another (a literal). Following up with a dot (`.`) consequently marks the end of a space in which things stand in relation to eachother, as a dot, by its visual content as a character, convays a singular thing. The dot, in _regex_ also symbolizes _any character_ and when we end with a dot, the parser accepts any character, looking for structure again. Thus, `.` symbolizes the end of structure and \_from now on, anything follows, till new structure is recognized. All structure tus involves the `:`.

Conceptually, following the above reasoning, it can be argued that; `.`, `\n` and `EOF` are all, share the functoin of being finalizers, of finalizing data, and logically we should be able to do the following all on the same line: `user: name. pwd: mypass. host: localhost.`

With this in mind, why can we not put `.` in the same category as `\n` and `EOF`? Conceptually, `.` is based in a higher level concept, meant to respond to \<block> which is higher than \<inline>. Enough are _character_, _sequence_ and _line_ to define an inline syntax, and still knowing how to end it, without introducing any new concept. `\n` and `EOF` are fundamental representations used to define the span of the inline data. \<block>, on the other hand, is a higher level construct, in the sense of building on several lines, where the `\n` thus can not uniquely represent the ending of the block, or we would not be able to include the next line of the multi-line data intended to be stored. Thus, a construct at the same level of conceptual emergence, needs to be in place. For this, we have the \<inline terminator definition> and \<inline terminator expression>. The `.` represents a default \<terminator definition>, and if used like `..` or `.myterminator` it means to take the content as \<raw>, where the first dot is \<literalizer>, indicating the content does not stand in relation to any oppinion of the format, but is taken as it is (not trimmed etc).

In summary, \<segmenter> and \<finalizer> (whoes functionality is absorbed by \<implicator>) exist at different conceptual levels. Using `.` for inline syntax, as in `user: name. pwd: mypass. host: localhost.` would not, as first thought, make effective use of an already existing representation of a higher level (block), but would spawn a new representation for the same essence of putting an end to a thing, but at a lower level (inline). It would add redundancy of an additional representation, having `\n`, `EOF` and `.` serving the same purpose, for the illusion of conceptual and representational coherence, ignoring the reality of conceptually different levels. It may seem conceptually coherent that the `.` at the higher level of \<block> should work to also terminate \<inline>, if desired, but it is not conceptually coherent, as the `.` would transgress its conceptual domain, trying to solve a lower level thing with a hihger level construct, that in itself was made first to solve the higher level structure that was built on the very lower level things. Conceptually, it would be similar having drawers deliniated by samller peices of wood, and drawers together are collected in a cabinet, which is deliniated with bigger pieces of wood, and then expecting to deliniate a drawer with the big wodden surface of the top of the cabinet. You would have drawers with the size of the cabinet surface, obviously being antagonostic towards the cabinette that then can not contain them.
