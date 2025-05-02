+++
date = '2025-04-30T17:00:00+01:00'
title = 'Are DIDs a DUD?'
+++

*Before we get into it, I want to say that I was conflicted about publishing this opinion piece. Discussions involving DIDs can sometimes take on a quasi-religious fervour. In the end, I believe robust debate is essential to reach the best outcomes. With that out of the way, let's get into it.*

----


## Introduction

I remember the time, not that long ago, when a [DID (Decentralized Identifier)](https://www.w3.org/TR/did-1.0/) was all the rage. Everyone interested in digital identity seemed to be talking about them, and I got lots of questions — from both technical and non-technical people — asking if we "do DIDs". We subsequently had to spend many hours explaining this inherently boring technical concept to a wide audience. In part, this was because some people thought that "digital identity" *was* DID. If you think about it, even the acronym acts as a doppelgänger for both terms (Digital Identity’s acronym is DI). Slowly, people caught on to the idea that it’s "just" some sort of pointer (with data attached to it) — and no, not every company needs to create "their DID method" as if it were a fashion item.

## Understanding DIDs

What is a "DID method"? For the uninitiated or just as a general reminder, below is an example of an actual DID

```
did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ
```

Not exactly something that you spell out to someone on the phone (not that that was ever a design goal of these identifiers). Still, every DID consists of three parts, separated by colons.

```
DID:METHOD:METHOD_SPECIFIC_IDENTIFIER
```

Every DID can be resolved to a DID document, a structured JSON object that contains extra information related to the DID. At minimum, this information contains the public part of an asymmetric cryptographic key that can be used in a challenge-response interaction to prove control over the keypair and as such the DID.

Below is an example of the DID document attached to the aforementioned DID.

```
{
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ",
    "verificationMethod": [
      {
        "id": "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ",
        "type": "JsonWebKey2020",
        "controller": "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ",
        "publicKeyJwk": {
          "crv": "P-256",
          "kty": "EC",
          "x": "uMzxGUb1Upi8gCPYtGuVqTsV7zayZKLcsTGy8TW6y2U",
          "y": "udvqG3hMatMn-9b_wlMSkC2WDw4VZ1xjr2OoqxvXjIk"
        }
      }
    ],
    "authentication": [
      "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ"
    ],
    "assertionMethod": [
      "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ"
    ],
    "capabilityInvocation": [
      "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ"
    ],
    "capabilityDelegation": [
      "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ"
    ]
  },
  "didDocumentMetadata": {},
  "didResolutionMetadata": {
    "contentType": "application/did+ld+json"
  }
}
```

The method-specific identifier either directly encodes this information — meaning it is self-contained (as with `did:key`) — or it is a reference to a location on a third-party service where the information can be found. The details of how this works are beyond the scope of this article, but what’s important to understand is that at its core, a DID is a key resolution mechanism. It’s a way to convey cryptographic keys. This will be important later.

## The Blockchain Era

Because the emergence of DIDs coincided with the blockchain hype, there was an explosion of DID methods that were “on-chain”. The promise that came with blockchain (and later the broader category of DLT) was that people would be able to “own” their identity and that nobody could take it away. It took some time, but eventually people realised that blockchain — especially public blockchains — and privacy are a *contradictio in terminis*. For 99.99% of use cases, there just isn’t a compelling reason to consider it. On top of that, interactions with DLTs are non-trivial and usually involve costs to be paid in the native DLT currency. This means businesses must convert real-world money (e.g. EUR) into something else, introducing additional risk. When scaled to millions of users, these costs can be eye-watering. As it turns out, businesses don’t like paying for this.

## Alternatives That Just Work

Now that we understand what a DID is, here are two DID methods that are widely supported by different vendors:

- `did:key`
- `did:web`

The first has the public key encoded in the method-specific string sequence (*), the second points to a URL where the DID document is hosted.

*Now… wait… a… minute*

This is a solved problem.

JWTs, for example, already have standardised ways to either directly include keys or reference them. Issuers — the entities creating credentials in JSON format — use the protected header to include a key or provide a hint in the `iss` claim that allows verifiers to resolve keys at predefined web addresses (e.g. at `.well-known` locations).

For cryptographic key binding with credentials, we can use [Proof-of-Possession Key Semantics for JWTs](https://datatracker.ietf.org/doc/html/rfc7800), abbreviated as DPoP. This allows direct inclusion of the key in [JWK format](https://datatracker.ietf.org/doc/html/rfc7517), a well-understood, easy-to-grasp, and widely supported construct in many programming languages.

DID methods introduce yet another layer of abstraction, with far too many implementation options, ultimately to achieve the same result.

Now, when it comes to `did:web`, instead of managing this abstraction layer, it's far easier — from both a comprehension and implementation perspective — to simply use a [Well-Known URI](https://datatracker.ietf.org/doc/html/rfc8615). This allows organisations to publish their keys and related metadata via a web service they already operate. OAuth 2.0, OpenID Connect, and OpenID4VC specifications all make extensive use of this. It’s easy to set up and use — you just have to host the file at a given location. All parties simply need to agree on the name and format of the document, which can now be made use-case-specific.

`did:web` essentially replicates this model, but adds additional constraints and complexity. Rather than using well-known paths, it encodes the domain name in the identifier string (e.g. `did:web:example.com`) and requires the DID document to live at a derived path. This indirection adds little benefit and makes implementations more fragile — especially when systems already rely on HTTPS, DNS, and existing PKI trust anchors. In practice, it often feels like a verbose way to achieve what `.well-known` already does, but with more moving parts.

Another way organisations can manage key resolution is by using X.509 certificates. I know, they’re old ([first published in 1988](https://www.itu.int/rec/T-REC-X.509-198811-S)), but they are robust, widely supported, and well understood. Crucially, they leverage the existing global PKI infrastructure, which is already trusted by browsers, operating systems, and mobile devices.

An underappreciated feature is that entire certificate chains can be embedded directly in the protected headers of credentials. This removes the need for verifiers to fetch anything from an external service — a requirement common in other mechanisms. The benefits are twofold: it preserves privacy by avoiding "phone-home" lookups, and it supports offline or air-gapped verification scenarios. These are particularly valuable for high-assurance, decentralised, or intermittently connected environments — like mobile wallets, border control, or secure corporate networks.

That said, there may be niche scenarios where DIDs offer value. In peer-to-peer systems without centralised DNS infrastructure, or in IoT environments where identifiers need to be portable and independent of a single domain, DIDs can help establish relationships without relying on global registries or where the current technology may fall short. I believe these are specialised cases, and they don’t reflect the majority of mainstream digital identity needs, often involving people and businesses. But in these niche contexts, DIDs may still serve a purpose.

## Conclusion

So, are DIDs a dud? From personal experience building digital identity systems over the years — and from speaking to others in the space — I’ve come to the conclusion that DIDs are an unnecessary technical construct. They are difficult to implement correctly, hampered by choice paralysis (see the [ever-increasing list of DID methods](https://www.w3.org/TR/did-extensions-methods/)), and lead to increased costs and lower levels of interoperability. They try to be too many things at once but fail to solve the core problem elegantly. As so often, history doesn't repeat itself, but it often rhymes. [XML Signatures had the same issues](https://ssoready.com/blog/engineering/xml-dsig-is-unfortunate/). In the end, implementors seem to gravitate towards a small subset of the overall specification, do away with all of the fluff. 

Mike Jones, in his [2024 EIC keynote](https://www.kuppingercole.com/sessions/5499/1) said it best, standards are about making choices, and DID is all about giving options. But, as always, I keep an open mind and welcome other perspectives.

(*) A side rant: even when two parties claim to support `did:key`, it doesn’t necessarily mean they will interoperate. That’s because `did:key` relies on [multiformats](https://multiformats.io/) — a suite of specifications that allow the same key material to be encoded in different ways (e.g. multibase, multicodec). This flexibility, while elegant in peer-to-peer systems like IPFS, introduces ambiguity in implementations. It adds extra complexity for developers, requires specialised tooling, and means that even simple key resolution can fail without tight coordination. This again shows that too many choices aren’t always a good thing — and it’s yet another hurdle to understand, implement, and support correctly.
