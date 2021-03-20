# USM Workflow JSON Format

## Abstract

USM Workflow JSON Format represents descriptions of Unified Service 
Management (USM) workflows using JSON-based data structures. This 
format is useful for interoperability between various systems
supporting the USM method.

## Status of This Memo

This document is a proposal intended for discussion.

## Copyright Notice

Copyright (c) 2021 SURVUZ Foundation and the persons identified as the
document authors. All rights reserved.

## Table of Contents

## Introduction

USM Workflow JSON Format represents descriptions of Unified Service 
Management (USM) workflows using JSON-based data structures. 

The purpose of this format is to allow interoperability between 
various systems that used to implement USM functionality. Specifically,
typically workflow and customer support systems (also known as ticketing
systems).

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

