+++
date = '2025-04-13T17:00:00+01:00'
title = 'The Overuse of Trust in Digital Identity'
+++

In the world of digital identity, it doesn’t take long before you start encountering the word trust. It appears everywhere: *trust model, trust framework, trust scheme, trust service, trust registry, trust management, trust infrastructure, trust anchor, trust chain, qualified trust service provider*. The term is used so frequently and in such a variety of ways that it begins to feel more like a placeholder than a concept with concrete meaning.

This raises an important question: what do we actually mean when we say "trust" in digital identity systems?

In practice, "trust" often functions as a shortcut for describing confidence in a process, technology, or relationship. We might say that a verifier trusts an issuer, or that a credential is trustworthy, or that a system is certified under a trust framework.

Instead of jumping straight into the technical and practical apparatus of digital trust — certificates, cryptographic proofs, and legal agreements — it is worth stepping back and asking a foundational question: What is trust?

As it turns out, that’s not an easy question to answer. Part of the difficulty lies in the fact that trust is something all humans intuitively understand and perform. That it is a basic fact of everyday life (Luhman, 1990), and as such there is no need to ever define it. Nevertheless, when we seek to model trust in digital systems, especially systems involving identity, we must move beyond intuition and develop a clearer, more formal understanding.

In what follows, we will begin from first principles, exploring how trust has been defined, how it can be formalised, and how these ideas can be mapped onto the domain of digital identity. By doing so, we aim to understand not just what trust is, but how it can be represented, computed, and ultimately used to inform policy decisions in digital ecosystems.

## What Is Trust?

Trust is a concept so deeply embedded in human interaction that it escapes scrutiny. We trust people to tell the truth, systems to function reliably, and organisations to act according to expectations. But what are we actually doing when we trust?

Definitions of trust can be found in biology, sociology, economics, history, philosophy and social psychology. It is from this last field that we find perhaps the most widely accepted definition of trust by American social phychologist Morton Deutsch. In 1962 he defines that trusting behaviour exists when there is an ambiguous path, one good and one bad, and the occurrence of either path is contingent on the actions of another entity. Also, the result of taking the bad path is more harmful than the consequences of taking the good path. So, if an entity goes down that path, it is said to have made a trusting choice, if not, it is distrusting. This framing brings an essential element into view: **risk**.

The connection between trust and risk is a recurring theme. To trust is to expose oneself to the possibility of harm, with the expectation that the other party will act in a way that avoids it. Without risk, there is no trust. It would simply be a rational choice (Luhmann, 2000).

## Formalising Trust

Stephen Marsh introduced a formalism in his 1994 thesis that models trust as a computational concept. Rather than treating trust as a binary (trusted or not). Marsh proposes first that trust is best understood as a scalar quantity. A value that can **range from -1 to +1**. On this scale -1 represents complete **distrust**, 0 represents **zero-trust**, and +1 represents **blind trust**. Note that zero-trust is not the same as distrust. With the former, an agent may be impartial or they may have never met. Distrust on the other hand is something that you arrive at  having known someone and their past actions cause you to no longer trust them.

This seemingly simple numerical scale opens the door to reasoning about trust in a way that machines, and policy engines, can handle. Trust is no longer a feeling or social intuition; it becomes something that can be evaluated, compared, adjusted, and even calculated. This makes it possible for agents to make trusting decisions.

The second concept of Marsh's model is that **trust is situational**: How much one agent trusts another in a particular context is calculated as:

```
Situational Trust = Importance x Utility x Reputation
```

Where:

- **Importance** is how critical the outcome is to the agent
- **Utility** is the expected upside in the situation
- **Reputation** is an estimation of past experiences

This formula captures that we tend to trust different in different situations. I might be fine using Google Docs to share information with community group, but I would not trust it as a way to deliver sensitive documents as a whistleblower in a high stakes case.

Lastly, trust by itself is not enough to act. Marsh introduced a **cooperation threshold**, a value representing the minimum situational trust needed for an agent to proceed.

