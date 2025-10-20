---
stand_alone: true
ipr: trust200902
cat: std
submissiontype: IETF
area: Applications
wg: sml

docname: draft-ietf-sml-trust-latest

title: Trust and security considerations for Structured Email
abbrev: SML Trust
lang: en
author:
- name: Hans-Joerg Happel
  org: audriga
  street:
  city:
  code:
  country:
  email: happel@audriga.com
  uri: https://www.audriga.com
- name: Arnt Gulbrandsen
  org: ICANN
  street: 6 Rond Point Schumann, Bd. 1
  city: Brussels
  code: 1040
  country: Belgium
  email: arnt@gulbrandsen.priv.no
  uri: https://icann.org/ua

normative:

informative:
  I-D.sml-structured-email:
  RFC5228:
  RFC6132:
  RFC6134:
  RFC7942:
  CalSpam:
    target: https://standards.calconnect.org/csd/cc-18003.html
    title: Calendar operator practices — Guidelines to protect against calendar abuse
  MachineReadable:
    target: https://csrc.nist.gov/glossary/term/Machine_Readable
    title: NIST IR 7511 Rev 4

--- abstract

This document discusses trust and security considerations for
structured email and provides
recommendations for message user agents on how to deal with structured
data in email messages.

--- middle

# Introduction

Structured email, as described in {{I-D.sml-structured-email}},
makes the content of some email messages machine-readable, such that
user agents can provide higher-level functions than
displaying/replying, for example "add this to calendar".

Naturally, new functions bring new trust and security considerations,
or bring new urgency to existing issues.  This document recommends
security and trust mechanisms that should be applied when processing
machine-readable content in email messages, both by senders and
receivers.

# Requirements Language

{::boilerplate bcp14-tagged}

# Types of security concerns

This section gives an overview of the various types of security and
privacy concerns that arise when email messages contain structured
data. The same concerns often arise for other messages, of course.

This section is informative. Or even unnecessary.

## Spam/virus filters

Structured email increases the syntactical and semantic complexity of
email messages. If a spam/virus filter parses structured email in
order to block malevolent messages, the filter's parser will
necessarily differ from that of the MUA that will finally act on the
structured data, creating a risk of misclassification.

These risks are elevated when a structured data format has complex
syntax, syntax that offers several optional or alternative ways to
express the same substance, and of course by parsers that deviate from
the specification for better bug compatibiloty.

## Formal display of data

A common example is displaying a received calendar invitation using
dates/times in the recipient's timezone, in a fixed format.

Formal display introduces additional possibilities of discrepancy
between the different representations. For example, a single message
might contains a multipart/alternative containing a text/plain
description of a flight itinerary, a text/html description of the same
itinerary, and a structured representation. All three may be
different, leading to confusion (and in this example, perhaps to
missing a flight).

Unintentional discrepancy is a risk for senders; some recipients may
be misinformed.

If a message is sent to a group and there is a discrepancy, different
members of the group may see it differently.

If a particular MUA displays the formal representation within the
message, a malevolent sender could try to mimic the visual
representation using HTML with CSS, but with misleading content.

## Automated processing

Automated processing covers actions that are taken as soon as the
message arrives rather than when a human user reads the message. For
example, a user might want flight reservations to be automatically
added to a travel itinerary application and/or a calendar.

Such automation could be a custom MUA feature or a future extension of
the Sieve email filtering language ({{RFC5228}}). A related example
for abuse in automated processing is calendar spam ({{CalSpam}}).

## External references

Email messages with a text/html body part ("HTML email messages") may
contain image resources that link to web servers. Such links can be
used for tracking user interaction with the message.

Similar concerns apply to structured data types which include image
references, such as the cover image of a music album or the teaser
image of a news article.

RDF structured data can be partial by design and include references to
additional data. Using a "follow your nose" approach, tools can follow
URL references to obtain further structured data concerning a
resource. For example, a piece of structured data about an article
could reference the article's authors only by URL. For a meaningful
processing of author information, one might try to obtain further data
using that URL.

## Social engineering

While the risks of social engineering are hardly new and the
human-readable text in a message can in principle be used to persuade
the human reader to do anything, structured data widens the variety of
actions the human reader can easily perform. If there are more buttons
to click, then there's also a greater variety of attacks.

Put differently: A user who might not be able to follow the
instructions in a long and involved text-based social engineering
attack may be able to follow simple instructions such as "click this
then that".

# Trust mechanisms

Several implementations of structured email restrict processing to
messages that are particularly trusted. That is to say, an incoming
message is in one of these three categories:

1. Spam. Structured data is not processed.

2. Ordinary. Structured data is not processed.

3. Trusted. Structured data is processed.

This section gives an overview of the trust mechanisms used to
differentiate between 2 and 3.

It does not attempt to describe whether a trust-based mechanism is
appopriate in a particular case.

## Processing structured data

MUAs SHOULD consider structured data in incoming email messages only
if either of these criteria hold:

* The sender is trusted (e.g., part of the user's address book) and
  the messsage either contains a valid personal or domain signature.

* The message is part of an ongoing thread with a trusted sender.

* The message's content indicate a connection with an older, trusted
  message. For example, if a calendar invitation was accepted, then
  updates or responses for the same event are connected with the
  original.

If none of these criteria is fulfilled, MUAs should fall back to
alternative presentations, typically "text/html".

Open issue 1: It's not clear that all types of structured data require
checking. Some may be 100% display-only. Is additional guidance for
display-only data worth the complexity?

Open issue 2: Really SHOULD? I (Arnt) don't see this guidance as
clearly right. It doesn't match existing code well, and doesn't seem
to be where we want to go.

## Inlining data

Structured data included in an email message SHOULD be self-contained
in order to avoid privacy problems.  This implies that if an MUA is
able to provide meaningful user interaction (rather than mere
display), then the data SHOULD be self-contained, such that the
interaction will not need referenced resources from the web.

# Security considerations

Security considerations are a core subject of this document.

# Privacy considerations

Privacy considerations are a core subject of this document.

# IANA Considerations

This document has no IANA actions at this time.

--- back

# Acknowledgements

The authors wish to thank Ben Bucksch, Alexey Melnikov, Phillip Tao,
Lisa Dusseault and others whose suggestions were made before this
paragraph was started.

# Notes to the RFC Editor

Please remove this section and everything after it.

# Note to self

The charter has this to say about what this document should contain:
"Recommendations for security and trust mechanisms that should be
applied when processing machine-readable content in email messages"
and "security and trust recommendations to prevent abuse of structured
email". No more, no less.
