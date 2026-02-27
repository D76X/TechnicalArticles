# Microsoft Entra Verified ID  

[AZ-500-SC-100-Understanding and Using Verifiable Credentials - John Savill](https://www.youtube.com/watch?v=BxLSSH_EHjo&t=4834s)   

[Defeat Deep Fakes and Imposters with Verified ID and Face Check - John Savill](https://www.youtube.com/watch?v=58j2PLW-M5k&t=8s)   

[Introduction to Microsoft Entra Verified ID Core Concepts and Use Cases Tech Mind Factory](https://www.youtube.com/watch?v=rek6KDEgGjE&t=913s)   

## Ecosystem Overview

![][01.EcosystemOverview]  

### Holder 

A **Holder is a role an entity might perform** by 

- possessing one or more verifiable credentials 
- generating verifiable presentations from them. 

Example holders include students, employees, and customers.
Typically this would be the Wallet App installed on a person's (=the entity) mobile.
A holder is usually, but not always, a subject of the verifiable credentials they hold.
Holders store their credentials in **credential repositories** (i.e the Wallet App).
Holders will **present** the verifiable credential so that they can be verified in order to grant them (the subject)
some kind of access credentials to some resources.

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

An entity about which claims are made. Example subjects include human beings, animals, and things. In many cases the holder of a verifiable credential is the subject.


### Verifier

**A Verifiyer is a role an entity performs** by receiving one or more verifiable credentials, 
optionally inside a verifiable presentation, for processing. 
Example verifiers include employers, security personnel, and websites.

Other specifications might refer to the Verifier role as the **Relying Party**.


### Verifiable Data Registry

A **Verifiable Data Registry is a role a system** might perform by **mediating** the creation and verification of identifiers, 
keys, and other relevant data, such as verifiable credential schemas, revocation registries, issuer public keys, and so on, 
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

### Claim 

A Claim an assertion made about a subject.

### Subject 

The subject is a logical object about which claims are made, i.e. a person, vehicle, employee, etc.

### Credential 

**A credential is a set of one or more claims made by an issuer**. 
A **verifiable credential is a tamper-evident credential** that has authorship that can be cryptographically verified. 
Verifiable credentials can be **used to build verifiable presentations**, which can also be cryptographically verified. 
The claims in a credential can be about different subjects.

--

###  Entity 

Is A thing with distinct and independent existence, such as a person, organization, or device that performs one or more roles in the ecosystem.

---

###  Decentralized Identifier 

A **DID** a **portable URL-based identifier**, also known as a DID, **associated with an entity**. 

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

---

[01.EcosystemOverview]: ./01.EcosystemOverview.png