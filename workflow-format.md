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
one for workflow, that are almost identical, except that requirements for
required fields are more relaxed.

### Terminology

<dl>
<dt>USM Workflow Interchange Format (USM-WIF)</dt>
<dd>JSON format defined in this document for USM Workflow interchange.</dd>

<dt>Must-Ignore Policy</dt>
<dd>When an implementation encounters a protocol element that it does not
recognize, it should treat the rest of the protocol transaction as if
the new element simply did not appear, and in particular, the
implementation MUST NOT treat this as an error condition. [RFC7493]</dd>

<dt>Must-Understand Policy</dt>
<dd>Implementations do not tolerate the introduction of new elements 
that they do not recognize, but treat them as an error condition.
[RFC7493]</dd>
</dl>

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

## Protocol Implementation Details

The implementations processing USM-WIF entities, MUST follow the
Must-Ignore Policy, in other words MUST NOT treat unrecognized elements 
as an error condition.

The order of object members in an USM-WIF message MUST NOT change the
meaning of the message.  A receiving implementation MAY treat
two messages as equivalent if they differ only in the order of
the object members.

## MIME types

Two temporary MIME types are proposed to be used. Later, we aim to 
register corresponding MIME types with IANA (Internet Assigned 
Numbers Authority).

"application/x-usm-workflow-template" - MIME type used for workflow templates.

"application/x-usm-workflow" - MIME type used for workflows.

## File extensions

Two following filename extensions SHOULD be used for the
file formats.

".workflow-template.json" - File extension used for workflow templates.

".workflow.json" - File extension used for workflows.

## Entities

The diagram below shows the relationships between the main entities. 
Person entity has been omitted for simplicity.

    +-------------------+
    | Workflow          |
    | +---------------+ |
    | | Step          | |
    | | +----------+  | |
    | | | Activity |  | |
    | | +----------+  | |
    | | +----------+  | |
    | | | Activity |  | |
    | | +----------+  | |
    | | +----------+  | |
    | | | Activity |  | |
    | | +----------+  | |
    | +---------------+ |
    | +---------------+ |
    | | Profile       | |
    | +---------------+ |
    | +---------------+ |
    | | Profile       | |
    | +---------------+ |
    +-------------------+

### Workflow (and Workflow template)

Workflow is the top-level entity.

Workflow templates have exactly the same format as the workflows, but there 
are no required fields.

#### format-version

Field "format-version" corresponds to the version of the USM Workflow 
Interchange format. The value MUST be a JSON string. The versioning is
based on semantic versioning [SEMVER2]. For this version of the document, 
it MUST be string literal "1.0.0".

#### source-system-type 

Field "source-system-type" uniquely identifies the originating system.

The value MUST be a JSON string. The value MUST consist of two
parts, seperated by colon. First part MUST be the domain name of 
the system vendor. The second part is system name, uniquely identifying
the software within the vendor.

The identifier MUST contain only alphanumeric characters and dash.
Colon is used as a separator.

#### source-system-version

source-system-version uniquely identifies the version of originating system.

The value MUST be JSON string. The values are system-specific.

#### source-system-instance

source-system-instance uniquely identifies the instance of the originating system.

The value MUST be a JSON string. The values are system-specific, but aggregate
of (originating-system-type, originating-system-instance) MUST be globally
unique.

#### workflow-version

workflow-version field specifies the version of the content.

The value MUST be JSON string. The actual value is specific to originating
system. The value MUST be unique within the workflow. The versioning MAY
use semantic versioning [SEMVER2]. However, it might not be possible to deduce
ordering of the values from the values. The example below uses UUIDs.

#### created-at

created-at field specifies the time the workflow was originally created.

The value MUST be a JSON string, and it MUST correspond to the ISO-8601 date and time format 
[ISO-8601-1984]. Milliseconds SHOULD be used to provide better granularity.

#### created-by

created-by field identifies the creator of the workflow.

The value MUST be a Person entity represented as JSON object.

#### modified-at

modified-at field specifies the last time the workflow was modified.

The value MUST be a JSON string, and it MUST correspond to the ISO-8601 date 
and time format [ISO-8601-1984]. Milliseconds SHOULD be used to provide better 
granularity.

#### modified-by

This field identifies the last modifier of the workflow.

The value MUST be a Person entity represented as JSON object.

#### process

process field specifies the USM process.

The value MUST be a JSON string, and it MUST have one of the following string literal values:

* "agree" - Agree process

* "change" - Change process

* "recover" - Recover process

* "operate" - Operate process

* "improve" - Improve process

#### dynamicity

This field specifies whether the workflow is static or dynamic, with the following string literal values:

* "static" - For static workflows, the path through the USM processes is pre-defined.

* "dynamic" - For dynamic workflows, where path is determined at the fork step in the workflow.

#### context

context field identifies the domain for the workflows. The field MUST be unique within the
system instance. 

The name MUST be a JSON string. The values are typically specific to the organization 
where USM is deployed. For example, they might correspond to the organization hierarchy.

#### slug

slug field identifies short, human-readable name of the workflow. The slug MUST be unique within the
context.

The slug MUST be a JSON string. Workflow definition MUST include this field.

The field MUST only contain alphanumeric characters and dashes. The field MUST NOT start or end with
dashes, or contain multiple consequtive dashes.

#### name

name field is a human-readable name of the workflow. The name SHOULD be unique within the context.

The name MUST be a JSON string. Workflow definition MUST include this field.

#### description

description field identifies the human-readable description of the workflow.

The description MUST be a JSON string. Workflow definition MAY include this field.

