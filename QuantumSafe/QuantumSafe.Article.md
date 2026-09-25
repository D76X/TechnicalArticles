# Quantum-safe security: why the migration starts now

John Savill's video, [What Quantum Safe Is and Why We Need It to Stay Secure][1], raises a practical security question: will today's protected data and trusted identities remain protected if a sufficiently powerful quantum computer becomes available? The answer is not to wait for a machine that can break current cryptography. It is to understand what is at risk, use the new standards carefully, and begin a measured migration while there is time to test it.

## What does "quantum-safe" mean?

Quantum-safe, also called post-quantum cryptography (PQC), means cryptographic methods designed to resist attacks from both conventional computers and quantum computers. These algorithms run on ordinary computers; they are not a form of quantum computing or quantum encryption.

The main concern is public-key cryptography. A sufficiently capable, fault-tolerant quantum computer running Shor's algorithm could break mathematical problems on which widely used systems such as RSA and elliptic-curve cryptography depend. These systems are used for tasks such as agreeing on secret keys and signing data. A successful attack could expose protected communications or let an attacker forge signatures and impersonate trusted systems.

That does not mean every encryption algorithm becomes useless. Quantum computers affect symmetric cryptography differently from public-key cryptography. NIST's ML-KEM standard establishes a shared secret that can then be used with symmetric algorithms for encryption and authentication; the migration is not a reason to abandon approved symmetric cryptography or invent replacement parameters.[2][3]

## Why act before a quantum computer arrives?

Cryptographic migrations take years. Algorithms are embedded in protocols, operating systems, certificate authorities, applications, hardware security modules, network appliances, and products supplied by other organizations. Replacing an algorithm in one library does not update all the systems that depend on it.

There is also a present-day confidentiality risk known as "harvest now, decrypt later." An attacker can copy encrypted traffic or stored data now and keep it in case future technology makes it readable. This matters most when the information must remain confidential for a long time: for example, sensitive health, financial, government, or intellectual-property data. The relevant question is not only "When might a quantum computer arrive?" but also "How long must this data remain secret, and how long will it take us to protect it?"[2][6][9]

Digital signatures need attention too. A signature helps a recipient verify who signed a message, software package, or certificate and whether it was changed. If the signing algorithm becomes vulnerable, an attacker may be able to create convincing forgeries. Long-lived certificate authorities and code-signing identities therefore belong in a migration plan alongside encrypted network traffic.[9]

## The standards: three building blocks

In August 2024, the U.S. National Institute of Standards and Technology (NIST) finalized its first three PQC standards and encouraged organizations to begin transitioning.[2] They address two different jobs:

| Standard | What it does | Plain-English explanation |
| --- | --- | --- |
| **ML-KEM (FIPS 203)** | Key establishment | Lets two parties establish a shared secret over a public connection. They then use that secret with symmetric cryptography to protect the actual conversation. A KEM is not, by itself, a general-purpose way to encrypt files or messages.[3] |
| **ML-DSA (FIPS 204)** | Digital signatures | Lets a system sign and verify data, software, or certificates.[4] |
| **SLH-DSA (FIPS 205)** | Digital signatures | Provides another standardized signature option, based on hash functions.[5] |

The distinction matters in a real migration: switching a connection's key establishment does not automatically replace the signatures in its certificates, and updating a certificate authority does not automatically make every client able to use the new certificates. These are related but separate workstreams.

PQC is also different from quantum key distribution. PQC is software-based cryptography designed to run on today's networks and computers. Quantum key distribution uses specialized hardware and a quantum communication channel. For most organizations, the practical migration work is about adopting standardized PQC through supported products and protocols, not building a quantum network.[6]

## What this means for Microsoft environments

Microsoft's public roadmap is a useful signal for organizations using Windows, Azure, and Microsoft 365. In June 2026, Microsoft said it was accelerating its Quantum Safe Program, with a goal of transitioning its own products and services to PQC by 2029. It recommends setting a migration strategy, building crypto-agility into systems, creating a living inventory of cryptography, and modernizing protocols such as TLS 1.3.[7] The 2029 date is Microsoft's program goal; it is not a universal customer deadline or a guarantee that every Microsoft service will support every PQC feature on that date. Check the current documentation for the specific service and deployment you use.

Microsoft has also described work at shared cryptographic foundations, including SymCrypt, which is used across Windows, Azure, Microsoft 365, and other platforms. Its 2025 engineering update discussed ML-KEM and ML-DSA support through Windows cryptographic APIs for early adopters, and hybrid TLS work in SymCrypt-OpenSSL.[8] This illustrates why PQC adoption happens in stages: algorithm support in a cryptographic library must be followed by support in protocols, services, applications, and the systems at both ends of a connection.

One concrete Windows example is Active Directory Certificate Services (AD CS). Microsoft's current documentation says AD CS supports ML-DSA for certificate signing. The listed requirements include Windows Server 2025 with the May 2026 security update or later for AD CS servers, and Windows 11 versions 24H2 or 25H2 with the October 2025 update or later for clients. PQC support requires a Cryptography API: Next Generation (CNG) provider; legacy Cryptographic Service Providers are not supported.[9] Microsoft also notes that ML-DSA is for signatures, not encryption or key exchange, and that certificate consumers must be tested for compatibility.[10]

