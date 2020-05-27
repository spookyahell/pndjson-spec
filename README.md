---
title: PNDJSON - Pretty Newline delimited JSON
version: 1.0.0
last_update: 2020-05-28
created: 2013-07-05
---

# PNDJSON - Pretty Newline delimited JSON

A standard for delimiting JSON in stream protocols while also pretty printing the output.

## 1. Introduction

## 1.1 About

There are already a number of stanadrds for newline delimited JSON text within a stream protocol, apart from \[[Websockets]\], which is unnecessarily complex for non-browser applications.<br/>
Here's some:<br/>
[JSON Lines](http://jsonlines.org/)<br/>
[ndjson](https://github.com/ndjson/ndjson-spec)<br/>
[ldjson](https://github.com/finnp/ldjson-spec) (fork of ndjson; in draft mode since 2014)

A common use case for PNDJSON is delivering multiple instances of JSON text through streaming protocols like TCP or UNIX Pipes. It can also be used to store semi-structured data.


### 1.2 Terminology
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119. \[[RFC2119]\]7

The term "newline character" refers to the newline character itself - `\n` (0x0A) or in combination with a carriage return (0x0D) - `\r\n`

## 2. Example PNDJSON

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
(with `\n` line separators)

## 3. Functional Specification

### 3.1 Serialization

Each JSON text MUST conform to the \[[RFC7159]\] standard and MUST be written to the stream followed by the newline character. The JSON texts MAY contain any amount of newlines but MAY not use more than ONE (1) newline at a time, every newline within a single JSON text is to be followed by JSON style text.

All serialized data MUST use the UTF8 encoding. And the output (when saved to a file) must be saved with Byte Order Mark, namely UTF-8-BOM (`utf-8-sig` in python).

### 3.2 Parsing

The parser MUST accept newline as line delimiter `\n` (0x0A) as well as carriage return and newline `\r\n` (0x0D0A). 

If the JSON text is not parsable, the parser SHOULD raise an error. The parser MAY silently ignore empty lines, e.g. `\n\n`. This behavior MUST be documented and SHOULD be configurable by the user of the parser.

### 3.3 MediaType and File Extensions

The MediaType \[[RFC4288]\] for Newline Delimited JSON SHOULD be _application/x-ndjson_.

When saved to a file, the file extension SHOULD be _.ndjson_.

## 4. Copyright

This specification is copyrighted by the authors named in section 4.1. It is free to use for any purposes commercial or non-commercial.

### 4.1 Authors

The following authors are responsible for the NDJSON core-specification:

~~~~
Thorsten Hoeger
Taimos GmbH
Hohenzollernstrasse 32
D-73262 Reichenbach
thorsten.hoeger@taimos.de
~~~~
~~~~
Chris Dew
chris.dew@barricane.com
~~~~
~~~~
Finn Pauls
ich@finnpauls.de
~~~~
~~~~
Jim Wilson
~~~~

### 4.2 Contact

This specification and any related work is located at <https://github.com/ndjson>. 
Discussion and help is located at <https://github.com/ndjson/ndjson-spec/issues>. 

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
