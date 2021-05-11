# USM Workflow Interchange Format

## Abstract

USM Workflow Interchange Format (USM-WIF) represents descriptions of Unified Service 
Management (USM) workflows using JSON-based data structures. The 
purpose is to allow for interoperability between systems
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

USM Workflow Interchange Format USM-WIF represents descriptions of Unified Service 
Management (USM) workflows using Javascript Object Notation (JSON) based 
data structures.

The purpose of this format is to allow interoperability between 
systems used to deploy USM in an organization. Specifically, this typically 
helps to integrate Business Process Management (BPM) systems with
Workflow (also known as Customer Support or Ticketing) systems.

JSON was chosen as the interchange format, as it is an 
open and de facto standard data interchange format. Text to store 
and transmit data objects is human-readable. As it is very common 
data format lightweight libraries for parsing and generating JSON data 
are readily available in many programming languages. 

This memo defines two formats, one for workflow template and
one for workflow, that are almost identical, except for their
place in the life cycle of definitions.

### Terminology

    TODO

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

Two temporary MIME types are proposed to be used. Later, we aim to 
register corresponding MIME types with IANA (Internet Assigned 
Numbers Authority).

application/x-usm-workflow-template
    MIME type used for workflow templates.

application/x-usm-workflow
    MIME type used for workflows.

## File extensions

Two following filename extensions SHOULD be used for the
file formats.

.workflow-template.json
    File extension used for workflow templates.

.workflow.json
    File extension used for workflows.

## Entities

### Profile



## Attributes

### format-version

Format version field "format-version" corresponds to the version of the USM
Workflow Interchjange format.

The value MUST be a JSON string. The versioning is based on semantic versioning
[SEMVER2]. For this version of the document, it MUST be string literal "1.0.0"

Example:

```json
  {
    ...
    "format-version": "1.0.0",
    ...
  }
```

### source-system-type

source-system-type uniquely identifies the originating system.

The value MUST be a JSON string. The value MUST consist of two
parts, seperated by colon. First part MUST be the domain name of 
the system vendor. The second part is system name, uniquely identifying
the software within the vendor.

The identifier MUST contain only alphanumeric characters and dash.
Colon is used as a separator.

Example:

```json
  {
    ...
    "source-system-type": "foo.com:bar",
    ...
  }
```

### source-system-version

source-system-version uniquely identifies the version of originating system.

The value MUST be JSON string. The values are system-specific.

Example:

```json
  {
    ...
    "source-system-version": "1.0",
    ...
  }
```

### source-system-instance

source-system-instance uniquely identifies the instance of the originating system.

The value MUST be a JSON string. The values are system-specific, but aggregate
of (originating-system-type, originating-system-instance) MUST be globally
unique.

Example:

```json
  {
    ...
    "originating-system-instance": "5831e06b-d579-4a32-91f8-877a18ae7118",
    ...
  }
```

### workflow-version

workflow-version field specifies the version of the content.

The value MUST be JSON string. The actual value is specific to originating
system. The value MUST be unique within the workflow. The versioning MAY
use semantic versioning [SEMVER2]. However, it might not be possible to deduce
ordering of the values from the values. The example below uses UUIDs.

Example:

```json
  {
    ...
    "workflow-version": "656e6bcd3e683982a572401b879d6db4b2931a4c",
    ...
  }
```

### created-at

created-at field specifies the time the workflow was originally created.

The value MUST be a JSON string, and it MUST correspond to the ISO-8601 date and time format 
[ISO-8601-1984]. Milliseconds SHOULD be used to provide better granularity.

Example:

```json
  {
    ...
    "created-at": "2021-03-22T07:38:22.800+00:00",
    ...
  }
```

### created-by

created-by field identifies the creator of the workflow.

The value MUST be a JSON object representing a person, with the following keys and values:

id - required
  id MUST be a JSON string uniquely identifying the person within the workflow system instance.

name - optional
  if present, name MUST be a JSON string for the name of the person. The value is optional.

email - optional
  if present, name MUST be a JSON string for the email of the person. The value is optional.

Example: 

```json
  {
    ...
    "created-by": {
        "id": "mikko"
        "name": "Mikko Ahonen",
        "email": "mikko@usm.coach"
    },
    ...
 }
```

### modified-at

modified-at field specifies the last time the workflow was modified.

The value MUST be a JSON string, and it MUST correspond to the ISO-8601 date and time format 
[ISO-8601-1984]. Milliseconds SHOULD be used to provide better granularity.

Example:

```json
  {
    ...
    "modified-at": "2021-03-22T07:38:22.800+00:00",
    ...
  }
```

### modified-by

modified-by field identifies the last modifier of the workflow.

The value MUST be a JSON object representing a person, with the following keys and values:

id - required
  id MUST be a JSON string uniquely identifying the person within the workflow system instance.

name - optional
  if present, name MUST be a JSON string for the name of the person. The value is optional.

email - optional
  if present, name MUST be a JSON string for the email of the person. The value is optional.

Example: 

