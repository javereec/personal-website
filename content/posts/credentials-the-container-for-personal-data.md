+++
date = '2025-03-14T17:00:00+01:00'
draft = false
title = 'Credentials - The Container for Personal Data'
+++

In this third part of our series on the universal model for personal data exchange, we examine credentials —the container for the data being transferred over the rails. We're going to dive deep into the technical details and absolutely nerd out, but this article is written to be accessible to a broad audience of tech-savvy readers, helping them form a clearer picture of what these credential formats look like.

You can find the previous installations of this series here:

* [universal model for personal data exchange]({{< ref "towards-a-universal-model-for-personal-data-exchange" >}})
* [laying the rails for personal data exchange]({{< ref "laying-the-rails-for-personal-data-exchange" >}})

## Credentials

Let's start with a definition of credentials. Rather than redefining it, I'll use the succinct definition given in the [OpenID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) specification:

> A set of one or more claims about a subject made by a Credential Issuer.

The credential is expressed in a credential format defined by the issuer. Credential formats have different security mechanisms and ways to achieve holder binding, the most common being cryptographic key binding. This involves including the public part of a keypair in the credential, that is subsequently signed by the issuer's private key.

There are multiple ways to represent credentials, each with trade-offs. The table below summarises key standards based credential formats that are either in production or have a clear roadmap for large-scale adoption.

|                        | JWT       | SD-JWT    | JWT VC    | mdoc      |
| ---------------------- | --------- | --------- | --------- | --------- |
| Format                 | JSON      | JSON      | JSON      | CBOR      |
| Encoding               | Base64URL | Base64URL | Base64URL | Binary    |
| Size                   | Medium    | Large     | Large     | Small     |
| Issuer-Holder-Verifier | No        | Yes       | Yes       | Yes       |
| Adoption               | High      | Low       | Low       | Low       |
| Use Case               | Online    | Online    | Online    | Proximity |
| Specification          | Public    | Public    | Public    | Paywall   |

Two major categories emerge:

1. **JSON-based formats**: Common in OpenID-based systems due to their simplicity and wide developer support.
2. **CBOR-based formats**: Optimised for offline use cases, such as presenting mDLs over NFC or Bluetooth.

JSON formats are developer-friendly because they are both human-readable and machine-parsable. As a result, JSON is ubiquitous on the web. On the other hand, CBOR's binary format has a smaller footprint, making it more suitable for bandwidth-constrained environments (such as proximity use cases). However, CBOR is harder to work with and lacks robust developer tooling support.

In the next sections, we'll take a closer look at each of these formats.

### JSON Web Token (JWT)

