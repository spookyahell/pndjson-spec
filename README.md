---
title: PNDJSON - Pretty Newline delimited JSON
version: 1.0.0
last_update: 2020-05-28
created: 2013-07-05 (the original); 2020-05-28 (this repo)
---

# PNDJSON - Pretty Newline delimited JSON

A standard for delimiting JSON in stream protocols while also pretty printing the output.

## 1. Introduction

## 1.1 About

There are already a number of stanadrds for newline delimited JSON text within a stream protocol, apart from \[[Websockets]\], which is unnecessarily complex for non-browser applications.

Here's some:<br/>
* [JSON Lines](http://jsonlines.org/)<br/>
* [ndjson](https://github.com/ndjson/ndjson-spec) (where this spec was originally forked from)<br/>
* [ldjson](https://github.com/finnp/ldjson-spec) (fork of ndjson; in draft mode since 2014)

Intended use case for PNDJSON (actually I may prefer PNDjson to seperate PND from the well-known format) is storing human-readable json texts in one `anything.json` file.

A common use case for the original NDJSON is delivering multiple instances of JSON text through streaming protocols like TCP or UNIX Pipes. It can also be used to store semi-structured data. I thought okay, lets have the original in here too, what the heck?. But this particular format is probably not great for streaming via TCP. In fact if you're doing that I think I am compelled to have to stress that it is NOT a desired format, but if it helps you: Not gonna stop you.


### 1.2 Terminology
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119. \[[RFC2119]\]

The term "newline character" refers to the newline character itself - `\n` (0x0A) or in combination with a carriage return (0x0D) - `\r\n`

## 2. Example PNDJSON

Example #1 (with `\n\n` line separators)
~~~~~
{
  "some": "thing"
}

{
  "foo": 17,
  "bar": false,
  "quux": true
}

{
  "may": {
    "include": "nested",
    "objects": [
      "and",
      "arrays"
    ]
  }
}
~~~~~
Example #2 (including captions)
~~~~~
EXAMPLE JSON
{
  "some": "thing"
}

2nd EXAMPLE JSON
{
  "foo": 17,
  "bar": false,
  "quux": true
}

3rd EXAMPLE JSON
{
  "may": {
    "include": "nested",
    "objects": [
      "and",
      "arrays"
    ]
  }
}
~~~~~
(with `\n` line separators)

## 3. Functional Specification

### 3.1 Serialization

Each JSON text MUST conform to the \[[RFC7159]\] standard and MUST be written to the stream followed by the newline character. The JSON texts MAY contain any amount of newlines but MAY not use more than ONE (1) newline at a time, every newline within a single JSON text is to be followed by JSON style text.

All serialized data MUST use the UTF8 encoding. And the output (when saved to a file) must be saved with Byte Order Mark, namely UTF-8-BOM (`utf-8-sig` in python).

Every JSON text that is a valid JSON object or list must be followed by two (2) newline characters.<br/>
The existance of one empty line marks the ending of one JSON text and — unless EOF occurs — the beginnging of a new one.<br/>

After the empty line (the two new line characters) it MAY be allowed to add a title soley BEFORE the JSON text to give it a title/caption, this IS NOT required to be part of a parser and/or application that produces files in PNDjson format, but could be a disired advanced functionality.<br/>
Any caption MUST only include UTF-8 compatible characters.<br/>
It SHOULD not exceed a length of 50 and MAY not exceed a maximum length of 100 chars.<br/>
Captions SHOULD be (mostly) capitalized to visually mark a heading.<br/>
No other limitations MAY apply for the caption.

### 3.2 Parsing

The parser MUST accept newline as line delimiter as defined in the section titled **Terminology** of this specification. 

If the JSON text is not parsable, the parser SHOULD raise an error. The parser MUST silently ignore empty lines, e.g. `\n\n`. This MUST NOT be configurable by the user of the parser.

### 3.3 MediaType and File Extensions

The MediaType \[[RFC4288]\] for Newline Delimited JSON SHOULD be _application/x-pndjson_.

When saved to a file, the file extension SHOULD be _.pjson_ or _.pndjson_.
## 4. Copyright

This specification is copyrighted by the author named in section 4.1.

It is free to use for any purposes commercial or non-commercial.

When you use in a commercial product, the author would love a donation (one-time or reoccurring).<br/>
Use the author's email to contact him about how to do this: <vidner123@gmail.com>

### 4.1 Authors

This specification is mainly authored and maintained by @spookyhahell (Trademark pending)

Visit [ndjson](https://github.com/ndjson/ndjson-spec) for a list of authors who are responsible of the original ndjson format.

### 4.2 Contact

This specification and any related work is located at <https://github.com/spookyahell/pndjson>.<br/>
Please refrain from using the issue tracker, located at <https://github.com/spookyahell/pndjson/issues>.<br/>
The format probably won't be changed even if people have valid reasons to object to the specification. They are welcome to create their own format. This is my format.

## A. References

### A.1 Normative

[RFC2119]: http://www.ietf.org/rfc/rfc2119.txt "RFC 2119 - Key words for use in RFCs to Indicate Requirement Levels"
\[RFC2119\]: RFC 2119 - Key words for use in RFCs to Indicate Requirement Levels

[RFC7159]: http://www.ietf.org/rfc/rfc7159.txt "RFC 7159 -  The JavaScript Object Notation (JSON) Data Interchange Format"
\[RFC7159\]: RFC 7159 -  The JavaScript Object Notation (JSON) Data Interchange Format

[RFC4627]: http://www.ietf.org/rfc/rfc4627.txt "RFC 4627 - The application/json Media Type for JavaScript Object Notation (JSON)"
\[RFC4627\]: RFC 4627 - The application/json Media Type for JavaScript Object Notation (JSON)

[RFC4288]: http://www.ietf.org/rfc/rfc4288.txt "RFC 4288 - Media Type Specifications and Registration Procedures"
\[RFC4288\]: RFC 4288 - Media Type Specifications and Registration Procedures

[RFC2616]: http://www.ietf.org/rfc/rfc2616.txt "RFC 2616 - Hypertext Transfer Protocol -- HTTP/1.1"
\[RFC2616\]: RFC 2616 - Hypertext Transfer Protocol -- HTTP/1.1

### A.1 Informative

[TCP]: http://www.ietf.org/rfc/rfc793.txt "RFC 793 - Transmission Control Protocol"
\[TCP\]: RFC 793 - Transmission Control Protocol

[Websockets]: http://tools.ietf.org/html/rfc6455 "RFC 6455 - The WebSocket Protocol"
\[Websockets\]: RFC 6455 - The WebSocket Protocol