#### type

type field identifies the type of the document. It MUST be a JSON string, and one of two string 
literal values is allowed.

* "workflow-template" - For workflow templates. Workflow templates are used as a basis for workflow definitions.

* "workflow" - For workflows. Workflows are more commonly used for interoperabilty between Business Process
  Management (BPM) and Workflow systems.

#### steps

JSON list of Step entities.

#### Example

```
  {
    "format-version": "1.0.0",
    "source-system-type": "foo.com:bar",
    "source-system-version": "1.0",
    "source-system-instance": "5831e06b-d579-4a32-91f8-877a18ae7118",
    "workflow-version": "656e6bcd3e683982a572401b879d6db4b2931a4c",
    "created-at": "2021-03-22T07:38:22.800+00:00",
    "created-by": {
        ...
    },
    "modified-at": "2021-03-22T07:38:22.800+00:00",
    "modified-by": {
        ...
    },
    "process": "agree",
    "dynamicity": "static",
    "type": "workflow",
    "context": "it-infra",
    "slug": "new-laptop",
    "name": "Order a new laptop",
    "description": "New laptop workflow for both consultants and internal emplyoees without existing accounts.",
    "steps": {
        ...
    }
  }
```

### Person

Entity refers to people, represented as an JSON object, with the following fields:

#### id - required

id MUST be a JSON string uniquely identifying the person within the workflow system instance.

#### name - optional

if present, name MUST be a JSON string for the name of the person. The value is optional.

#### email - optional

if present, name MUST be a JSON string for the email of the person. The value is optional.

#### Example 

```
{
    "id": "s393939"
    "name": "Mikko Ahonen",
    "email": "mikko@usm.coach"
}
```

### Profile

Profile refers to either role or position in the organization hierarchy, that may be
assigned responsibilities for this workflow. It is represented as a JSON object.

#### id

This field MUST uniquely identify the role or position. This value MUST be JSON string.
This field MUST be unique within the organization using this USM deployment.

#### name

Human-readable name for the profile. This field MUST be a JSON string.

#### Example

```
    {
        "id": "operator",
        "name": "IT service desk Operator"
    }
```

### Step

Step is an entity representing a step in the USM process model.

#### id

id field MUST uniquely identify the step within the workflow.

This field MUST be a JSON string.

This field is pre-filled from the template.

#### sort-index

sort-index is an optional field, that if present, suggests the ascending ordering 
of the steps within the workflow.

The value MUST be a non-negative JSON integer.

#### name

name field is a human-readable name for the step.

This field MUST be a JSON string.

This field is pre-filled from the template.

#### description

description field is a human-readable description for the step.

This field MUST be a JSON string.

This field is pre-filled from the template.

#### process

process field specifies the USM process for this step.

The value MUST be a JSON string, and it MUST have one of the same 
string literal values defined for the process.

#### activities

activities is a JSON list containing Activity entities.

#### Example

```
    {
        "id": "1",
        "sort-index": "3",
        "name": "Accept the request",
        "description": "Log the wish, and link it to the service ...",
        "process": "agree",
        "activities": [ ... ]
    }
```

### Activity

Describes one activity within the step, and the responsiblity assignment
matrix according to the RACI model [RACI].

#### id

This field MUST uniquely identify the activity within the step. This field MUST 
be a JSON string.
  
#### sort-index

sort-index suggests the ascending ordering of the activities within the step.

The value MUST be a non-negative JSON integer.

#### description

Human-readable description of the activity. This field MUST be a JSON string.

#### responsibilities

List of responsibility assignments. The field value MUST be a JSON object, 
where key is the identifier of a profile, and value is a list of JSON string 
literals, referring to the RACI model responsibility roles [RACI].

* "R" - Responsible
* "A" - Accountable
* "C" - Consulted
* "I" - Informed

#### Example

```
    {
        "id": "1",
        "sort-index": "1",
        "name": "Log the wish",
        "description": "Log the wish into the X system",
        "responsibilities": { 
            "operator": [ "R" ]
        }
    }
```

# Internationalization and Localization

TODO

# Security Considerations

All the security considerations that apply to JSON (see [RFC 7159])
apply to this format. There are no additional security considerations
specific to USM-WIF.

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
              <http://www.rfc-editor.org/info/rfc2119>.

   [RFC3986]  Berners-Lee, T., Fielding, R., and L. Masinter, "Uniform
              Resource Identifier (URI): Generic Syntax", STD 66, RFC
              3986, January 2005,
              <http://www.rfc-editor.org/info/rfc3986>.

   [RFC4648]  Josefsson, S., "The Base16, Base32, and Base64 Data
              Encodings", RFC 4648, October 2006,
              <http://www.rfc-editor.org/info/rfc4648>.

   [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax
              Specifications: ABNF", STD 68, RFC 5234, January 2008,
              <http://www.rfc-editor.org/info/rfc5234>.

   [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data
              Interchange Format", RFC 7159, March 2014,
              <http://www.rfc-editor.org/info/rfc7159>.

   [RFC7493]  Bray, T., Ed., "The I-JSON Message Format", RFC 7493,
              DOI 10.17487/RFC7493, March 2015,
              <http://www.rfc-editor.org/info/rfc7493>.

   [SEMVER2]  Preston-Werner, T. "Semantic Versioning 2.0.0", 
              <https://semver.org/>.

## Informative References

   ...

## Author's Address

   Mikko Ahonen (editor)<br/>
   USM Coach

   E-Mail: mikko@usm.coach<br/>
   URI:    https://usm.coach/
