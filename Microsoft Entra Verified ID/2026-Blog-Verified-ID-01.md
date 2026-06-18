# Decentralized Identity Systems Concepts

![Centralized Identity Systems](./202606-BLOG-Verified-ID-01.jpg)

Today, most digital identity systems are built around a central identity provider. That provider signs users in, stores key identity data, and often sits in the middle of every trust relationship between people, apps, and organizations.

This model works, but it also creates some growing problems. 

1. Extensive personal data is collected and retained by ISPs (Identity Service Providers)  
2. Many organizations need custom trust connections with each ISP (Trust Federations)
3. The centralized architecture of each ISP constitutes a single point of failure.

This article explains: 

1. Why decentralized identity is a better alternative to the current model 
2. How verifiable credentials improve the current model 
3. Why Microsoft Entra Verified ID is an important example of this new approach.

## The problems of the current model

In the traditional model, identity is something people do not really own. A user signs in through an identity provider, and that provider controls the credential lifecycle, the sign-in process, and often the way identity data is shared with applications and partner organizations.

This creates four practical problems.

- **Too much concentration of trust.** When one identity provider or its surrounding infrastructure fails, many connected services can be affected at the same time.

- **Too much stored data.** Central systems often collect and retain more personal data than a verifier needs for a specific decision.

- **Too much integration work.** Organizations often need federation, guest accounts, or other explicit trust arrangements to work across company boundaries.

- **Too little user control.** Users usually have little visibility into who can access their data and how often it is reused.

A good way to summarize the issue is this: 

the current model is excellent at account-based access, but not as good at portable, privacy-friendly proof. It can say, “this person can sign in here,” but it cannot say, “this person can prove one fact, and nothing more.”

## What decentralized identity changes

![Decentralized Identity Systems](./202606-BLOG-Verified-ID-02.jpg)

Decentralized identity changes the center of the system. Instead of a central identity provider being involved every time, the user can hold digitally signed credentials in a wallet app on their own device.

These credentials are called **verifiable credentials**. 

An issuer, such as an employer, university, or government office, signs claims about a person or organization, often after having verified them through official means, sometimes in person and through actual human interactions. The holder stores that issued credential in a wallet, and a verifier can later check that this information is authentic and still valid **through a distrubuted system, therefore without having to directly contact the issuer!**.

This model is often described using three roles.

- **Issuer** — the organization that creates and signs the credential.
- **Holder** — the person or entity that stores it in a wallet and chooses when to present it.
- **Verifier** — the organization that checks the credential before granting access, trust, or a service.

That shift is important because it turns identity from a stream of repeated account lookups into a system of reusable, trusted proofs.

## A simple real-world analogy

A driving licence is a useful analogy. A government authority first checks a person’s identity and eligibility, then issues a document that states certain claims, such as the right to drive a certain class of vehicle, and also age, registered address and other relevant personal and professional information about the holder.

Later, the licence holder can show that document to another party, such as a police officer or a car rental company. The third party does not need to call the original authority each time to understand what the document means; they only need to trust the issuer and verify that the document is genuine and belongs to the person presenting it.
In general, a police officer will known that the document is legit or could even have 
way to verify it.

A verifiable credential follows the same broad idea in digital form. The document becomes a cryptographically signed digital credential, the user stores it in a wallet, and the verifier checks the credential instead of collecting and storing large amounts of raw identity data again. 

In this model, neither the verifier, nor necessarily the issuer need to retain part or 
even any of the information about the user, instead the information will be packaged 
in a verifiable digital form signed by the issuer and stored on the holder's wallet.

## Why this is better for users

The biggest improvement for users is control. A user can keep credentials in a wallet and present them when needed, instead of repeatedly filling in the same forms and sending the same documents to many different services.

This also supports data minimization. In many cases, the verifier does not need a full identity profile. It may only need proof of employment, proof of student status, proof of age, or proof that a person passed an identity check.

That means the user can share less, while still proving enough. This is a better fit for privacy, and it reduces the spread of personal data across many disconnected systems.

## Why this is better for organizations

For organizations, decentralized identity is not just a user-experience improvement. It can also reduce operational friction and lower security risk and liability.

