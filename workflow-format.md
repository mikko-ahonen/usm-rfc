# USM Workflow JSON Format

## Abstract

USM Workflow JSON Format represents descriptions of Unified Service 
Management (USM) workflows using JSON-based data structures. This 
format is useful for increasing the interoperability between systems
supporting the USM method. JSON is a lightweight, text-based, 
language-independent data interchange format commonly used in Internet 
applications.

## Status of This Memo

This document is a proposal intended for discussion.

## Copyright Notice

Copyright (c) 2021 SURVUZ Foundation and the persons identified as the
document authors. All rights reserved.

## Table of Contents

## Introduction

USM Workflow JSON Format represents descriptions of Unified Service 
Management (USM) workflows using Javascript Object Notation (JSON) based 
data structures. 

The purpose of this format is to allow interoperability between 
systems used to deploy USM in an organization.

Specifically, this is typically helpful to tie workflow and 
customer support or ticketing systems.

JSON was chosen as the interchange format, because JSON is an 
open standard file and data interchange format. Text to store and transmit 
data objects is human-readable. It is also a very common data format, and
lightweight libraries for parsing and generating JSON data is readily 
available in many programming languages. 

### Terminology

These terms are defined by this specification:

USM Workflow JSON Format
    TODO: Missing definition

### Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
"Key words for use in RFCs to Indicate Requirement Levels" [RFC2119].
The interpretation should only be applied when the terms appear in
all capital letters.

BASE64URL(OCTETS) denotes the base64url encoding of OCTETS, per
Section 2.

UTF8(STRING) denotes the octets of the UTF-8 [RFC3629] representation
of STRING, where STRING is a sequence of zero or more Unicode
[UNICODE] characters.

ASCII(STRING) denotes the octets of the ASCII [RFC20] representation
of STRING, where STRING is a sequence of zero or more ASCII
characters.

The concatenation of two values A and B is denoted as A || B.

## MIME types

application/x-usm-workflow-template
application/x-usm-workflow

## File extensions

.workflow-template.json
.workflow

## Metadata

### format-version

Format version field "format-version" corresponds to the version of the USM Workflow JSON format.

The value MUST be a JSON string. The versioning is based on semantic versioning [SEMVER2]. For this 
version of the document, it MUST be string literal "1.0.0"

Example:

  {
    ...
    "format-version": "1.0.0",
    ...
  }


### originating-system-type

originating-system-type uniquely identifies the originating system.

The value MUST be a JSON string. The system-specific value is assigned by SURVUZ Foundation on
request.

Example:

  {
    ...
    "originating-system-type": "foobar",
    ...
  }

### originating-system-version

originating-system-version uniquely identifies the version of originating system.

The value MUST be JSON string. The values are system-specific.

Example:

  {
    ...
    "originating-system-version": "1.0A",
    ...
  }

### originating-system-instance

originating-system-instance uniquely identifies the instance of the originating system.

The value MUST be JSON string. The values are system-specific, but aggregate of (originating-system-type, 
originating-system-instance) MUST be globally unique.

Example:

  {
    ...
    "originating-system-instance": "5831e06b-d579-4a32-91f8-877a18ae7118",
    ...
  }

### workflow-version

workflow-version field specifies the version of the content.

The value MUST be JSON string. The actual value is specific to originating system type. The value must be 
unique within the workflow. The versioning MAY use semantic versioning [SEMVER2]. However, it might not be possible to 
deduce ordering of the values from the values.

Example:

  {
    ...
    "workflow-version": "656e6bcd3e683982a572401b879d6db4b2931a4c",
    ...
  }

### created-at

created-at field specifies the time the workflow was originally created.

The value MUST be a JSON string, and it MUST correspond to the ISO-8601 date and time format 
[ISO-8601-1984]. Milliseconds SHOULD be used to provide better granularity.

Example:

  {
    ...
    "created-at": "2021-03-22T07:38:22.800+00:00",
    ...
  }

### created-by

created-by field identifies the creator of the workflow.

The value MUST be a JSON object representing a person, with the following keys and values:

### id - required

id MUST be a JSON string uniquely identifying the person in the workflow system instance.

### name - optional

if present, name MUST be a JSON string for the name of the person. The value is optional.

### email - optional

if present, name MUST be a JSON string for the email of the person. The value is optional.

Example: 

  {
    ...
    "created-by": {
        "id": "mikko"
        "name": "Mikko Ahonen",
        "email": "mikko@usm.coach"
    },
    ...
 }

### modified-at

modified-at field specifies the last time the workflow was modified.

The value MUST be a JSON string, and it MUST correspond to the ISO-8601 date and time format 
[ISO-8601-1984]. Milliseconds SHOULD be used to provide better granularity.


Example:

  {
    ...
    "modified-at": "2021-03-22T07:38:22.800+00:00",
    ...
  }


### modified-by

modified-by field identifies the last modifier of the workflow.

The value MUST be a JSON object representing a person, with the following keys and values:

### id - required

id MUST be a JSON string uniquely identifying the person in the workflow system instance.

### name - optional

if present, name MUST be a JSON string for the name of the person. The value is optional.

### email - optional

if present, name MUST be a JSON string for the email of the person. The value is optional.

Example: 

  {
    ...
    "modified-by": {
        "id": "mikko"
        "name": "Mikko Ahonen",
        "email": "mikko@usm.coach"
    },
    ...
 }

### workflow-type

workflow-type field specifies the type of the workflow.



### context

name field identifies a context for the workflows. The name MUST be unique within the
system instance. The values are typically specific to the organization where USM is deployed.
For example, they might correspond to the organization hierarchy.

### name

name field identifies the human-readable name of the workflow. The name MUST be unique within the
context.

### description

## steps

Teps 

## profiles


# Security Considerations

# Acknowledgments

   The authors would like to thank the following for their valuable
   contributions: ...

#  References

## Normative References

   [ISO.8601.2004]
              International Organization for Standardization, "Data
              elements and interchange formats -- Information
              interchange -- Representation of dates and times", ISO
              8601, December 2004,
              <http://www.iso.org/iso/catalogue_detail?csnumber=40874>.

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119, March 1997.

   [RFC3986]  Berners-Lee, T., Fielding, R., and L. Masinter, "Uniform
              Resource Identifier (URI): Generic Syntax", STD 66, RFC
              3986, January 2005.

   [RFC4648]  Josefsson, S., "The Base16, Base32, and Base64 Data
              Encodings", RFC 4648, October 2006.

   [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax
              Specifications: ABNF", STD 68, RFC 5234, January 2008.

   [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data
              Interchange Format", RFC 7159, March 2014.

   [SEMVER2]  Preston-Werner, T. "Semantic Versioning 2.0.0", 
              <https://semver.org/>

## Informative References

   ...
