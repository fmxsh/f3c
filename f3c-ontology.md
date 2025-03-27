# f3c - Ontology

This document describes the ontological foundation for reasoning about the f3c.

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

### Emergence goes upwards

Clarify why a higher-level construct cannot transgress downwards.

Could specify that emergence in ontology goes upwards, where new levels are formed to solve higher-order structural constraints. (Based in reflection on the `.` as [finalizer] in _f3c_.

iConceptual minimalism by removing redundant representatoins... ontological minimalism demands each representation be uniquely necessary.

## More things...

Do not wrongfully make categories into things. A category holding 3 sub-cats should not lead to the assumption the three sub-cats create an emergent thing that would be the parent cat. Perhaps, as a rule, do not have overarching categories. Whats the purpose, other than wrongfully used convention for the sake of it, rather than as with structural relevance as reason.
The document should only consist of things, not categories that are not things.. Each heading represents a thing. What about sub,headings? Should then Conceptual definition and semantic attribution be _things_... as that is my way of categorizing... yes, they can be made into ontological things, ... I guess..

--
Recognize the conceptual casual chain...
For identity, describe by order of conceptual casual chain...

## A thing, format

Identity: A thing's identity is what the thing is not. Identity is a constitution of the rest or is there any reason for this category.

Each concept is a thing created by defining what it is, but without reference to a more abstract or concrete layer. An example of a reference to a more abstract layer is if _a[1,2]_ + _b[1,2]_ = _c[11,12,21,22]_, then we have 4 different _c_, and it would be tempting to group them under a general category _x_, which would then look like a general thing that can be any of the four, but there is no _general thing_, there are just these four _c_. Example: A _field_ has _data format_ and _data type_ and combined they create a set of _data constructs_. We could wrongfully assume those _data constructs_ are _fields_, but there _is_ no thing called _field_, only our own grouping of the four _data constructs_ which are _things_.

It also does not refer to representational layer; that which is semantically attributed to. That would be to refer to the more concrete level.

Identity starts with "X is..."

The _thing_ IS, by all its parts and properties, the culminating emergence. Without the category "identity", there is just emergence as structure, but no definitive clarification that that is what the thing _is_. "Identity" clearly states what of its constitution makes its total identity. Identity thus is the totality of the structure constituting the thing.

Properties:

If it is a description of structure within, it is a property.

Constitution:

If it is a thing, it can be a constitution of another thing.

we can say, when we rely on external conceptual units like _tokens_, its constitution, but when internal, its property. so _data format_ and _data type_ is property, because its a characteristic we see within the structure made by the constituting things. properties are thus not things. but defines a thing. Any _thing_ that is part of a thing is constitution

A property is not a thing, but something seen in the structure of a thing. A constituting element is itself a thing, and thus is part of the structure of the composed thing."

Emergence:
We treat emergence not as an extra feature of a thing, but as the thing itself.

## Constitution

Constituents:

## Property

Property 1:
Name: Example: data format.
Inference: Inferred/explicit property.
Recognition:
Modes: Example: X can be either true or false
Interface labels:
Props automatically give labels, for example:
[data construct: data format (inline | block)]

Property 2:
Name: Example: data type.
...
[data construct: data type (identifier | literal)]

## Emergence

Name:
Type Structural, behavioral, or functional emergence
Recognition:

### Property

Identity: A property is a _thing_ that is a concept, existing in the space if thought, describing the characteristic of a _thing_.
Constitution: Space of thought, a _pre-fundamental thing_ which encapsulates things.
Property:
Emergence: Characteristic.

### The ontological chain:

Emergence depends on property, and property depends on constitution.
Constitution alone does not produce emergence — it must be interpreted to yield properties.
Properties are recognized internal structures among the constitutional things, and these recognized structures are the emergent thing that the thing _is_.

Constitution ➝ Property ➝ Emergence

Layer What it is Relation to the others
Constitution The composed-of layer — the structural components that are themselves things. Provides the basis for...
Property The patterned or characteristic layer — structural features seen in the assembled parts. Emerges from constitution, but is not reducible to it.
Emergence The new thing that comes into being from the configuration of properties. Arises only when the properties align in a way that creates new structural identity.

## Defining the building block of the ontology

A _thing of thins_ is:
Self-defining: It names and identifies its own role and composition.

Self-constituting: Its purpose (root intent) determines what differences matter — thus, it creates the parts it needs to fulfill that intent.

Self-organizing: Through structured perception and categorization.

Self-reflecting: It describes itself using the same descriptive categories that define other things.

### Defining the _thing_ of a thing.

Purpose: organize the field of perception in acordance with choice based in usefulness. (a thing can thus self-constituting, as the root purpose of all things, is to organize the field of perception (meaning define constitutions, properties etc) in accordance with choice based in usefulness. Usefulness is the root purpose, and a thing defines its specific purpose based in that core purpose)

Identity: A _thing of things_ is a discernable purposeful entity that contains discernable enteties that are purposeful to the thing.
Constitution:

- Constituted by the perception of difference.
- Purpose, that one discerned thing is favored (choosen) over another.
- Value of good and bad basis of choice and purpose. An usefull thing is good, a harmful thing is bad.

A _thing of things_ is also constituted by the structure of purpose, identity, constitution, property, and emergence. Thus, a _thing of a thing_ is self-constituting (consisting of the things it defines), self-organizing (organizes itself by the way it defines), and self-reflecting (describes itself using the categories it created). (In addition to this, it is self-defining, in that it identifies its own role in the purpose part, and the purpose part is a result of its self-constituting ability to create such a thing called "purpose")
i

... any constitution of a thing, is the perception of difference, steered by purpose, thus steered perception creates the things needed to fulfil the purpose of the thing.

... The thing envelops many parts and self-constitutes it by enveloping them.”

Property: A thing has the property of containing units. An unit is percieved because of the difference between it and other units. Things are percieved on basis of purpose and usefulness.

Emergence: One unit, delinated in the field of perception, emerges, which has many units. The unit is purposeful.

(The thing of things: It is constituted by perception of difference and the value-driven favoring of one thing over another. It has properties, such as the ability to contain subunits and be perceived as a unit. It gives emergence to structures — these emergent things are the solid, nameable "units" of your system (like data construct). That emergent unit becomes the first usable "Thing" in your ontology.)

A thing of things is a discernible purposeful entity containing other, to the thing of things, discernible purposeful entities.

### Defining the description of a thing:

Identity: The description of a thing is the description, as outlined in text, of the thing's. The descriptoin follows a structure of identity, constitution, property, and emergence.
Constitution: Identity, constitution, property, and emergence.
Property:

- Sequence: The order of the categories is as follows: identity, constitution, property, and emergence.

Emergence: Combining the constitutions of identity, constitution, and property, the emergence in sequence, is the description of the thing.

### Defining identity, constitution, property, and emergence

(Each their own heading, each their own thing)

#### Identity

Identity: Is a concept...
Constitution: Consists of a _thing of things_.

Property:

- Self reliant (: Each concept is a thing created by defining what it is, but without reference to a more abstract or concrete layer.) And all the rest...
  Emergence:

Emergence: a _thing of things_ takes the form of _identity_ by the combined properties...