This is meaningful progress, but it is not proof that an entire certificate-based environment is quantum-safe. A certificate must be issued, distributed, trusted, and understood by every relevant client and service. Other parts of a deployment may still rely on quantum-vulnerable algorithms. Use Microsoft's deployment instructions and test with the actual devices, applications, and certificate chains in your environment before changing production systems.[9][10]

## A practical migration plan

A useful first phase is discovery and risk reduction, not an organization-wide algorithm switch.

1. **Assign ownership and scope.** Treat PQC as a multi-year security and architecture program. Identify accountable teams for infrastructure, applications, identity, procurement, and data governance. Check the regulatory and contractual requirements that apply to your organization.
2. **Build a cryptographic inventory.** Record where public-key algorithms and certificates are used: TLS endpoints, VPNs, identity systems, code signing, device management, APIs, databases, backups, HSMs, appliances, cloud services, and third-party products. Capture algorithm and protocol versions, owners, renewal dates, dependencies, and supplier support. Keep the inventory current; an initial spreadsheet that quickly goes stale is not enough.[7]
3. **Prioritize by risk and lead time.** Start with data that must stay confidential for many years, exposed connections that can be recorded, long-lived root and code-signing keys, and systems that are hard to upgrade. Include supplier and hardware replacement timelines: a system may need a long procurement cycle before it can support a new algorithm.
4. **Map the supported path.** Prefer current NIST-standardized algorithms and implementations provided by your platform, cloud service, protocol, or cryptographic library; check current standards errata as well. Confirm exactly what is supported, on which versions, and whether both ends of a connection are compatible. TLS 1.3 is a useful modern baseline, but using TLS 1.3 alone does not mean a connection is using post-quantum key establishment.[7]
5. **Pilot and measure.** Test realistic end-to-end flows in a representative environment. PQC can change key, signature, and certificate sizes, which may affect network messages, device limits, latency, performance, logging, or compatibility with older systems. Test certificate chains and trust stores as well as handshakes. Where a standards-based hybrid mode is offered, follow the implementation guidance rather than inventing a combination of algorithms yourself.
6. **Design for change.** Crypto-agility means being able to change algorithms without redesigning every application. Keep cryptographic choices configurable where appropriate, automate certificate renewal, document dependencies, and test upgrade and rollback procedures. That work improves resilience to future standards changes and helps address outdated cryptography today.

NIST's migration project likewise advises organizations to begin planning and transitioning rather than wait for a final quantum-computing timeline.[6] There is no single switch that makes a large estate quantum-safe. Progress comes from knowing where cryptography is used, protecting the highest-risk data and identities first, and upgrading each dependency through tested, supported paths.

## The bottom line

Quantum-safe security is a cryptographic modernization effort, not a prediction about the exact year a quantum computer will arrive. The standards are now published, the migration will be gradual, and the systems that need the most attention are often the least visible. Start with an inventory, prioritize by data lifetime and upgrade lead time, and build the ability to change cryptography safely. That is how organizations can reduce risk now and be ready to adopt PQC across their Microsoft environments as supported capabilities mature.

## References

[1] John Savill's Technical Training, ["What Quantum Safe Is and Why We Need It to Stay Secure" (YouTube)](https://www.youtube.com/watch?v=5--yBhgDrXM).

[2] National Institute of Standards and Technology (NIST), ["NIST Releases First 3 Finalized Post-Quantum Encryption Standards" (August 13, 2024)](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards).

[3] NIST, [FIPS 203: Module-Lattice-Based Key-Encapsulation Mechanism Standard](https://csrc.nist.gov/pubs/fips/203/final).

[4] NIST, [FIPS 204: Module-Lattice-Based Digital Signature Standard](https://csrc.nist.gov/pubs/fips/204/final).

[5] NIST, [FIPS 205: Stateless Hash-Based Digital Signature Standard](https://csrc.nist.gov/pubs/fips/205/final).

[6] NIST, [Post-Quantum Cryptography project: standards and migration](https://csrc.nist.gov/projects/post-quantum-cryptography).

[7] Mark Russinovich, Microsoft Security Blog, ["Accelerating the quantum-safe timeline" (June 30, 2026)](https://www.microsoft.com/en-us/security/blog/2026/06/30/microsoft-advances-quantum-safe-security-as-the-risk-timeline-shifts/).

[8] Mark Russinovich and Michal Braverman-Blumenstyk, Microsoft Security Blog, ["Quantum-safe security: Progress towards next-generation cryptography" (August 20, 2025)](https://www.microsoft.com/en-us/security/blog/2025/08/20/quantum-safe-security-progress-towards-next-generation-cryptography/).

[9] Microsoft Learn, [Post-Quantum Cryptography in AD CS overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/post-quantum-cryptography-overview).

[10] Microsoft Learn, [What is ML-DSA support in AD CS?](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/ml-dsa-overview).