The decision rule is straightforward. If situational trust >= cooperation threshold, act (e.g. accept the credential). If situational trust < cooperation threshold, abort.

This model enables dynamic, rational, automated decision making in complex environments.

## Trust and Transitivity

The last element to discuss before we move back to digital identity, is that trust is not necessarily transitive (Christianson and Harbison, 1997). If Alice trusts Bob, and Bob trusts Charlie, it does not automatically mean that Alice should trust Charlie.

Exactly because trust is situational, it depends on an agents own utility, importance and previous experience. Just because Bob finds Charlie reliable, doesn't mean that Alice should. At best it can inform a trust decision.

## Trust in Digital Identity Systems

Now that we have a deeper understanding of trust, it properties and it's limits, we can take a more critical look at how “trust” is currently handled in digital identity systems.

At first glance, many identity frameworks appear to be built on trust: trust frameworks, trust anchors, trust lists, trust chains. But what these systems actually encode is not trust in how we discussed it above. Instead, they primarily model and manage assurance.

The dictionary defines assurance as a positive declaration intended to give confidence. Now, confidence is not the same as trust. The main difference being that trust presupposes an element of risk, whereas confidence does not (Luhmann, 2000). When you choose to create a connection with another agent, but not consider any alternatives, you are confident that they will do the right thing. [this might be to simplistic, but want to give an example that relates to systems, not people]

## From Trust to Assurance

In frameworks such as eIDAS 1 and more recently eIDAS 2, what is often referred to as “trust” is actually a structured way of expressing confidence in conformance and meant to support trust (EC, 2014):

- Confidence that a credential was issued following defined procedures
- Confidence that a cryptographic key was managed in a compliant manner
- Confidence that an actor operates within agreed legal and operational boundaries

These forms of confidence are not subjective evaluations made by a relying party at the moment of interaction, instead they are pre-verified attributes, externally validated and certified, and typically published in registries or "trust lists".

But here’s the key point: these artefacts don’t convey trust, they convey assurance.

Whereas trust is situational, agent-specific, and dynamic, assurance is structural, objective, and static.

## ~~Trust~~ Assurance Lists: Evidence, Not Decision

Take the example of the EU Trusted List under eIDAS. It contains a set of qualified trust service providers who have been accredited under national supervision bodies. Being on the list means a provider has met the legal and technical requirements to issue certain types of credentials or signatures.

But it does not mean:

- That the credential will always be accepted
- That the verifier should trust the issuer in all contexts
- That transitive trust is safe or implied

Instead, the list acts as a source of assurance. It informs trust decisions but does not make them. It surely helps a relying party increase its general trust score in Marsh’s model, but the final situational trust and if it meets the cooperation threshold, still depends on the context, the stakes, and the verifier’s own judgement.

From this perspective, the term “trust list” is actually a misnomer. A more accurate term would potentially be an “**assurance list**”, a record of entities who have demonstrated compliance with specific requirements, and who may be eligible to be trusted under the right circumstances.

## Level of Assurance (LoA) as Cooperation Thresholds

eIDAS uses the concept of Levels of Assurance LOW, SUBSTANTIAL and HIGH to provide a degree of confidence an agent can have in digital credentials (called electronic attestations in eIDAS) to establish the identity of a person (EC, 2014).

LoA's work very much like cooperation threshold discussed earlier. It represents the minimal situational trust required to proceed, but instead of calculating it, it is externalised as a policy and not dynamically calculated.

## Digital Signatures: Evidence of Origin, Not Evidence of Trust

Digital signatures are used ubiquitously in digital identity systems. From authenticating credentials, requests for credentials and presentations of those credentials to supporting legally binding transactions, they are seen as foundational building blocks of any digital identity system. But what do they actually provide?

To answer that, it’s helpful to return to their origin.

