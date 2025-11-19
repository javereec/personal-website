+++
date = '2025-11-19T18:00:00+01:00'
title = 'How a Restaurant Can Help You Better Understand Standards'
+++

## The Interoperability Trap

A classic problem when dealing with standards development is that when two companies claim they both have "implemented the standard", it doesn't necessarily follow that their software successfully interoperates. Now why is that? Isn't the whole purpose of the standard to describe the one way of doing things? Well, no, not exactly.

##  A Concrete Example: Optionality Inside a Standard

To better understand what standards are, let's look at an example from the [OpenID for Verifiable Presentations](https://openid.net/specs/openid-4-verifiable-presentations-1_0-final.html) specification. Here, I want to illustrate how one specification builds upon the foundation of another and the optionality and branching behaviour inside standards. This part might be slightly technical, but stay with me.

[Section 5.9 Client Identifier Prefix and Verifier Metadata Management](https://openid.net/specs/openid-4-verifiable-presentations-1_0-final.html#section-5.9) of OpenID4VP defines the concept of `Client Identifier Prefixes`. In it, the spec extends [RFC 6749 The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749#section-2.2) specification that requires every request to have a `client_id` present when making an (authorization) request. The value of the `client_id` normally is undefined, opaque, but OpenID4VP changes this as it requires implementations to interpret this value. It's structure is as follows

```
<client_id_prefix>:<orig_client_id>
```

The spec further defines the possible values for these client identifier prefixes and how they should be used. I'll list them below: 

- `redirect_uri`
- `openid_federation`
- `decentralised_identifier`
- `verifier_attestation`
- `x509_san_dns`
- `x509_hash`
- `origin`

What each of them does would take us too far, but what I want to show here is that this creates 7 possible ways a wallet can authenticate the request from the verifier. Do you have to implement all of them? No, of course not. But if you only support two, and your counterpart supports three, the overlap is not guaranteed. Okay, so which ones do I choose? It depends. There is no right or wrong. It truly depends on the network you want to participate and what the prevailing method(s) of client authentication is. And this optionality is not an accident. It is purposely there to allow serving as many legitimate use cases as possible. 

## Why Standards Are Not Implementation Specs

When I started my standards journey, my first reflex was to treat a standard like you would treat an implementation spec. It took me a while to realise why that mindset fails in practice. The key is that a standard rarely tells you exactly what to build; it describes the framework and the possibilities.

A specification:

- almost always depends on other specifications, each with their own options and interpretations
- is not a standalone implementation blueprint; essential details often sit inside referenced documents
- defines only one part of a wider system and assumes substantial prior knowledge of the layers it extends

## The Only Correct Answer is 'It Depends'

Staying with the example of OpenID4VP, at the start of [Section 5](https://openid.net/specs/openid-4-verifiable-presentations-1_0-final.html#section-5) we read

> The Verifier MAY send an Authorization Request as a Request Object either by value or by reference, as defined in the JWT-Secured Authorization Request (JAR) [RFC9101].

Should the Verifier use JAR or not? According to OpenID4VP it is not mandated (hence the MAY). However, from a security point of view JAR adds integrity protection to the request, offering additional guarantees so it can't be tampered with. That's a good thing, right? Let's mandate it! First of all, this comes at an implementation cost for all parties involved. Secondly, there might be other – some would argue – better solutions such as the W3C Digital Credentials API which uses a different, platform & browser enabled, feature that offers these guarantees.

Between branching options like the client identifier prefixes and optional security mechanisms like JAR, you quickly end up with a myriad of possibilities. This example illustrates that the answer, as with most things, is: "it depends". Not because the standards are vague, but because they deliberately leave room for different design choices. Two perfectly reasonable implementors can follow the spec and still end up with different security measures, different request flows, or different assumptions about the underlying platform. And that variation is enough to make interoperability fall apart. All of this might be obvious for the standards illuminati, but for those starting their journey to enlightenment, these things are not immediately apparent. 

## A Restaurant Menu as a Standard

Okay, how is a restaurant going to help us understand this? I chose a restaurant because I love food, wine and good company. The answer is: why not? The specific analogy that I would like to use here comes from the restaurant's menu. In a typical sort-of-classy restaurant you can choose to either eat à-la-carte or go for the menu option. When you are on your own, you can go any way you like. Want 2 starters and 2 desserts, go for it. Prefer the classic starter, main and dessert, awesome. On a diet, go for the chicken without carbs as main dish, just like the cool kids do. The point that I want to make here is that you have options. You can select any dish from the menu, it's all up to you and there is nobody to defy you. As you should be able to guess, in this analogy the menu is a standard specification. It describes a system or part of a system, be it an API (such as in the case of OpenID for Verifiable Presentations), a credential format (such is the case for IETF SD-JWT VC) or anything else involved in a system.

## Enter the Chef's Menu: Profiles

Things start to change, at least here in Belgium, as soon as you go as a group. Now you have to take into account what other people want. They want you to limit your choice so they can actually prepare the food on time to an acceptable standard. What they really like you to do is go for: "The Chef's Menu". These menus are predefined selection of dishes. You might have the option between fish and meat and you might even have the cheese instead of the dessert at the end, but choices are much more restricted (if any at all). You have to: agree on what to take as a group.

"The Chef's Menu" in this analogy is a "profile". It is a set of choices that the restaurant makes for you to provide the best possible experience at a good price. In the same way that a profile, such as [OpenID's High Assurance Interoperability Profile (HAIP)](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html), selects features and introduces requirements to all participants in order to promote interoperability between parties. It tries to make everyone's life easier.

In technical terms, that's exactly what a profile does: it narrows the field. A profile doesn't replace the underlying standard; it selects from it. It chooses which options are allowed, which parameters are required, which flows must be supported, and which security measures everyone has to align on. It removes ambiguities and makes things predictable. In other words, a profile turns a (set of) standards into a playbook that all participants can realistically implement and reliably interoperate on.

It's also worth noting that every ecosystem tends to define its own profile. Different countries, industries, and networks pick different defaults, mandate different security levels, and sometimes even make incompatible choices. So simply saying "we support OpenID4VP" is almost meaningless without naming which profile you align with. The profile is the real contract: it tells others exactly which branch of the standards tree you’re following, and therefore whether you can realistically interoperate.

If we go back to our previous example of the authorization request. We can read in [Section 5.1 OpenID for Verifiable Presentations via Redirects](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html#section-5.1)

> Signed Authorization Requests MUST be used by utilizing JWT-Secured Authorization Request (JAR) [RFC9101] with the request_uri parameter.

And in [Section 5.2 OpenID for Verifiable Presentations via W3C Digital Credentials API](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html#section-5.2)

> The Wallet MUST support both signed and unsigned requests as defined in Annex A.3.1 and A.3.2 of [OIDF.OID4VP]. The Verifier MAY support signed requests, unsigned requests, or both.

As an implementor, this is now clear. The selection has been made and each side knows what they have to support. Testing can begin!

So, when two parties say that they support [HAIP's OpenID for Verifiable Presentations via the W3C Digital Credentials API using ISO Mobile Documents](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html#name-openid-for-verifiable-present), that is something where technical interoperability can and should be achieved. It provides a much narrower scope and limits the amount of permutations that are possible when "just" considering OpenID4VP. It links everything together.

## Conclusion: From Menu to Interoperability

In the end, that's the whole point of a profile: it takes the sprawling à-la-carte menu of a standard and turns it into something everyone at the table can actually enjoy together. By narrowing the choices, profiles such as HAIP or FAPI 2.0, remove ambiguity, reduce combinatorial madness, and give implementors a shared target. With that, interoperability stops being an aspiration and becomes something you can realistically test, verify, and deliver.
