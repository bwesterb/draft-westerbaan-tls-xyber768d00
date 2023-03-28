---
title: "X25519Kyber768Draft00 hybrid post-quantum key agreement"
abbrev: xyber768d00
category: info

docname: draft-tls-westerbaan-xyber768d00-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: AREA
# workgroup: WG Working Group
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
    fullname: Bas Westerbaan
    organization: Cloudflare
    email: bas@cloudflare.com

normative:
  rfc8037:
  kyber: I-D.cfrg-schwabe-kyber


informative:
  HYBRID: I-D.ietf-tls-hybrid-design
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

# Construction

We instantiate draft-ietf-tls-hybrid-design-06 with
    X25519 {{rfc8037}} and Kyber768Draft00 {{kyber}}.
The latter is Kyber as submitted
    to round 3 of the NIST PQC process {{KyberV302}}.

For the client's share,
 the key_exchange value contains
    the concatenation of the client's X25519 ephemeral share
    and the client's Kyber768Draft00 public key.

For the server's share,
 the key_exchange value contains
    the concatenation of the server's X25519 ephemeral share
    and the Kyber768Draft00 ciphertext returned
    from encapsulation for the client's public key.

The shared secret is calculated as the concatenation of
    the X25519 shared secret
    and the Kyber768Draft00 shared secret.

# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

This concatenation approach provides a secure key exchange if either
    component key exchange methods (X25519 or Kyber768Draft00)
    are secure {{HYBRID}}.


# IANA Considerations

This document requests/registers TLS Named Group 
 (or Supported Group), according to the procedures in
 {{Section 6 of TLS-REG=I-D.ietf-tls-rfc8447bis}}:
 
 Value:
 : 0xfe31 (please)
 Description:
 : Xyber768Draft00
 DTLS-OK:
 : Y
 Recommended:
 : N
 Reference:
 : This document
 Comment:
 : Pre-standards version of Kyber768

--- back

