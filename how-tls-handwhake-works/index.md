---
date: 2025-07-24
title: How TLS Handshake works?
description: tls handshake
tags:
  - dev/network
posted:
slug: how-tls-handshake-works
author:
  - realbro
---

The goal of SSL/TLS is to achieve secure *symmetric* encryption, while it uses asymmetric encryption during TLS handshake.

(more: why use symmetric encryption at result?)

TLS handshake happens after [TCP handshake](https://developer.mozilla.org/en-US/docs/Glossary/TCP_handshake), and here is step by step:

![TLS handshake - cloudflare.com](tls_handshake_diagram.webp)

1. Client Hello
    1. send supported TLS version, cypher suites, and "client random" string.
2. Server Hello
    1. send chosen TLS version, cypher suites, "server random" string, and SSL Certificate.
3. Certificate Authentication at Client
    1. client authenticate server using SSL certificate given. 
4. Client sends Premaster Secret to Server
    1. encrypts premaster secret using public key in SSL certificate.
5. both generate session key using 1. client random, 2. server random, and 3. premaster secret
    1. client has all three
    2. server has two, client random and server random, and decrypts premaster secret using private key of certificate.
6. Client Ready
7. Server Ready
8. Secure Symmetric Encryption Achieved.

Make sure that this is not all cases of TLS handshake, only RSA key exchange algorithm, which is not supported TLSv1.3, considered not secure.

(more: why RSA algorithm is considered to be not secure?)

### Reference

[What happens in a TLS handshake? | SSL handshake | Cloudflare](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)