When an organization verifies a signed credential instead of storing every identity detail itself, it can reduce the amount of sensitive data it needs to protect. This can simplify onboarding, partner access, and other trust-based workflows.

There are several clear benefits.

- **Less repeated identity proofing.** A trusted issuer can verify a person once and issue a reusable credential.

- **Lower data exposure.** The verifier can ask for the proof it needs instead of collecting a full identity profile.

- **Cleaner cross-company trust.** Portable credentials can reduce the need to expand trust boundaries through federation for every scenario.

- **Better fraud resistance.** Credentials are signed and checked cryptographically, which makes tampering harder and verification stronger.

- **Lower risks mand liability** Credentials, once issued and verified, do not necessarily need to be stored by the businesses that produce and consume them. This reduces their responsibility towards the individuals and organization interacts with and also makes the organization a less valuable target in the eyes of malicious agents.

This is why decentralized identity is often best understood as an addition to existing identity systems, not always as a full replacement. Centralized identity still works well inside a company’s own trust boundary, while verifiable credentials are especially useful when trust has to move across boundaries.

Nevertheless, emerging technologies and implementation such as **Microsoft Entra Verified ID** makes this transition possible for the organizations that wish to implement it.

## The role of the wallet

The wallet is central to the user experience. In Microsoft’s documentation, Microsoft Authenticator is the wallet app that can: 

- create decentralized identifiers 
- receive issuance requests
- store credentials
- respond to presentation requests

That matters because the wallet is where the user’s control becomes real. The user can review a request, consent to share a credential, and keep a record of where that credential was presented.

This is a major conceptual improvement over systems where identity is silently copied between services or reused without much visibility from the user.

## Examples that make the value clear

The easiest way to understand decentralized identity is through real-world scenarios.

### Employee proof

A company can issue an employee credential to a worker. That worker can later present it to a partner organization to prove employment status without needing a custom federation link between both companies.

Microsoft’s Woodgrove and Proseware example shows exactly this pattern: one company issues proof of employment, and another company accepts that proof to grant a discount or service.

### Remote onboarding

A verified identity credential can help with remote hiring and onboarding. After an identity proofing step, the new employee can use the credential to receive initial access or prove identity in later onboarding steps.

This can reduce manual review and avoid unnecessary copying of sensitive identity data into each internal system.

### Access to external services

A person may need to prove something outside their employer’s trust boundary, for example employee status, supplier status, or membership in a program. A portable credential is often a better fit than creating yet another external account.

This is useful because the verifier can trust the issuer’s signed credential without needing the issuer to sit in the middle of every transaction.

### Public and regulated services

The same pattern can apply to health cards, permits, student credentials, age checks, or local government services. The verifier only needs enough proof for the decision at hand, not a full copy of the user’s identity records.

That makes the model easier to explain to readers because it mirrors how official documents are already used in the physical world.

## Where Microsoft Entra Verified ID fits

Microsoft Entra Verified ID is Microsoft’s managed service for issuing and verifying verifiable credentials. It is based on decentralized identity concepts and open standards such as W3C verifiable credentials and decentralized identifiers.

For a Microsoft-focused audience, the key point is that Verified ID extends identity beyond ordinary sign-in. It supports use cases where an organization needs a trusted, portable, privacy-aware proof that can be reused across systems and organizational boundaries.

In simple terms, Entra ID helps users sign in to systems. Verified ID helps users prove something about themselves in a way that another party can verify and trust.

## Conclusion

The Centralized Identity Systems currently in use place much trust, data, and control in the hands of a few Identity Providers. 

Decentralized identity Systems change te paradigm by giving: 

- users a wallet
- organizations reusable signed credentials
- verifiers a better way to check claims without collecting everything themselves

That is why verifiable credentials matter and represent the basis for a more secure approach to the management of user identies and their handling. 

They make digital trust 

- more portable
- more private 
- easier to reuse across real business scenarios

A follow-up article will build on this foundation and explain the technical details behind DIDs, verifiable presentations, trust systems, issuance flows, verification flows, and the Microsoft Entra Verified ID architecture in more depth.
