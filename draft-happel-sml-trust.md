---
stand_alone: true
ipr: trust200902
cat: std
submissiontype: IETF
area: Applications
wg: sml

docname: draft-ietf-sml-trust

title: Structured Email: Trust and security considerations
abbrev: IMAP JMAPACCESS
lang: en
kw:
  - IMAP
  - JMAP
author:
- name: Arnt Gulbrandsen
  org: ICANN
  street: 6 Rond Point Schumann, Bd. 1
  city: Brussels
  code: 1040
  country: Belgium
  email: arnt@gulbrandsen.priv.no
  uri: https://icann.org/ua
- name: Hans-Joerg Happel
  org: audriga
  street:
  city:
  code:
  country:
  email: happel@audriga.com
  uri: https://www.audriga.com


normative:

informative:
  I-D.happel-structured-email-00

--- abstract

This document discusses trust and security considerations for
"structured email" ({{I-D.happel-structured-email-00}}) and provides
recommendations for message user agents on how to deal with structured
data in email messages.

--- middle

# Introduction

Structured email makes the content of some email messages
machine-readable, such that user agents can provide higher-level
functions than displaying/replying, for example "add this to
calendar".

Naturally, the new functions bring new trust and security
consideratons. This document discusses issues related to trust and
security of structured email, and provides advice in some cases.

# Requirements Language

{::boilerplate bcp14-tagged}

# Types of security concerns

This section gives an overview of the various types of security and
privacy concerns that arise when email messages contain structured
data.

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

# Content encryption mechanisms

# Trust mechanisms

Several implementations of structured email restrict processing to
messages that are particularly trusted. That is to say, an incoming
message is in one of these three categories:

1. Spam. Structured data is not processed.

2. Ordinary. Structured data is not processed.

3. Trusted. Structured data is processed.

This section gives an overview of the trust mechanisms used.

It does not attempt to describe whether a trust-based mechanism is
appopriate in a particular case.

## Trusted senders

For the case of displaying remote images embedded in HTML email
messages, MUAs often allow users to manually choose if they trust a
certain sender.

Sometimes, addresses in the MUA's address book are considered trusted,
or in a special list in the address book.

Besides mentions in related use cases ([@RFC6132]/[@RFC6134]) this
mechanism is currently not standardized.

Several services manage trust centrally: trusted senders are trusted
by the mail service rather than by the individual users.

## Sender signatures

## Domain signatures

DKIM defines a signature on sender domain that may be used to verify
that a message was sent by the same sender as an earlier message.

Google and friends currently have a scheme to display a logo for some
senders. Just a few. This logo effectively acts as a sender signature.

## Transaction identifiers

Part of the simplicity of email is the fact that just the email
address is required to reach out to a recipient. This however puts
burden on the recipient to discern if a message is legitimate or
abusive.

Recipient-generated transaction identifiers aim to pass a certain
secret to the sender, which helps to prove legitimacy.

One such approach are one-time email aliases, which are generated for
a single transaction or series of transactions.

Structured email by itself might also help define special types of
structured data that could help to manage and communicate transaction
identifiers more easily.

Open issue 1: Propose a header field with an opaque cookie like
thread-id, to tie a message to older messages?

Open issue 2:

# Categories of use cases

Certain types of structured data might need to be kept more secure
than others. For instance, the structured data representation of a
music album shared by a friend would not contain major personal
information, while e.g., medical records or financial statements
certainly would.

# Implementation guidelines

## Processing structured data

MUAs SHOULD consider structured data in incoming email messages only
if:
* The sender is trusted (e.g., part of the user's address book) and
the messsage either contains a valid personal or domain signature
* The message is part of an ongoing thread with a trusted sender

If none of those criteria is fulfilled, MUAs should fallback to
alternative presentations (e.g., "text/html" or "text/plain" of such
message).

	Open isues
	* Github

## Inlining data

Structured data included in an email message should be
self-contained. This means that a MUA should be able to provide
meaningful user interaction also without loading additional referenced
resources from the web.

	Open issues
	* Github

# Implementation status

< RFC Editor: before publication please remove this section and the
reference to [@RFC7942] >

This section records the status of known implementations of the
protocol defined by this specification at the time of posting of this
Internet-Draft, and is based on a proposal described in
[@RFC7942]. The description of implementations in this section is
intended to assist the IETF in its decision processes in progressing
drafts to RFCs. Please note that the listing of any individual
implementation here does not imply endorsement by the
IETF. Furthermore, no effort has been spent to verify the information
presented here that was supplied by IETF contributors. This is not
intended as, and must not be construed to be, a catalog of available
implementations or their features. Readers are advised to note that
other implementations may exist.

According to [@RFC7942], "this will allow reviewers and working groups
to assign due consideration to documents that have the benefit of
running code, which may serve as evidence of valuable experimentation
and feedback that have made the implemented protocols more mature. It
is up to the individual working groups to use this information as they
see fit".

## Structured Email plugin for Roundcube Webmail

The plugin will currently use the "trusted sender" feature of
Roundcube to determine if structured data should be processed.

# Security considerations

Security considerations are core subject of this document.

# Privacy considerations

Privacy considerations are core subject of this document.

# IANA Considerations

This document has no IANA actions at this time.

--- back
{backmatter}

<reference anchor="CalSpam" target="https://standards.calconnect.org/csd/cc-18003.html">
   <front>
      <title>Calendar operator practices — Guidelines to protect against calendar abuse (CC/R 18003:2019) </title>
      <author>
        <organization>The Calendaring and Scheduling Consortium (“CalConnect”)</organization>
      </author>
      <date>2019</date>
   </front>
</reference>


<reference anchor="MachineReadable" target="https://csrc.nist.gov/glossary/term/Machine_Readable">
   <front>
      <title>NIST IR 7511 Rev. 4</title>
      <author>
        <organization>NIST</organization>
      </author>
      <date>2016</date>
   </front>
</reference>