Specified in [RFC7519](https://datatracker.ietf.org/doc/html/rfc7519), each JWT consists of a header and a payload, separated by a dot. The payload contains the claims that make up the credential. A JWT can be secured using signatures as described in JSON Web Signatures [RFC 7515](https://datatracker.ietf.org/doc/html/rfc7515), and encrypted as described in JSON Web Encryption [RFC516](https://datatracker.ietf.org/doc/html/rfc7516).

Below is an example of a JWT secured with JWS.

```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IkRNMnpqSVVtVXJ5
SHNVeGxVYkVXZkJySUt5eWJGZVR6T0w4VFF2OU1uVkkifQ.eyJzdWIiOiJjO
WFkMzRlNC1mODZhLTQ4NTAtOTQ1Ny02NjE1NzRmMTg5YTUiLCJmYW1pbHlfb
mFtZSI6IlZlcmVlY2tlbiIsImdpdmVuX25hbWUiOiJKYW4iLCJuYW1lIjoiS
mFuIFZlcmVlY2tlbiIsInVwZGF0ZWRfYXQiOiIyMDIzLTA5LTA2VDE4OjI2O
jE4LjQ0MloiLCJlbWFpbCI6Imphbi52ZXJlZWNrZW5AbWVlY28ubWUiLCJlb
WFpbF92ZXJpZmllZCI6ZmFsc2UsImF0X2hhc2giOiJxZV9lQklvODN3aHhYZ
EsxTC1JVE1RIiwiYXVkIjoiZGQyNTY4NmMtZTBlMi00MmQ0LWJhOTItZjBlZ
jkzMWU4YjQ4IiwiZXhwIjoxNzQwNTc4MzQ4LCJpYXQiOjE3NDA1NzQ3NDgsI
mlzcyI6Imh0dHBzOi8vbG9naW4tZGV2LnNlY3VyZXZhbHVlZXhjaGFuZ2UuY
29tL29hdXRoMiJ9.jsapazj6V7AAgzYZylbsHxsDjaIZ1fPRbwXX98k0Xqgl
zyovLEB-feIJHNjtDmQAhBjCeg9OwGz0FSLeyX8r5NQqnf82OaTROJiG-0g8
8pjfyPgnK5Chd_MPfupOwn-CYPNAI9piLWSE11saiZ74x-_Q38knRRvyWs1z
xEm-LxmyQAH9smPcUK072o54kbUQs02OmWDSQ341btZ6o4Ca77MCstPpxJYU
1qdOwiGWHg9D64ect33rZ0D0t2pMN6ID6-75G01a1sJS-nL5gDWht7g0axxv
LfMZCvtHj7z5kFHwhlYFebwnHXNv7IlfkuRdourUqXkVfRTqR7Xw0AGy8A
```

The structure of a signature secured JWT is

```
header.payload.signature
```

The encoding applied in the above JWT is Base64URL. If we decode and format the header and payload, we can see the information stored within the JWT. This structure is intuitive to read (aside from some abbreviations), which is a major reason why developers enjoy working with it.


```
{
  "alg": "RS256",
  "typ": "JWT",
  "kid": "DM2zjIUmUryHsUxlUbEWfBrIKyybFeTzOL8TQv9MnVI"
}
{
  "sub": "c9ad34e4-f86a-4850-9457-661574f189a5",
  "family_name": "Vereecken",
  "given_name": "Jan",
  "name": "Jan Vereecken",
  "updated_at": "2023-09-06T18:26:18.442Z",
  "email": "jan.vereecken@meeco.me",
  "email_verified": false,
  "at_hash": "qe_eBIo83whxXdK1L-ITMQ",
  "aud": "dd25686c-e0e2-42d4-ba92-f0ef931e8b48",
  "exp": 1740578348,
  "iat": 1740574748,
  "iss": "https://login-dev.securevalueexchange.com/oauth2"
}
```

JWTs do not natively support the Issuer-Holder-Verifier model because a plain JWT, as shown above, isn't "bound" to a specific holder. Extensions such as Demonstrating Proof of Possession (DPoP) can be used to address this, but other credential formats have well-defined semantics for holder binding. In practice, plain JWTs are commonly used in two party models such as Issuer-Verifier and Holder-Verifier models, but not in three party models such as the Issuer-Holder-Verifier model.

### SD-JWT

JWT credentials lack a mechanism that allows a holder to selectively disclose claims from a signed JWT. To address this, the IETF has drafted Selective Disclosure for JWTs, also known as SD-JWT. This credential format uses hashes instead of claims in the JWT payload (for claims requiring selective disclosure) and places the disclosures in a new element.

The new structure for a signature-secured SD-JWT is:

```
header.payload.signature.disclosures
```

Since the disclosures are not secured, the holder can choose which ones to include when presenting the credential to a verifier.

Belw is an example of a Base64URL encoded SD-JWT.

```
eyJ0eXAiOiJ2YytzZC1qd3QiLCJhbGciOiJFUzI1NiIsImtpZCI6Im03SzFO
aVduWUE4bWNUUDR2OERVZDN1dVBoaWFvcnJ2a1hHb0JHQ0RNNWMiLCJ4NWMi
OlsiTUlJQjdUQ0NBWk9nQXdJQkFnSVVhRG8xZmxNMHpwQ1BaR3lkQ09UeEhu
di9kK3N3Q2dZSUtvWkl6ajBFQXdJd1RERXRNQ3NHQTFVRUF3d2tiM0puWVc1
cGMyRjBhVzl1TFhkaGJHeGxkQzFrWlhZdWMzWjRMbWx1ZEdWeWJtRnNNUXN3
Q1FZRFZRUUdFd0pCVlRFT01Bd0dBMVVFQ2d3RlRXVmxZMjh3SGhjTk1qVXdN
VE13TVRrMU5qTTJXaGNOTWpZd01UTXdNVGsxTmpNMldqQk1NUzB3S3dZRFZR
UUREQ1J2Y21kaGJtbHpZWFJwYjI0dGQyRnNiR1YwTFdSbGRpNXpkbmd1YVc1
MFpYSnVZV3d4Q3pBSkJnTlZCQVlUQWtGVk1RNHdEQVlEVlFRS0RBVk5aV1Zq
YnpCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUhBMElBQkk1LzZpVGVh
VVR2bXNnRlN5RmMwVUt5ZzZmOEpiTzZoUERaN1B3RHh2SUlmWWJMem5QZkxK
WTBKM0NBK1lOYlo5TEVDbXk1QkVLbUpwL2k1WGhpRWpHalV6QlJNQjBHQTFV
ZERnUVdCQlIydGpuUnBmRHl2aVFaMXllenpiNjQ1Yit0UERBZkJnTlZIU01F
R0RBV2dCUjJ0am5ScGZEeXZpUVoxeWV6emI2NDViK3RQREFQQmdOVkhSTUJB
ZjhFQlRBREFRSC9NQW9HQ0NxR1NNNDlCQU1DQTBnQU1FVUNJUUQxcVlGaHli
Y1F3SU82V0dvRUpMRkNsSFkvcm9Dck1rdSsvZ2VWa09RSGVBSWdLUWNteE94
UzNTbDJScUdkWGdWVE9QQm9aaTMxT1RIUFd2U05JR3N1QVl3PSJdfQ.eyJpc
3MiOiJodHRwczovL2R2Zm16Mm9oLWRldi5tZWVjby5jbG91ZCIsImp0aSI6I
nVybjp1dWlkOjMxZGE1ZTcyLTA1ODQtNDRmMy04MjlmLTg4YTJkMDA4ZTNlN
iIsInZjdCI6IkdBSU5QT0NTaW1wbGVJZGVudGl0eUNyZWRlbnRpYWwiLCJjb
mYiOnsiandrIjp7ImFsZyI6IkVTMjU2IiwiY3J2IjoiUC0yNTYiLCJrdHkiO
iJFQyIsInVzZSI6InNpZyIsIngiOiJ1TXp4R1ViMVVwaThnQ1BZdEd1VnFUc
1Y3emF5WktMY3NUR3k4VFc2eTJVIiwieSI6InVkdnFHM2hNYXRNbi05Yl93b
E1Ta0MyV0R3NFZaMXhqcjJPb3F4dlhqSWsiLCJraWQiOiJkaWQ6a2V5OnoyZ
G16RDgxY2dQeDhWa2k3SmJ1dU1tRllyV1BnWW95dHlrVVozZXlxaHQxajlLY
nNtbjU3ZUxSMXkxb2Y4RmdxdXdjZXNheW9meHNCaWlzS2hVd1RyQlJjZDhYQ
nlMVHgzMVY5QmtoeUpWYTczS3h0UVJteXRSd3Y1OFBqS2ozblQ0VnBtUWtYY
nVjd3NrWU45YXFxS1N6aTlyWDVGS2VUWHVRRWd6RlN3aE12VGl1Y1EjejJkb
XpEODFjZ1B4OFZraTdKYnV1TW1GWXJXUGdZb3l0eWtVWjNleXFodDFqOUtic
21uNTdlTFIxeTFvZjhGZ3F1d2Nlc2F5b2Z4c0JpaXNLaFV3VHJCUmNkOFhCe
UxUeDMxVjlCa2h5SlZhNzNLeHRRUm15dFJ3djU4UGpLajNuVDRWcG1Ra1hid
WN3c2tZTjlhcXFLU3ppOXJYNUZLZVRYdVFFZ3pGU3doTXZUaXVjUSJ9fSwia
WF0IjoxNzQwNTc2ODAzLCJuYmYiOjE3NDA1NzY4MDMsIl9zZCI6WyJBc2tpV
W9ZRWszeDJ4cWI1NVdkdmF2MWhwT3JjQm1xOTJPazgtbERQVzhnIiwiX3NPS
zdqWEdaYUtUVnlJRFZHdkpINHkxUG9iemtSWEQwd3AwVDdVbEtzTSIsImFie
FY5T0NEajg1dXFsRGQ5RXlwZUhqdmp2VTF2SVpQbGxqSFVTQkNOMlUiLCJwW
EpMLTJ1MHlzdjdHNmY0Q0FxUmVwYWRkOXBoQkVhVjBKaXJkRzRZOEVZIiwie
HlvSkZSdUVSRHF5VnR1c2N1Z25LWXJORGFRVTVyQTlkcE90Z2w2dTJGdyJdL
CJfc2RfYWxnIjoic2hhLTI1NiJ9.mlq0gXoxPdaG7-VzVZs7IQzBnCrjf3Af
i0GBPM6b-nSjvLY3_wzCs3V57xCitANxs_CujxxM6OaTyad1WqrjiA~WyJ2b
1g0aG9jQzJZMzc5UDdqIiwiZ2l2ZW5fbmFtZSIsIkphbiJd~WyJtSTRwMnJ2
eUJQak5OdllTIiwiZmFtaWx5X25hbWUiLCJWZXJlZWNrZW4iXQ~WyJndkFKS
XpEMFR6OTZuQmZmIiwiYmlydGhkYXRlIiwiMTk2MC0wMS0yNCJd~
```

After decoding and formatting, the header and payload look as follows.

```
{
  "typ": "vc+sd-jwt",
  "alg": "ES256",
  "kid": "m7K1NiWnYA8mcTP4v8DUd3uuPhiaorrvkXGoBGCDM5c",
  "x5c": [
    "MIIB7TCCAZOgAwIBAgIUaDo1flM0zpCPZGydCOTxHnv/d+swCgYIKoZIzj0EAwIwTDEtMCsGA1UEAwwkb3JnYW5pc2F0aW9uLXdhbGxldC1kZXYuc3Z4LmludGVybmFsMQswCQYDVQQGEwJBVTEOMAwGA1UECgwFTWVlY28wHhcNMjUwMTMwMTk1NjM2WhcNMjYwMTMwMTk1NjM2WjBMMS0wKwYDVQQDDCRvcmdhbmlzYXRpb24td2FsbGV0LWRldi5zdnguaW50ZXJuYWwxCzAJBgNVBAYTAkFVMQ4wDAYDVQQKDAVNZWVjbzBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IABI5/6iTeaUTvmsgFSyFc0UKyg6f8JbO6hPDZ7PwDxvIIfYbLznPfLJY0J3CA+YNbZ9LECmy5BEKmJp/i5XhiEjGjUzBRMB0GA1UdDgQWBBR2tjnRpfDyviQZ1yezzb645b+tPDAfBgNVHSMEGDAWgBR2tjnRpfDyviQZ1yezzb645b+tPDAPBgNVHRMBAf8EBTADAQH/MAoGCCqGSM49BAMCA0gAMEUCIQD1qYFhybcQwIO6WGoEJLFClHY/roCrMku+/geVkOQHeAIgKQcmxOxS3Sl2RqGdXgVTOPBoZi31OTHPWvSNIGsuAYw="
  ]
}
{
  "iss": "https://dvfmz2oh-dev.meeco.cloud",
  "jti": "urn:uuid:31da5e72-0584-44f3-829f-88a2d008e3e6",
  "vct": "GAINPOCSimpleIdentityCredential",
  "cnf": {
    "jwk": {
      "alg": "ES256",
      "crv": "P-256",
      "kty": "EC",
      "use": "sig",
      "x": "uMzxGUb1Upi8gCPYtGuVqTsV7zayZKLcsTGy8TW6y2U",
      "y": "udvqG3hMatMn-9b_wlMSkC2WDw4VZ1xjr2OoqxvXjIk",
      "kid": "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ"
    }
  },
  "iat": 1740576803,
  "nbf": 1740576803,
  "_sd": [
    "AskiUoYEk3x2xqb55Wdvav1hpOrcBmq92Ok8-lDPW8g",
    "_sOK7jXGZaKTVyIDVGvJH4y1PobzkRXD0wp0T7UlKsM",
    "abxV9OCDj85uqlDd9EypeHjvjvU1vIZPlljHUSBCN2U",
    "pXJL-2u0ysv7G6f4CAqRepadd9phBEaV0JirdG4Y8EY",
    "xyoJFRuERDqyVtuscugnKYrNDaQU5rA9dpOtgl6u2Fw"
  ],
  "_sd_alg": "sha-256"
}
```

As you can see above, some of the claims are replaced by hashes. These link to the disclosures at the end of the file. Below you can find a mapping between the hashes and the disclosures:

```
abxV9OCDj85uqlDd9EypeHjvjvU1vIZPlljHUSBCN2U => ["voX4hocC2Y379P7j","given_name","Jan"]
_sOK7jXGZaKTVyIDVGvJH4y1PobzkRXD0wp0T7UlKsM => ["mI4p2rvyBPjNNvYS","family_name","Vereecken"]
pXJL-2u0ysv7G6f4CAqRepadd9phBEaV0JirdG4Y8EY => ["gvAJIzD0Tz96nBff","birthdate","1960-01-24"]
```

Adding this feature to the base JWT as such gives you the option to select what to disclose (a very effective tool for better privacy control), but comes at the cost of making the whole structure bigger and more complex to work with.

### JWT-VC

This credential format is defined in [W3C’s Verifiable Credential Data Model v1.1](https://www.w3.org/TR/2022/REC-vc-data-model-20220303), commonly referred to as W3C VCDM. For some, this is even synonymous for verifiable credential, but we've seen that that can be defined more generally. For all intents and purposes, it is a standard JWT with a well-defined structure.


In the example below, we provide an example of a JWT-VC that is bound to a holder using the `cnf` claim and leave the `credentialSubject.id` claim undefined.

```
{
  "alg": "ES256",
  "kid": "m7K1NiWnYA8mcTP4v8DUd3uuPhiaorrvkXGoBGCDM5c",
  "typ": "JWT"
}
{
  "cnf": {
    "jwk": {
      "alg": "ES256",
      "crv": "P-256",
      "kid": "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ",
      "kty": "EC",
      "use": "sig",
      "x": "uMzxGUb1Upi8gCPYtGuVqTsV7zayZKLcsTGy8TW6y2U",
      "y": "udvqG3hMatMn-9b_wlMSkC2WDw4VZ1xjr2OoqxvXjIk"
    }
  },
  "iat": 1740577439,
  "id": "urn:uuid:b18106e6-9067-41de-85a2-7ab4a28ff611",
  "iss": "https://dvfmz2oh-dev.meeco.cloud",
  "nbf": 1740577439,
  "vc": {
    "@context": [
      "https://www.w3.org/2018/credentials/v1"
    ],
    "credentialStatus": {
      "id": "https://api-dev.svx.exchange/status_list/2#443",
      "statusListCredential": "https://api-dev.svx.exchange/status_list/2",
      "statusListIndex": 443,
      "statusPurpose": "revocation",
      "type": "BitstringStatusListEntry"
    },
    "credentialSubject": {
      "birthdate": "1960-01-24",
      "family_name": "Vereecken",
      "given_name": "Jan"
    },
    "id": "urn:uuid:b18106e6-9067-41de-85a2-7ab4a28ff611",
    "issuanceDate": "2025-02-26T13:43:59Z",
    "issuer": {
      "id": "https://dvfmz2oh-dev.meeco.cloud",
      "name": "Meeco CI Test Org"
    },
    "type": [
      "VerifiableCredential",
      "GAINPOCSimpleIdentityCredential"
    ]
  }
}
```

One notable aspect of JWT-VCs is the duplication of certain information. For example, `sub` and `vc.id`, as well as `iss` and `vc.issuer.id`, contain overlapping data. This duplication exists because the VCDM is designed to accommodate multiple credential formats, not just JWTs. As a result, JWT-VCs tend to have a larger footprint compared to regular JWTs.

This duplication is being addressed in newer versions of [VCDM 2.0](https://www.w3.org/TR/vc-data-model-2.0/) and related specification [Securing Verifiable Credentials using JOSE and COSE](https://www.w3.org/TR/vc-jose-cose/).

Another features of this credential format is that it supports the Issuer-Holder-Verifier model, allowing presentations of a credential to a third party (i.e. verifier).

Traditionally, it achieves this goal via the `credentialSubject.id` claim. You'll see that a DID is used in a lot of examples and implementations to achieve this binding to a holder. While a DID is by no means needed (some people still seem to think that VCs and DIDs are linked together), I believe the semantics of the `credentialSubject.id` claim is not well defined.

The above JWT-VC also features the `cnf` claim that is used to contain the holder's cryptographic key. This can be used later in the presentation process to validate the signature added by the holder. The presentation has a separate structure.

### mdoc

Mobile documents (mdoc) are defined in [ISO/IEC 18013-5](https://www.iso.org/standard/69084.html). They are being popularised by the adoption of Mobile Driving Licenses (mDL), but they can also be used other types of credentials.

Mdoc uses [CBOR](https://datatracker.ietf.org/doc/html/rfc8949) encoding, which is a compact binary format. Since binary formats are not human-readable, the CBOR specification includes a diagnostic notation to facilitate discussion and debugging.

A Base64url encoded mdoc as issued via OpenID4VCI rails look like this:

```
o2d2ZXJzaW9uYzEuMGlkb2N1bWVudHOBomdkb2NUeXBlcXNjdGVzdDIxMDIy
NS5tZG9jbGlzc3VlclNpZ25lZKJqbmFtZVNwYWNlc6F4HHRlc3QubXNvX21k
b2MuYmFzaWNfb3B0aW9uYWyD2BhYZKRoZGlnZXN0SUQAcWVsZW1lbnRJZGVu
dGlmaWVyamdpdmVuX25hbWVsZWxlbWVudFZhbHVlZWFsaWNlZnJhbmRvbVgg
9uMhcP9TvDAqLRmPLDY8Dhq53Azg0Xv7doHJhRiTy5XYGFhjpGhkaWdlc3RJ
RAFxZWxlbWVudElkZW50aWZpZXJrZmFtaWx5X25hbWVsZWxlbWVudFZhbHVl
Y0RvZWZyYW5kb21YIH2lvk4MbxFN2JSJRm9XsnKAloNhfBJCIyLO0G7adn_j
2BhYbKRoZGlnZXN0SUQCcWVsZW1lbnRJZGVudGlmaWVyamJpcnRoX2RhdGVs
ZWxlbWVudFZhbHVl2QPsajIwMDAtMDEtMjVmcmFuZG9tWCD4NuXQUctuAAk_
MALSeImEeq_Q6G4Xfm2dI3M6Zz00eWppc3N1ZXJBdXRohEOhASaiBFgrbTdL
MU5pV25ZQThtY1RQNHY4RFVkM3V1UGhpYW9ycnZrWEdvQkdDRE01YxghgVkB
8TCCAe0wggGToAMCAQICFGg6NX5TNM6Qj2RsnQjk8R57_3frMAoGCCqGSM49
BAMCMEwxLTArBgNVBAMMJG9yZ2FuaXNhdGlvbi13YWxsZXQtZGV2LnN2eC5p
bnRlcm5hbDELMAkGA1UEBhMCQVUxDjAMBgNVBAoMBU1lZWNvMB4XDTI1MDEz
MDE5NTYzNloXDTI2MDEzMDE5NTYzNlowTDEtMCsGA1UEAwwkb3JnYW5pc2F0
aW9uLXdhbGxldC1kZXYuc3Z4LmludGVybmFsMQswCQYDVQQGEwJBVTEOMAwG
A1UECgwFTWVlY28wWTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAASOf-ok3mlE
75rIBUshXNFCsoOn_CWzuoTw2ez8A8byCH2Gy85z3yyWNCdwgPmDW2fSxAps
uQRCpiaf4uV4YhIxo1MwUTAdBgNVHQ4EFgQUdrY50aXw8r4kGdcns82-uOW_
rTwwHwYDVR0jBBgwFoAUdrY50aXw8r4kGdcns82-uOW_rTwwDwYDVR0TAQH_
BAUwAwEB_zAKBggqhkjOPQQDAgNIADBFAiEA9amBYcm3EMCDulhqBCSxQpR2
P66AqzJLvv4HlZDkB3gCICkHJsTsUt0pdkahnV4FUzjwaGYt9Tkxz1r0jSBr
LgGMWQMz2BhZAy65AAZndmVyc2lvbmMxLjBvZGlnZXN0QWxnb3JpdGhtZ1NI
QS0yNTZsdmFsdWVEaWdlc3RzoXgcdGVzdC5tc29fbWRvYy5iYXNpY19vcHRp
b25hbKMAWCDsZwIV5jxqGrp3RCecPOyI3y_71tgAjIpnRoQEz0SRdQFYIC7v
ADEQDu3ki4KnurMK7ngvoIlBblrQXV5VQSwVjV8pAlggpfAv-iCQCB059QQq
dqCFDQBLw3kya_0ugoqYPUhfZ_9tZGV2aWNlS2V5SW5mb7kAAWlkZXZpY2VL
ZXmmAyYgAQECIVgguMzxGUb1Upi8gCPYtGuVqTsV7zayZKLcsTGy8TW6y2Ui
WCC52-obeExq0yf71v_CUxKQLZYPDhVnXGOvY6irG9eMiQJ5AW1kaWQ6a2V5
OnoyZG16RDgxY2dQeDhWa2k3SmJ1dU1tRllyV1BnWW95dHlrVVozZXlxaHQx
ajlLYnNtbjU3ZUxSMXkxb2Y4RmdxdXdjZXNheW9meHNCaWlzS2hVd1RyQlJj
ZDhYQnlMVHgzMVY5QmtoeUpWYTczS3h0UVJteXRSd3Y1OFBqS2ozblQ0VnBt
UWtYYnVjd3NrWU45YXFxS1N6aTlyWDVGS2VUWHVRRWd6RlN3aE12VGl1Y1Ej
ejJkbXpEODFjZ1B4OFZraTdKYnV1TW1GWXJXUGdZb3l0eWtVWjNleXFodDFq
OUtic21uNTdlTFIxeTFvZjhGZ3F1d2Nlc2F5b2Z4c0JpaXNLaFV3VHJCUmNk
OFhCeUxUeDMxVjlCa2h5SlZhNzNLeHRRUm15dFJ3djU4UGpLajNuVDRWcG1R
a1hidWN3c2tZTjlhcXFLU3ppOXJYNUZLZVRYdVFFZ3pGU3doTXZUaXVjUWdk
b2NUeXBlcXNjdGVzdDIxMDIyNS5tZG9jbHZhbGlkaXR5SW5mb7kABGZzaWdu
ZWTAdDIwMjUtMDItMjRUMTM6NTQ6MTZaaXZhbGlkRnJvbcB0MjAyNS0wMi0y
NFQxMzo1NDoxNlpqdmFsaWRVbnRpbMB0MjAyNS0wMi0yNVQxMzowMDowMFpu
ZXhwZWN0ZWRVcGRhdGX3WEAqk-r6wTFJLFwx5LEMDV9j8zPsn_lpwgqatDGe
H7dVr9uNjGD3QkWrB8cjwsJCdN5lX92V6NtAOFqU-8TgUkVgZnN0YXR1cwA
```

In diagnostic notation it looks like this:

```
{
    "version": "1.0",
    "documents": [
        {
            "docType": "sctest210225.mdoc",
            "issuerSigned": {
                "nameSpaces": {
                    "test.mso_mdoc.basic_optional": [
                        24_0(<<{
                            "digestID": 0,
                            "elementIdentifier": "given_name",
                            "elementValue": "alice",
                            "random": h'f6e32170ff53bc302a2d198f2c363c0e1ab9dc0ce0d17bfb7681c9851893cb95',
                        }>>),
                        24_0(<<{
                            "digestID": 1,
                            "elementIdentifier": "family_name",
                            "elementValue": "Doe",
                            "random": h'7da5be4e0c6f114dd89489466f57b272809683617c12422322ced06eda767fe3',
                        }>>),
                        24_0(<<{
                            "digestID": 2,
                            "elementIdentifier": "birth_date",
                            "elementValue": 1004_1("2000-01-25"),
                            "random": h'f836e5d051cb6e00093f3002d27889847aafd0e86e177e6d9d23733a673d3479',
                        }>>),
                    ],
                },
                "issuerAuth": [
                    h'a10126',
                    {
                        4: h'6d374b314e69576e5941386d635450347638445564337575506869616f7272766b58476f424743444d3563',
                        33_0: [
                            h'308201ed30820193a0030201020214683a357e5334ce908f646c9d08e4f11e7bff77eb300a06082a8648ce3d040302304c312d302b06035504030c246f7267616e69736174696f6e2d77616c6c65742d6465762e7376782e696e7465726e616c310b3009060355040613024155310e300c060355040a0c054d6565636f301e170d3235303133303139353633365a170d3236303133303139353633365a304c312d302b06035504030c246f7267616e69736174696f6e2d77616c6c65742d6465762e7376782e696e7465726e616c310b3009060355040613024155310e300c060355040a0c054d6565636f3059301306072a8648ce3d020106082a8648ce3d030107034200048e7fea24de6944ef9ac8054b215cd142b283a7fc25b3ba84f0d9ecfc03c6f2087d86cbce73df2c9634277080f9835b67d2c40a6cb90442a6269fe2e578621231a3533051301d0603551d0e0416041476b639d1a5f0f2be2419d727b3cdbeb8e5bfad3c301f0603551d2304183016801476b639d1a5f0f2be2419d727b3cdbeb8e5bfad3c300f0603551d130101ff040530030101ff300a06082a8648ce3d0403020348003045022100f5a98161c9b710c083ba586a0424b14294763fae80ab324bbefe079590e407780220290726c4ec52dd297646a19d5e055338f068662df53931cf5af48d206b2e018c',
                        ],
                    },
                    h'd81859032eb900066776657273696f6e63312e306f646967657374416c676f726974686d675348412d3235366c76616c756544696765737473a1781c746573742e6d736f5f6d646f632e62617369635f6f7074696f6e616ca3005820ec670215e63c6a1aba7744279c3cec88df2ffbd6d8008c8a67468404cf4491750158202eef0031100eede48b82a7bab30aee782fa089416e5ad05d5e55412c158d5f29025820a5f02ffa2090081d39f5042a76a0850d004bc379326bfd2e828a983d485f67ff6d6465766963654b6579496e666fb90001696465766963654b6579a6032620010102215820b8ccf11946f55298bc8023d8b46b95a93b15ef36b264a2dcb131b2f135bacb65225820b9dbea1b784c6ad327fbd6ffc25312902d960f0e15675c63af63a8ab1bd78c890279016d6469643a6b65793a7a32646d7a4438316367507838566b69374a6275754d6d465972575067596f7974796b555a336579716874316a394b62736d6e3537654c523179316f6638466771757763657361796f667873426969734b685577547242526364385842794c547833315639426b68794a566137334b787451526d79745277763538506a4b6a336e543456706d516b5862756377736b594e396171714b537a6939725835464b655458755145677a465377684d765469756351237a32646d7a4438316367507838566b69374a6275754d6d465972575067596f7974796b555a336579716874316a394b62736d6e3537654c523179316f6638466771757763657361796f667873426969734b685577547242526364385842794c547833315639426b68794a566137334b787451526d79745277763538506a4b6a336e543456706d516b5862756377736b594e396171714b537a6939725835464b655458755145677a465377684d76546975635167646f6354797065717363746573743231303232352e6d646f636c76616c6964697479496e666fb90004667369676e6564c074323032352d30322d32345431333a35343a31365a6976616c696446726f6dc074323032352d30322d32345431333a35343a31365a6a76616c6964556e74696cc074323032352d30322d32355431333a30303a30305a6e6578706563746564557064617465f7',
                    h'2a93eafac131492c5c31e4b10c0d5f63f333ec9ff969c20a9ab4319e1fb755afdb8d8c60f74245ab07c723c2c24274de655fdd95e8db40385a94fbc4e0524560',
                ],
            },
        },
    ],
    "status": 0,
}
```

An mdoc consists of one or more documents, each containing an `issuerSigned` part that consists out of two sections:

1. **`nameSpaces`** – this section contains claims structured within one or more namespaces. These claims can be selectively disclosed by omitting certain elements when the credential is presented.
2. **`issuerAuth`** – This secures the `issuerSigned` data and includes the Mobile Security Object (MSO), which contains cryptographic digests to validate claims. The MSO also binds the credential to a cryptographic key held by the holder.

An example of the Mobile Security Object (MSO) is as follows:

```
24_0(<<{
    "version": "1.0",
    "digestAlgorithm": "SHA-256",
    "valueDigests": {
        "test.mso_mdoc.basic_optional": {
            0: h'ec670215e63c6a1aba7744279c3cec88df2ffbd6d8008c8a67468404cf449175',
            1: h'2eef0031100eede48b82a7bab30aee782fa089416e5ad05d5e55412c158d5f29',
            2: h'a5f02ffa2090081d39f5042a76a0850d004bc379326bfd2e828a983d485f67ff',
        },
    },
    "deviceKeyInfo": {
        "deviceKey": {
            3: -7,
            -1: 1,
            1: 2,
            -2: h'b8ccf11946f55298bc8023d8b46b95a93b15ef36b264a2dcb131b2f135bacb65',
            -3: h'b9dbea1b784c6ad327fbd6ffc25312902d960f0e15675c63af63a8ab1bd78c89',
            2: "did:key:z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ#z2dmzD81cgPx8Vki7JbuuMmFYrWPgYoytykUZ3eyqht1j9Kbsmn57eLR1y1of8FgquwcesayofxsBiisKhUwTrBRcd8XByLTx31V9BkhyJVa73KxtQRmytRwv58PjKj3nT4VpmQkXbucwskYN9aqqKSzi9rX5FKeTXuQEgzFSwhMvTiucQ",
        },
    },
    "docType": "sctest210225.mdoc",
    "validityInfo": {
        "signed": 0("2025-02-24T13:54:16Z"),
        "validFrom": 0("2025-02-24T13:54:16Z"),
        "validUntil": 0("2025-02-25T13:00:00Z"),
        "expectedUpdate": undefined,
    },
}>>)
```
As you can see, it contains a `valueDigests` section that has a similar setup to the `disclosures` in the SD-JWT credential format.

Within OpenID4VCI and OpenID4VP, the credential format is identified as `mso_mdoc`. 

When an mdoc is presented to a verifier, an additional `deviceSigned` section is included to further secure the exchange. However, for simplicity, we leave that aspect out in this discussion.

# Conclusion

Each credential format has its strengths and trade-offs. JSON-based formats are widely used for their ease of implementation and readability, while CBOR-based formats optimise for offline use and efficiency. The choice of format depends on the specific use case, security requirements, and level of adoption in the industry.

An interesting distinction is that the ISO/IEC 18013-5 standard defines rails that exclusively support mdoc credentials. In contrast, OpenID4VC rails are credential format-agnostic, allowing for a broader range of credential formats to be issued and verified.

In the next article, we'll explore **trust mechanisms** that ensure the integrity and authenticity of credentials within this universal model for personal data exchange.