```json
  {
    ...
    "modified-by": {
        "id": "mikko"
        "name": "Mikko Ahonen",
        "email": "mikko@usm.coach"
    },
    ...
 }
```

### process

process field specifies the USM process.

The value MUST be a JSON string, and it MUST have one of the following string literal values:

"agree"
  Agree process

"change"
  Change process

"recover"
  Recover process

"operate"
  Operate process

"improve"
  Improve process

Example:

```json
  {
    ...
    "process": "agree",
    ...
 }
```

### dynamicity

dynamicity field specifies whether the workflow is static or dynamic, with the following string literal values:

"static"
  For static workflows, the path through the USM processes is pre-defined.

"dynamic"
  For dynamic workflows, it is determined at the fork step in the workflow.

Example:

```json
  {
    ...
    "dynamicity": "static",
    ...
  }
```

### context

context field identifies the domain for the workflows. The field MUST be unique within the
system instance. 

The name MUST be a JSON string. The values are typically specific to the organization 
where USM is deployed. For example, they might correspond to the organization hierarchy.

Example:

```json
  {
    ...
    "context": "it-infra",
    ...
  }
```

### slug

slug field identifies short, human-readable name of the workflow. The slug MUST be unique within the
context.

The slug MUST be a JSON string. Workflow definition MUST include this field.

The field MUST only contain alphanumeric characters and dashes. The field MUST NOT start or end with
dashes, or contain multiple consequtive dashes.

Example:

```json
  {
    ...
    "slug": "new-laptop",
    ...
  }
```

### name

name field is a human-readable name of the workflow. The name SHOULD be unique within the context.

The name MUST be a JSON string. Workflow definition MUST include this field.

```json
Example:

  {
    ...
    "name": "Order a new laptop",
    ...
  }
```

### description

description field identifies the human-readable description of the workflow.

The description MUST be a JSON string. Workflow definition MAY include this field.

Example:

```json
  {
    ...
    "description": "New laptop workflow for both consultants and internal emplyoees without existing accounts.",
    ...
  }
```

### type

type field identifies the type of the document. It MUST be a JSON string, and one of two string 
literal values is allowed.

"workflow-template"
  For workflow templates. Workflow templates are used as a basis for workflow definitions.

"workflow"
  For workflows. Workflows are more commonly used for interoperabilty between Business Process
  Management (BPM) and Workflow systems.

Example:

```json
  {
    ...
    "type": "workflow",
    ...
  }
```

### steps

Contains a JSON list of step items, defined below.

Example:

```json
  {
    ...
    "steps": [
        {
            ... 
        },  
        ...
        {
            ...
        }
    ],
    ...
  }
```

#### step item

Example:

```json
    ...
    {
        "id": ...,
        "name": ...,
        "description": ...,
        "activities": [ ...
        ],
    },  
    ...
```

##### id

id field MUST uniquely identify the step within the workflow.

This field MUST be a JSON string.

Example:

```json
    ...
    "id": "1",
    ...
```

##### sort-index

sort-index is an optional field, that if present, suggests the ascending ordering 
of the steps.

The value MUST be a non-negative JSON integer.

Example:

```json
    ...
    "sort-index": "3",
    ...
```

##### name

name field is a human-readable name for the step.

This field MUST be a JSON string.

Example:

```json
  ...
  "name": "Accept the erquest",
  ...
```

##### description

description field is a human-readable description for the step.

This field MUST be a JSON string.

##### process

process field specifies the USM process for this step.

The value MUST be a JSON string, and it MUST have one of the same string literal values 
defined for the process. This is used to provide all the alternatives 

##### activities

activities field specifies a list of activities and the responsiblity assignment
matrix according to the RACI model [RACI].

It contains the following fields:

id
    This field MUST uniquely identify the activity within the step. This field MUST 
    be a JSON string.
  
description
    Human-readable description of the activity. This field MUST be a JSON string.

responsibilities
    List of responsibility assignments. This field MUST be a JSON object, where key is
    the identifier of a profile, and value is a list of JSON string literals

## profiles

Contains a JSON list of profile items, defined below.

### profile item

A JSON object representing a profile, with the following fields:

id
    This field MUST uniquely identify the role. This value MUST be JSON string.
    This field MUST be unique within the
    organization using this USM deployment.

name
    Human-readable name for the activity. This field MUST be a JSON string.

# Internationalization and Localization

    TODO

# Security Considerations

All the security considerations that apply to JSON (see [RFC 7159])
apply to this format. There are no additional security considerations
specific to USM-WIF.

# An Example

# Acknowledgments

   The authors would like to thank the following people for their 
   valuable contributions: ...

#  References

## Normative References

   [ISO.8601.2004]
              International Organization for Standardization, "Data
              elements and interchange formats -- Information
              interchange -- Representation of dates and times", ISO
              8601, December 2004,
              <http://www.iso.org/iso/catalogue_detail?csnumber=40874>.

   [RACI]     Smith, M., Erwin J., "Role & Responsibility Charting (RACI)", 
              Project Management Forum, 2005.

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
