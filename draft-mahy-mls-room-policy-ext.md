---
title: A Messaging Layer Security (MLS) extension for More Instant Messaging Interoperability (MIMI) room policies
abbrev: MLS Extension for MIMI Room Policy
docname: draft-mahy-mls-room-policy-ext-latest
ipr: trust200902
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"area: art
workgroup: MLS
area: sec
category: info
keyword:
 - room policy
 - MIMI
 - room_policy
 - group chat

stand_alone: yes
pi: [toc, sortrefs, symrefs]

venue:
  group: MLS
  type: Working Group
  mail: mls@ietf.org
  arch: https://mailarchive.ietf.org/arch/browse/mls/
  github: rohan-wire/mls-room-policy-ext/
#  latest: https://github.com/rohan-wire/mls-room-policy-ext/latest

author:
 -  ins: R. Mahy
    name: Rohan Mahy
    organization: Wire
    email: rohan.mahy@wire.com

normative:

informative:

--- abstract

This document describes a Message Layer Security (MLS) GroupContext extension
to include More Instant Messaging Interoperability (MIMI) group chat (room)
policy inside an MLS group. This allows all users in a group to agree on the
current policy and make authorization decisions accordingly.

--- middle

# Introduction

The More Instant Messaging Interoperability (MIMI) group chat framework
{{!I-D.mahy-mimi-group-chat}} defines a room policy format. This document
describes a Message Layer Security (MLS) {{!I-D.ietf-mls-protocol}}
GroupContext extension that allows all users in a group to agree on the
current policy and make authorization decisions accordingly.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

The terms MLS client, MLS group, GroupContext, Commit, Proposal,
GroupContextExtensions Proposal, and`required_capabilities`
have the same meanings as in the MLS protocol [@!I-D.ietf-mls-protocol].

# Extension Description

This document specifies a GroupContext MLS extension `room_policy`
of type RoomPolicy (defined in Section 8 of {{!I-D.mahy-mimi-group-chat}}).
The syntax is described using the TLS Presentation Language [@!RFC8446].

~~~ tls-presentation
RoomPolicy room_policy;
~~~

An MLS client which implements this specification SHOULD include the
`room_policy` extension in the `extensions` field in the
GroupContext of groups it creates which have an associated room policy.
It includes a machine readable description of the room policy sufficient
for all clients in the group to authorize MLS primitives (like a Commit
with an Add Proposal). As new members of the group are added or removed, the member
which Commits these membership changes is expected to maintain the `room_policy`
up-to-date, and consistent with the membership of the group by also including an appropriate
GroupContextExtensions Proposal any time the policy changes. It also allows
a client to inspect the `room_policy` before joining to determine if the policies
are consistent with its own.

# Security Considerations

The Security Considerations of MLS apply.
The authorization rules for the RoomPolicy object are described in
detail in {{!I-D.mahy-mimi-group-chat}}.

Improper use of the extension in this document could allow the creator of an
MLS group or the sender of a GroupContextExtensions Proposal to maliciously
modify or subvert the policies of a group chat which could result in viewing
private information about the participants of the room, loss of confidentiality
of the contents of the room (after subverting the policies and until the policies
are corrected), and injecting potentially unwanted messages into the room
(spam, abuse, phishing, and attempted impersonation).


# IANA Considerations

This document proposes registration of a new MLS Extension Type.

RFC EDITOR: Please replace XXXX throughout with the RFC number assigned to this document

## room_policy MLS Extension Type

The `room_policy` MLS Extension Type is used inside GroupContext objects. It
contains a RoomPolicy object as defined in Section 8 of {{!I-D.mahy-mimi-group-chat}},
representing the room policy for the MLS group corresponding with the room.

~~~~~~~~
  Template:
  Value: 0x000B
  Name: room_policy
  Message(s): This extension may appear in GroupContext objects
  Recommended: Y
  Reference: RFC XXXX
~~~~~~~~

--- back

