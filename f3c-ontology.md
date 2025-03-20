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
