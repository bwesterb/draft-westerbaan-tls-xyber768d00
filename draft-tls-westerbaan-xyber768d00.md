---
title: "X25519Kyber768Draft00 hybrid post-quantum key agreement"
abbrev: xyber768d00
category: info

docname: draft-tls-westerbaan-xyber768d00-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
stand_alone: yes
consensus: true
v: 3
# area: AREA
workgroup: None
keyword:
 - kyber
 - x25519
 - post-quantum
venue:
  group: TLS
  type: Working Group
#  mail: WG@example.com TODO
#  arch: https://example.com/WG
  github: "bwesterb/draft-westerbaan-tls-xyber768d00"
  latest: "https://bwesterb.github.io/draft-westerbaan-tls-xyber768d00/draft-tls-westerbaan-xyber768d00.html"

author:
 -
    ins: B.E. Westerbaan
    fullname: Bas Westerbaan
    organization: Cloudflare
    email: bas@cloudflare.com
 -
    fullname: Douglas Stebila
    organization: University of Waterloo
    email: dstebila@uwaterloo.ca

normative:
  rfc7748:
  kyber: I-D.cfrg-schwabe-kyber


informative:
  hybrid: I-D.ietf-tls-hybrid-design
  tlsiana: I-D.ietf-tls-rfc8447bis
  hpkexyber: I-D.westerbaan-cfrg-hpke-xyber768d00
  KyberV302:
    target: https://pq-crystals.org/kyber/data/kyber-specification-round3-20210804.pdf
    title: CRYSTALS-Kyber, Algorithm Specification And Supporting Documentation (version 3.02)
    author:
      -
        ins: R. Avanzi
      -
        ins: J. Bos
      -
        ins: L. Ducas
      -
        ins: E. Kiltz
      -
        ins: T. Lepoint
      -
        ins: V. Lyubashevsky
      -
        ins: J. Schanck
      -
        ins: P. Schwabe
      -
        ins: G. Seiler
      -
        ins: D. Stehle # TODO unicode in references
    date: 2021
    format:
      PDF: https://pq-crystals.org/kyber/data/kyber-specification-round3-20210804.pdf


--- abstract

This memo defines X25519Kyber768Draft00, a hybrid post-quantum key exchange
    for TLS 1.3.


--- middle

# Introduction

## Motivation

The final draft for Kyber is expected in 2024.
There are already early deployments of post-quantum key agreement,
    with more to come before Kyber is standardised.
To promote interoperability of early implementations,
    this document specifies a preliminary hybrid post-quantum key agreement.

## Warning: relation with X25519Kyber768Draft00 for HPKE

In {{hpkexyber}} a hybrid KEM with the same name is defined
for use in HPKE. It differs from the hybrid KEM implicit in
this document: here we use the X25519 shared secret directly,
whereas in {{hpkexyber}}, the ephemeral X25519 public key
(ciphertext) is mixed in.
For use in HPKE this is required to be IND-CCA2 robust.
This is not required for use in TLS 1.3, thanks to
the inclusion of the keyshare in the message transcript.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Construction

We instantiate draft-ietf-tls-hybrid-design-06 with
    X25519 {{rfc7748}} and Kyber768Draft00 {{kyber}}.
The latter is Kyber as submitted
    to round 3 of the NIST PQC process {{KyberV302}}.

For the client's share,
 the key_exchange value contains
    the concatenation of the client's X25519 ephemeral share (32 bytes)
    and the client's Kyber768Draft00 public key (1184 bytes).
    The resulting key_exchange value is 1216 bytes in length.

For the server's share,
 the key_exchange value contains
    the concatenation of the server's X25519 ephemeral share (32 bytes)
    and the Kyber768Draft00 ciphertext (1088 bytes) returned
    from encapsulation for the client's public key.
    The resulting key_exchange value is 1120 bytes in length.

The shared secret is calculated as the concatenation of
    the X25519 shared secret (32 bytes)
    and the Kyber768Draft00 shared secret (32 bytes).
    The resulting shared secret value is 64 bytes in length.



# Security Considerations

For TLS 1.3, this concatenation approach provides a secure key
    exchange if either component key exchange methods (X25519
    or Kyber768Draft00) are secure {{hybrid}}.


# IANA Considerations

This document requests/registers a new entry to the TLS Named Group
 (or Supported Group) registry, according to the procedures in
 {{Section 6 of tlsiana}}.

 Value:
 : 0x6399 (please)

 Description:
 : X25519Kyber768Draft00

 DTLS-OK:
 : Y

 Recommended:
 : N

 Reference:
 : This document

 Comment:
 : Pre-standards version of Kyber768

--- back

# Change log

> **RFC Editor's Note:** Please remove this section prior to publication of a
> final version of this document.

## Since draft-tls-westerbaan-xyber768d00-02

- Explain relation with HPKE hybrid

## Since draft-tls-westerbaan-xyber768d00-01

- Change reference for X25519

## Since draft-tls-westerbaan-xyber768d00-00

- Set working group to None.
- Bump to cfrg-schwabe-kyber-02