In 1978, Rivest, Shamir, and Adleman (RSA) introduced the first practical implementation of a public-key cryptosystem, which included a method for creating and verifying digital signatures. In their model, the signer applies a private key to a message to produce a signature. Anyone with the corresponding public key can verify that the message was indeed signed by the private key holder and that it has not been tampered with, without also revealing the corresponding decryption key.

Their system ensured:

- **Authenticity**: The signature could only have come from the owner of the private key.
- **Integrity**: The message had not changed since it was signed.
- **Non-repudiation**: The signer could not later deny having signed the message.

What it did not do, however, was tell you whether or not to trust the signer. They provide however cryptographic evidence that can serve as inputs to reasoning about trust.

## X.509 Certificates: Binding, Not Belief

Building on digital signatures, X.509 certificates wrap them in structured metadata. They are foundational in many digital identity systems (and many others beyond). They cryptographically bind a public key to a subject, such as a person, organisation, or service, and are typically issued by a Certificate Authority (CA).

In many systems, the presence of a valid certificate is often interpreted as an indicator of trust. But much like trust lists, X.509 certificates do not represent trust decisions, they provide evidence that supports those decisions.

An X.509 certificate answers one very narrow question:

“Has this CA asserted that this key belongs to this entity?”

It does not answer whether the entity should be trusted in this situation. Trust decisions still need to be made by the verifier, based on the certificate’s metadata, assurance level, issuance context, and potentially the issuer’s reputation. In Marsh’s terms, the certificate might contribute to relational trust in an entity, but the actual situational trust needs to be calculated per use.

Thus, certificates are not trust, they are binding artefacts that support trust reasoning.

## Conclusion

We’ll continue to use the word trust, it’s too embedded in our standards, frameworks, and conversations to avoid. But we should now be more precise about what it means.

Trust is not something that can be handed over in a certificate, listed in a registry, or guaranteed by a signature. These are all vital tools, but they are tools that provide evidence, not decisions. Trust remains something that must be reasoned, contextualised, and earned over time.

What Marsh’s model shows us is that trust is not a static status, but a situational judgement. It depends on how important the outcome is, how much utility it provides, and how reliable an agent has been in the past. Formalising trust in this way helps us design systems that don’t just feel trustworthy, but can actually explain why something was trusted and under what conditions that trust might no longer apply.

As digital identity systems scale, diversify, and decentralise, this distinction between assurance and trust becomes more important, not less. A more complex environment, requires a more nuanced implementation. We need both: the structured, externally verifiable assurance that a system or actor conforms to expectations, and the internal, contextual reasoning that allows agents to decide when to act.

By keeping those roles clear we create space for systems that are not only more secure, but also more honest about what trust really means.

---

- Christianson, Bruce, and William S. Harbison. 1997. ‘Why Isn’t Trust Transitive?’ In Security Protocols, edited by Mark Lomas, 1189:171–76. Lecture Notes in Computer Science. Berlin, Heidelberg: Springer Berlin Heidelberg. https://doi.org/10.1007/3-540-62494-5_16.
- Deutsch, M., 1962. Cooperation and trust: Some theoretical notes.
- European Commission. 2014. ‘Regulation (EU) 2014/910 of the European Parliament and of the Council of 23 July 2014 Repealing Regulation (EU) No 1999/93/EC on Electronic Identification and Trust Services for Electronic Transactions in the Internal Market’.
- Luhmann, Niklas. 2000. ‘Familiarity, Confidence, Trust: Problems and Alternatives’. Trust: Making and Breaking Cooperative Relations 6 (1): 94–107.
- Marsh, Stephen Paul. 1994. ‘Formalising Trust as a Computational Concept’. http://dspace.stir.ac.uk/handle/1893/2010.
- Rivest, R. L., A. Shamir, and L. Adleman. 1978. ‘A Method for Obtaining Digital Signatures and Public-Key Cryptosystems’. Communications of the ACM 21 (2): 120–26. https://doi.org/10.1145/359340.359342.
