# Microsoft Entra Verified ID  

[AZ-500-SC-100-Understanding and Using Verifiable Credentials - John Savill](https://www.youtube.com/watch?v=BxLSSH_EHjo&t=4834s)   

[Why Verifiable Credentials and Decentralized Identity? - Daniel Krzyczkowski NDC Conferences](https://www.youtube.com/watch?v=cDka7MdtjEA)   

[Introduction to Microsoft Entra Verified ID Core Concepts and Use Cases Tech Mind Factory](https://www.youtube.com/watch?v=rek6KDEgGjE&t=913s)   

[Defeat Deep Fakes and Imposters with Verified ID and Face Check - John Savill](https://www.youtube.com/watch?v=58j2PLW-M5k&t=8s)   

[Decentralized-Identity-and-Verifiable-Credentials Public](https://github.com/microsoft/Decentralized-Identity-and-Verifiable-Credentials)  

## Ecosystem Overview

![01.EcosystemOverview](./01.EcosystemOverview.png)    


### Holder 

A **Holder is a role an entity might perform** by 

- possessing one or more verifiable credentials 
- generating verifiable presentations from them. 

Example holders include students, employees, and customers.
Typically this would be the Wallet App installed on a person's (=the entity) mobile.
A holder is usually, but not always, a subject of the verifiable credentials they hold.
Holders store their credentials in **credential repositories** (i.e the Wallet App).
Holders will **present** the verifiable credential so that they can be verified 
in order to grant them (the subject) some kind of access credentials to some resources.

### Issuer 

**A issuer a role an entity might perform** by: 

- asserting claims about one or more subjects
- creating a verifiable credential from these claims
- transmitting the verifiable credential to a holder. 

Example issuers include corporations, non-profit organizations, trade associations, governments, and individuals.

### Credential Repository

A Credential Repository is a program, such as a storage vault or a **personal verifiable credential wallet**, that 
**stores verifiable credentials and protects access to them**. For example, the **Microsoft Authenticator App** is
one Credential Repository.

### Subject

An entity about which claims are made. Example subjects include human beings, animals, and things. 
In many cases the holder of a verifiable credential is the subject.

### Verifier

**A Verifiyer is a role an entity performs** by receiving one or more verifiable credentials, 
optionally inside a verifiable presentation, for processing. 
Example verifiers include employers, security personnel, and websites.

Other specifications might refer to the Verifier role as the **Relying Party**.


### Verifiable Data Registry

A **Verifiable Data Registry is a role a system** might perform by **mediating** 
the creation and verification of identifiers, keys, and other relevant data, such as: 

- verifiable credential schemas
- revocation registries
- issuer of public keys
- more

which might be required to use verifiable credentials.

In other words **a Verifiable Data Registry is a role a system is a mediator** among:

- the Holder
- the Issure
- the Verifier

so that the entities with these roles do not have to interact directly with each other in order to 
create, transmit, present use and validate verifiable credentials.

### Verification

Verificatin is the evaluation of whether a verifiable credential or verifiable presentation is an **authentic** 
and a timely statement of the issuer or presenter, respectively. 

This includes checking that: 

- the credential (or presentation) conforms to the specification; 
- the proof method is satisfied; 
- if present, the status check succeeds. 

Verification of a credential **does not imply evaluation of the truth of claims encoded in the credential**, 
it only guarantees that the verifiable credentials are value, not true!

This is why often Verifiable Credentils will be combined with **Identiy Proofing**, that is the **process** 
that verify the identity the identity of the subject of the credential. The typical analogy is that of
a driving license (the verifiable credential) on which some claims about a subject are printed 
(claims on the verifiable credential, the driving licence), in which the officer who issues the driving
luicense document verifies the identity of the subject to whom the verifiable credentials are issued,
often by oher means of verifiable and authentifiable identification, i.e. passports or identity documents 
issues to the subject by other authorities.

### Claim 

A Claim an assertion made about a subject.

### Subject 

The subject is a logical object about which claims are made, i.e. a person, vehicle, employee, etc.

### Credential and Verifiable Credential

**A credential is a set of one or more claims made by an issuer about an entity**. 

Credentials can also include **metadata used to describe properities of the credential**, 
.i.e the issuer or the credential creation date and time and expiration date and time, etc.
The **metadata may also be signed by the issuer** to cryptographylly prove that the metadata 
was issued by the issuer.

A **verifiable credential is a tamper-evident credential** that has authorship that can 
be cryptographically verified. Verifiable credentials can be **used to build verifiable presentations**, 
which can also be cryptographically verified. 

The claims in a credential can be about different subjects.

###  Entity 

Is A thing with distinct and independent existence, such as a person, organization, or device that performs one or more roles in the ecosystem.

---

###  Decentralized Identifier 

A **DID** a **portable URL-based identifier**, also known as a DID, **associated with an entity**. 

It is a URI resolvable to DID documents and it is composed of three parts: 

- the scheme did 
- a method identifier
- a unique method-specific identifier specified by the DID method. 

These identifiers are most often used in a verifiable credential and are associated with subjects 
such that a verifiable credential itself **can be easily ported from one repository to another without the need to reissue the credential**. 

An example of a DID is string or text value such as `did:example:123456abcdef` whose parts are:

1. The Scheme: did
2. The DID Method: example
3. The DID Method-Specific Identifier: 123456abcdef

---

### Decentralized Identifier Document 

A DID document, is a digital document that is **accessible using a verifiable data registry** 
and **contains information related to a specific decentralized identifier**, such as the 
associated repository and public key information.

A DID document typically:

- **DOES NOT** contain any claims related to the user
- express one or more Verification Methods
- contain cryptographic Public Keys
- add services relevant to the interactions with the DID subject

The `did:example:123456abcdef` **resolves to a DID Document**.
The DID Doc contains information about the DID, for example it contains the DID **controller**.
The values in the DID Doc **are required to very the verifiable credentials**.

Users create, own, and control Decentralized Identifiers (DIDs) independently 
of any organization or government. These globally unique identifiers are linked 
to Decentralized Public Key Infrastructure (DPKI) metadata, which consists of 
JSON documents that contain public key material, authentication descriptors, 
and service endpoints.

The DID Doc in short is a container for public cryptographical information and metadata.
DID Documents will eventually be stored by a Decentralized Public Key Infrastructure (DPKI)
layer that is sometimes referred to as the **Trust System**.

> Example 1: a simple DID Document

```
{
"@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
]
"id": "did:example: 123456789abcdefghi",
"authentication": [{
    // used to authenticate as did:...fghi
    "id": "did:example: 123456789abcdefghi#keys-1",
    "type": "Ed25519VerificationKey2020",
    "controller": "did:example: 123456789abcdefghi",
    "publickeyMultibase": "zH3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV"
}]
}
```

![02.DecentralizedIdentifierArchitecture](./02.DecentralizedIdentifierArchitecture.png)    


### DID controllers 

The controller of a DID is the entity (person, organization,or autonomous software) 
that has the capability (as defined by a DID method) to make changes to a DID document. 
This capability is typically asserted by the control of a set of cryptographic keys used 
by software acting on behalf of the controller, though it might also be asserted via other 
mechanisms.

### DID subjects 

The subject of a DID is, by definition, the entity identified by the DID. 
The DID subject might also be the DID controller. 
Anything can be the subject of a DID: person, group, organization, thing, or concept.

### DID Methods

DID methods such as `did:web` and `did:ion` are the nechanisms by which 
a particula type of DID and its associated DID Document are: 

- created
- resolved
- updated
- deactivated

### DID resolvers and DID resolution 

A DID resolver is a system component that takes a DID as input and produces a conforming DID document as output. 
This process is called DID resolution. The steps for resolving a specific type of DID are defined by the relevant 
DID method specification.

### Verifiable data registries 

DIDs are typically recorded on an underlying system or network of some kind, in order to be resolvable to DID documents. 
Any system that supports recording DIDs and returning data necessary to produce DID documents is called a verifiable data 
registry inrespective of the technology used to implement them. 

Examples include: 

- distributed ledgers
- decentralized file systems
- databases of any kind
- peer-to-peer networks
- other forms of trusted and generally distributed data storage such as blockchain

---

# Interoperability with Current Identity Standards

OpenID standard is used for the two parts

1. Verifiable Credentials Issuance
2. Verifiable Presentations

## OpenID for Verifiable Credentials Issuance

The OpenID for Verifiable Credentials Issuance is the specification that 
defines the API used to issue verifiable credentials. 

Identity Providers (Issuers) implements this specification to issue verifiable 
credentials to Holders.

The access to any implementation of the OpenID for Verifiable Credentials is 
**authorized using OAuth 2.0**.  This also menas that **any Wallet application** 
**will authorize itself to the any API that issues verifiable creadential with OAuth 2.0**.

**Verifiable Credentials** (VC) are very similar to **identity assertions** such as 
**ID Tokens in OpenID Connect**, in that VC allow a **Credential Issuer** to assert
End-User claims.

---

## OpenID for Verifiable Presentations

> Default Specification

The OpenID for Verifiable Credentials Presentations is the specification 
that defines the API used to issue Verifiable Presentations. 

**Holders** implement this specification to issue verifiable presentations 
to Verifiers. A Verifier implements the issuance of access token to access 
an API based on the verifiable credentials in the verifiable presentation.
The access token amy also be signed by the subject, if required; this makes
it possible to issue self-signed tokens. 

The OpenID for Verifiable Presentations specification mandates that access to the 
implementation API is by authorization via OAuth 2. Therefore, Verifiers are the 
role that requests access to the Verifiable Presentations (through the API), 
therefore this interaction is authorizated via OAuth 2.  

> Latest Specification: There is a newer standard for OpenID for Verifiable Presentations

Verifiers are normally Wallet applications, such as one of the many 
Authenticator Apps that may be installed on a user's mobile.
In the default OpenID for Verifiable Presentations just presented 
the App obtains an access token through the OAuth 2.0 Authorization Flow.
However, the new standard specifies how an an access token may be obtained 
by using verifiable credentials already stored in the Wallet. 

> Combination of the New specification for Verifiable Presentations with the Default Specification

The newer specification implementations of the OpenID for Verifiable Presentations can be combined with the **OpenID Connect** default
implementation when it is required that **the ID token is signed by the subject**.

> Combination of the New specification for Verifiable Presentations with other Specifications (for self-issued ID tokens)

.

---

## 03 OpenID for User Authentication: (SIOP v2 [Self-Issued OpenID Porvider])

![03.SIOPv2](./03.SIOPv2.png)    

The OpenID for User Authentication is **distinct from the spEcifications used for the issuance of verifiable credentials and presentetions**
described above. This specification extends OpenID Connect with the concept of a Self-Issued OpenID Provider (Self-Issued OP), 
an **OP controlled by the End-User**. 

It can be combined with OpenID for Verifiable Presentations specification.

OpenID Connect defines mechanisms by which an end-user can leverage an **OpenID Provider (OP)** 
to **release identity information** about the,eselves, such as authentication and claims, to 
a **Relying Party (RP) / Verifier**  which can act on that information. 

In this model, the RP trusts assertions made by the self-issued OP, the self-issued OpenID Provider, 
and eliminates the need for a thrid-party identity provider such as Google or Microsoft. 

The Self-Issued OP does not itself assert identity information about this End-user. 
Instead, the End-user becomes the issuer of identity information. 
Using Self-Issued OPs, the End-Users can authenticate themselves with Self-Issued ID Tokens 
signed with keys under the End-user's control and present self-attested claims directly to the RPs. 
**There is no dependency on centralized Identity Providers**.

The flow from the image can be described as follows:

1. a Relying Party, for example and API implementation that needs to autheticate the user issues an OpenID Provider Request to a Self-Issued OpenID Provider, that is a Identity Provider that implements the Self-Issued OpenID Provider API.

2. The Self-Issued OpenID Provider collect the information about the user that are in the reuest from the RP, whic is enough to identify the user to the Self-Issued OP Provider, and this then carries out Authentication and Authorization against the Wallet of the user. The wallet application of the user, under the control of the user, releases the necessary authorization information about themselves which are also digitally self-signed, and return it ot the OP provider.

3. The OP Provider returns a OP Porvider Response, a self-issued ID Token, to the Relying Party. The RP does not verify the validity of the assertion made on the idetified user, it just trust them as they are digitally self signed and can only have come from the Wallet of the user themselves. The user is in control of any information they wish to share with the RP.

---

## Use Cases

1. If you only need to present and issue Verifiable Credentials AND you do not need ID Tokens 

Use OpenID for Verifiable Credentials Presentation to issue Verifiable Credentials and 
Verifiable Presentations. 

**This standard cannot be used for user authentication**, which means that cannot be used
to produce ID Tokens.

2. When to use (SIOP v2 [Self-Issued OpenID Porvider])

(SIOP) v2 is used to handle user authentication and issue ID Tokens 
to Relying Parties (applications / APIs) usinf the user's Wallet Applications
which are implementation of SIOP-v2 compliant OPs.

3. Combining OpenID for Verifiable Credentials and Presentation with SIOP v2

It is possible to combine the standards described above:

- OpenID for the issuance of Verifiable Credentials and Presentation
- SIOP v2 [Self-Issued OpenID Porvider]

In this case Verifiable Credentials and Verifiable Presentation issuance
is made conditional to the success of a previous user authentication step.
Only when the user is successfully authenticated the Verifiable Credentials 
and Verifiable Presentation are issued.

4. Cryptographically Verifiable Claims

- OpenID for the issueance of Verifiable Credentials and Presentation
- SIOP v2 [Self-Issued OpenID Porvider]

Self-Issued OPs (OpenID Providers) [SIOP v2] can also present cryptographically 
verifiable claims issued by the third parties trusted by the RPs, when used with 
separate specifications such as OpenID for Verifiable Credentials Presentation.

This mechanism allows to enhance the ID Tokens by embedding in them verified claims 
about the subject that can be passed to the consuming application/API (Rely Party).
The advantage of embedding these signed data directly in the ID Token is that the RP 
to which this verified information is presented in the ID Token, **no longer needs** 
**to interact with Claims Issuers** and therefore the RP interacts directly and 
excluively with the user.

In the traditional flow any information concerning the subject of the ID Token is 
obtained by the RP by interacting with the corresponding Identity Provider , tipically 
one of the many well-known Identity Providers, such as Google, Microsoft, etc.

Conversely with SIOP v2 paradigm **decentralization** is possible in that the RP truts 
the enhanced self-igned information included in the ID Tokens issued by a SIOP v2 OP 
which makes the interaction with a centralized identity provider unnecessary and the role
of the **Claims Issuers** **is no longer required**.  

This is arguably the main achievements of the SIOP v2 paradigm.

---

# Decentralized System

![04.DecentralizedSystemv2](./04.DecentralizedSystem.png)     

- the user, and not a Identity Provider, is at the center of this architecture

- data is stored in a decentralized system, which is any system of storage that implements specific protocols for a decentralized storage system, irrespective of the internal implementation details. Examples of technologies that can be use to achieve this goal are: Blockchain, Azure Ledger, etc.

- there is the concept of W3C Decentralized Identifier

- Universal Resolvers are used to retrieve DID Documents

- User can authenticate using Open ID Connect SIOP Compliant Providers

## Example of user workflow

1. The user has a wallet application installed on their mobile phone or device

2. The user 

### Is a wallet application also used to create or obtain verifiable credentials or variable presentations?

Yes, a wallet application (or digital wallet) is a central component in the ecosystem of verifiable credentials (VCs) and, in relation to Verifiable Presentations (VPs), is used to:

- obtain VPs
- store VPs
- create VPs

1. Obtaining Verifiable Credentials (VCs)

The wallet app acts as a secure container to receive and store digital credentials (e.g., mobile driver’s license, diplomas, employee IDs).

- Obtaining: 

The user obtains a credential from an issuer (like a government or university) which is then sent securely to the user's wallet application.

- Secure Storage: 

The wallet acts as a digital repository to store these credentials,   
often protected by biometrics.

2. Creating Verifiable Presentations (VPs)

When a user needs to prove a claim to a verifier (e.g., proving age over 21), the wallet application is used to generate a Verifiable Presentation (VP).

- Creating: 

The wallet signs the presentation with its private key,   
proving the user is the rightful owner of the credentials.

- Selective Disclosure: 

The wallet allows users to pick which credentials to share,  
and sometimes allows them to reveal only specific data within  
a credential (e.g., sharing "over 18" instead of the full birth date).

3. Key Functions of a Wallet

- Issuance: 

Users receive credentials directly into the wallet.

- Management: 

Users can view, organize, and check the status of their credentials.

- Presentation: 

Users present credentials via QR codes or direct links to third parties.

Refs:

[JWT VC Presentation Profile - Draft](https://identity.foundation/jwt-vc-presentation-profile/)  

---

# Microsoft Verified ID

## Core concepts and facts

Microsoft Entra Verified ID uses the OpenID for Verifiable Credentials (Issuance and Presentation) specifications which describes how an issuance or presentation request, and submission should look like when issuers/verifiers communicate with a wallet (Microsoft Authenticator App).

The W3C standards are used by Microsoft Entra Verified ID to define how Verifiable Credentials should look, including which revocation status list spec they follow, etc, so that verifiers from different vendors can understand and verify a presented Verifiable Credentials (for the future interoperability).

The Self-Issued OpenID Provider (SIOP) is used for the id_token_hint attestation flow for Verifiable Credentials claims. We specify `https://self-issued.me` for the issuer instead of an identity provider. In the future it should be possible to authenticate user before the credential is issued.

Azure Active Directory and Azure Active Directory B2C services (at the moment of creating this video) do not support user authentication with Verifiable Credentials. It should be supported in the nearest future so please check the official documentation.

---

# Microsoft Verified ID example use case

![05.MicrosoftVerifiedID.HowItWorks](./05.MicrosoftVerifiedID.HowItWorks.png) 
![06.MicrosoftVerifiedID.HowItWorks](./06.MicrosoftVerifiedID.HowItWorks.png) 

1. The user is a employee at Woodgrove Inc
2. Woodgrove Inc is the issuer fo verifiable credential to the subject, the employee
3. The employee uses thier wallet application (Microsoft Authenticator App) to request the Verifiable Credentials from Woodgrove Inc
4. Woodgrove Inc authenticates the employee and based on claims known about the subject issues to them a verifiable credential that is signed with the issuer, Woodgrove Inc, private key
5. The issuer Woodgrove Inc, stores the correspoonding public key on a decentralized infrastructure
6. The verifiable credential is stored in the subject's wallet
7. The employee at Woodgrove Inc wants to access some resources, such as Apps or APIs available at a (partner) business Fabrikam Co
8. The employee releases a Verifiable Presentation with some claims to Fabrikam Co, the verifier in this interaction.
9. The verifier, Fabrikam Co in this example, can use the PKI and Decentralized Store System to verify the VP.
10. The verifier retrieves the Public Key of the issuer, together with some other metadata, i.e. the DID Document and can verify that the VP is legittimate in that it is from the claimed issuer Woodgrove Inc.
11. The verifier retrieves the Public Key of the subject, and in the same way can verify the ownership of the VP to the subject.

---

## Why is a Verifiable Presentation issued by a Wallet application signed with the public key of the issuer and the private key of the subject?


A Verifiable Presentation (VP) is a holder-controlled data structure 
used to securely share Verifiable Credentials (VCs) with a verifier. 
It uses a dual-cryptographic signature structure to prove authenticity 
and ownership.

- The Issuer's Public Key: 

Proves the underlying credential itself is legitimate. 

Because the Issuer originally signed the credential with their private key, 
the verifier uses the Issuer's public key to confirm the data was actually 
issued by the trusted party (e.g., a university or government) and hasn't 
been tampered with.

- The Subject's Private Key: 

Proves the rightful ownership of the credentials. 

The Wallet application (the holder) signs the entire presentation with 
the subject's private key. This allows the verifier to cryptographically 
confirm that the person presenting the credentials is the exact same 
individual they were originally issued to.

---

## The Trust System

DID Documents as container for public cryptographical information and metadata 
are stored by a Decentralized Public Key Infrastructure (DPKI) layer that is 
sometimes referred to as the **Trust System**.

More precisely, the term **Trust System** does not only refers to the decentrilized
storage but also to the infrastructure that tohgether with the decentrilized store
enable enitites, such as issuers and verifiers to issue and verify verifiable credentials,
respectively, **without direct interaction with each other**.

Microsoft, currently, support the following Trust Systems:

---

1. DID:ION (Identity Overlay Network) with Blockchain usage.

[DID:ION - GitHub](https://github.com/decentralized-identity/ion)    
[Microsoft Security Community Blog ION – We Have Liftoff!](https://techcommunity.microsoft.com/blog/microsoft-security-blog/ion-%E2%80%93-we-have-liftoff/1441555)  

ION is a public, permissionless, Decentralized Identifier (DID) network that implements 
the blockchain-agnostic Sidetree protocol on top of Bitcoin (as a 'Layer 2' overlay) to 
support DIDs/DPKI (Decentralized Public Key Infrastructure) at scale.

## What is Microsoft DID:ION ?

ION (Identity Overlay Network) ION is a Layer 2 open, permissionless network based \
on the purely deterministic Sidetree protocol. It allows individuals and organizations 
to create, own, and control their digital identities securely, independently of any 
central authority or corporation.

With ION it is possible to easily anchor the DID on the Bitcoin blockchain.

###  Here are the core components and features of the ION network:

- No Special Tokens or Validators:  

Unlike many crypto networks, ION relies only on the linear block chronology 
of the Bitcoin blockchain for consensus. It does not have its own utility 
token or a closed group of trusted validators.

- High Scalability: 

ION operates on Layer 2 using the deterministic Sidetree protocol. 
It solves the scalability limits of base blockchains, processing tens of 
thousands of identity operations per second.

- True Ownership: 

Because the network is decentralized, users retain absolute ownership of 
their digital identity. If a centralized service or account is deleted, 
the underlying identity and verifiable credentials remain securely in the 
user's control.

- Open Source: 

The code is open-source and maintained within the Decentralized Identity Foundation 
(DIF) and is not controlled by Microsoft.

[ION Explorer: a tool to retrive DID Document from its DID](https://identity.foundation/ion/explorer/)

---

2. DID:Web is a permission based model that allows trust using a web domain's existing reputation. No blockchain is used here.

[Register your decentralized ID for did:web](https://learn.microsoft.com/en-us/entra/verified-id/how-to-register-didwebsite)  

## What is Microsoft DID:Web ?

A DID (Decentralized Identifier) is a secure, user-owned digital identifier that allows individuals 
and organizations to control their own identity without relying on centralized third-party providers.
It provides a permission based model that allows trust using a web domain's existing reputation.

`did:web` is a specific, permission-based DID method supported by Microsoft. 

It utilizes an organization's existing website domain to establish trust and issue digital identities.

How did:web Works in Microsoft Entra Verified IDSelf-Owned Identity: Instead of relying on a blockchain, Microsoft uses the did:web method to tie an identity to a domain. For example, did:web:microsoft.com.The did.json File: Microsoft Entra generates a did.json document that contains your organization's public keys and service endpoints. This file must be hosted at a specific location on your web server: https://[your-domain]/.well-known/did.json.Verification: When your organization issues a Verifiable Credential, digital wallets (like the Microsoft Authenticator app) resolve the DID by checking your web server. If the keys match, the wallet displays a verified badge, proving the credential's authenticity.Key BenefitsNo Blockchain Required: It uses standard HTTPS and DNS infrastructure, making it much easier and cheaper for companies to adopt.Leverages Existing Reputation: It piggybacks on the trust and reputation your website domain already has.Tamper-Proof: It creates a Decentralized Public Key Infrastructure (DPKI), meaning credentials are cryptographically signed and cannot be faked or altered.

---

## DID User Agent/Wallet

- Microsoft Entra Verified ID uses Microsoft Authenticator as Wallet.

- The Authenticator 

    - creates DIDs
    - facilitates issuance and presentation requests for verifiable credentials 
    - manages the backup of the DID's seed through an encrypted wallet file.

---

## Microsoft Resolver

The Microsoft Resolver is used to interact with Decentralized Systems v ia a 
standard API.

It is an API that looks up and resolves DIDs using the `did:web` or the `did:ion` 
methods and returns the DID Document Object (DDO). 

The DDO includes DPKI metadata associated with the DID such as public keys and service endpoints.

[GitHub: Decentralized Identity Foundation](https://github.com/decentralized-identity)
[Web DID Resolver](https://github.com/decentralized-identity/web-did-resolver/blob/master/README.md) 

# How to resolve DID Documents from their DID Identifier

---

## 1. DID Discovery API

[DID Discovery](https://didproject.azurewebsites.net/docs/discovery.html)    

You can use the discovery API to fetch the DID document associated with a DID. 
This step will be necessary any time you wish to interact with a DID, including 
during authentication. 

This is a developer node and API endpoint operated by Microsoft to resolve and fetch 
Decentralized Identifier (DID) documents. It is used for handling decentralized identity 
protocols, primarily related to the ION (Identity Overlay Network) and Microsoft Entra Verified.


To discover a DID, you can send an HTTP request:

```
GET /1.0/identifiers/did:ion-test:EiDDNR0RyVI4rtKFeI8GpaSougQ36mr1ZJb8u6vTZOW6Vw HTTP/1.1
Host: beta.discover.did.microsoft.com
Accept: application/json
```

---

## 2. ION Explorer

[ION Explorer: a tool to retrive DID Document from its DID](https://identity.foundation/ion/explorer/)  

There is however, the tool called `ION Explorer` based on the ION Reolver
that can be used in the browser to resolve DID Documents from their DID 
Identifier.

---

## 3. The beta.discover.did Endpoint

The `https://beta.discover.did.microsoft.com/1.0/identifiers` endpoint may 
have been taken down and become irrelevant. 

Example:
[DID not resolving after multiple blocks #195](https://github.com/decentralized-identity/ion/issues/195)  

```
https://beta.discover.did.microsoft.com/1.0/identifiers/did:ion:EiBodPw-I3MC-Dfn-0pyK3UqlwHa9OqexA98d9GqTsTISw
```
---

### Key Features and UsesDID Resolution: 

The endpoint takes a Decentralized Identifier (e.g., did:ion-test:...) and returns the 
associated DID document in JSON format. This document contains the public keys and service
endpoints required to verify identities and authenticate users.

### Developer Integration: 

It is primarily utilized by developers building applications that require cryptographically 
secure, verifiable credentials.

### API Structure: 

Developers fetch the data by sending a standard GET HTTP request to the endpoint.

---

## Entra Verified ID Service

An issuance and verification service in Azure cloud and a REST API 
for W3C Verifiable Credentials that are signed with the `did:web` or 
the `did:ion` methods.

---

# Microsoft Verified ID Architecture

![07.MicrosoftVerifiedID.Architecture](./07.MicrosoftVerifiedID.Architecture.png) 

When Microsoft Entra Verified ID is configured by a tenant admin:

1. A Trust System type must be choosen between the two available options `did:web` or the `did:ion`. The Trust System is responsible for maintaining the verification system for verifiable credentials and its choice cannot be changed later on unless an unenrollment procedure (aka opt-out procedure) is carried out in Microsoft Entra ID and a new setup for Microsoft Entra ID Verifiable Credentials is performed.

2. A Azure Key Vault must be configured, which will generate and store the keys that are used for cryptographic operations, such as signing Verifiable Credentials. 

![07.MicrosoftVerifiedID.Architecture.ChoiceOfTheTrustSystem](./07.MicrosoftVerifiedID.Architecture.ChoiceOfTheTrustSystem.png) 

---

# Microsoft Verified ID Flow

![08.MicrosoftVerifiedID.InteractionFlow.ION](./08.MicrosoftVerifiedID.InteractionFlow.ION.png)   

AIH: The flow is particularly complex to understand I need help!

![09.MicrosoftVerifiedID.WalletRequestIssuanceDetails](./09.MicrosoftVerifiedID.WalletRequestIssuanceDetails.png)   

---

# Verifiable Credentials Use Cases

## Enterprise Scenarios

1. Onboarding and credential issuance

Enterpise have the following general onboarding needs:

- onboard employees
- onboard partners
- onboard customers

Usually a dedicated Enterprise department, i.e.HR, is in charge of 
enrolling the subject and verify their identity through legal documents, 
i.e. their government IDs, together with outher legal source of claims 
such as their degrees or proof of previous employment, etc. 

Once the identity and claims of the subject have been verified the enterprise
wants to issue to the subject some ID in a digital form so that, for example,
the subject may be authorized to access some corporate resources that are 
relevant to them.

If the issued digital IDs adhere to an open standard, such as Veriable Credentials,
then a the mechanisms of issuance, verification and access are going to be uniform.
This is the goal of inter-operability that Veriable Credentials achieve.

2. Self-service account recovery

Enterpises have also the need to streamline the account recovery procedures
to minimize its complexity and cost through self-servicing. 

Self-servicing reduces support calls to the Help-Desk.

If some verifiable credential remain securely stored on the subject's autheticator
app, these can be used to recover their account without Help-Desk intervention.

---

## Customer Scenarios

### The B2C Scenario 

> Scenario 1:

Imagine the case of a utility company, for example an energy provider responsible
for supplying their customers with utilities such as electric power and gas and 
for the maintenance to the necessary domestic infrastructure.

Following the remote detection of fault in a infrastructure component within a 
customer household or industrial premise and confirmation of the problem with the 
customr over the phone, the provider agrees with the customer to send out a qulified
technician for assessment and repair.

The customer can ask the visiting technician for a digital Verified Credential presented 
to them by means of a scannable QR displayed on the technician's device. The customer scans
the QR code and their wallet application confirms that the presented credential has been 
verified has been issued by the firm for the specific assistance case as agreed. 

> Scenario 2:

A vehicles rental company may issue verifiable credentials to their returning customers to 
make it easier for them to use their services by avoiding the need of repeated manual licence
requirements verification. Aftet a first-time procedure in which the customer's driving license 
details, i.e. the expiry date are collected, stored and verified a digital verifiable credential 
is issued to them to be stored on their wallet application for later easy and fast reuse at any of 
their branch office or even by online booking and reservations. 

The interaction would take place in the form of the tipical QR code issued to the customer at the 
first verification step, when the manual documentation checks are carried out. The employee processing
the customer details, ask the customer to scan the QR code with their scanner app at the end of a positive
verification and their wallet application carries out the interactions that lead to the issuance and 
secure storage of the digital verifiable credentials on the customer's device for later reuse.


### The Scenario of the Creation of Digital Passes  

Imagine a company plans an (exclusive) event and wishes to share some information such as catalogs, \
broshures or other exclusive information strictly with the partecipants to the event. 

The organizers may also issue digital tickets to the partecipants as opposed to the paper counterpart,  
so that the ticket will be stored on the wallet's app of each partecipant at the time of their enrollment.

Further, the event organizer may use thee same mechanism yet again to issue a free-coffe coupon for 
some partecipants, who will be able to use the coupon as a 1-time verifiable credential at the time of
the event.

---