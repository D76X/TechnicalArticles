# Global Secure Access with Microsoft Entra

![Global Secure Access Architecture](./20260311-MicrosoftGlobalSecurePrivateAccess-01.jpg)

This article expands on the topic introduced in the earlier blog post
[Secure Access to Applications with Azure](https://www.neteye-blog.com/2025/09/secure-access-to-applications-with-azure/),
which focused on securing access to an on-premises web application using Microsoft Entra Application Proxy.

[Global Secure Access](https://learn.microsoft.com/en-us/entra/global-secure-access/) is Microsoft's
unified framework for securing access to private resources, Microsoft 365 services, and internet
destinations through identity-aware controls. It brings **Microsoft Entra Private Access** and
**Microsoft Entra Internet Access** together in a single administrative and policy model, managed
through the Microsoft Entra admin center.

Together, these two components form Microsoft's **Security Service Edge (SSE)** solution. SSE is a
network security architecture class that delivers security controls as a cloud service, closer to the
user, rather than requiring traffic to be backhauled through a central corporate perimeter. SSE is a
subset of the broader SASE (Secure Access Service Edge) model; SASE additionally includes SD-WAN
capabilities, which are outside the scope of this article.

Global Secure Access addresses the following goals:

1. Replace many traditional VPN scenarios with identity-aware, segmented access to private resources.
2. Reduce the need to expose broad network access to remote users.
3. Apply granular, identity-centric access control to private, Microsoft 365, and internet resources.
4. Extend policy enforcement consistently across managed endpoints and supported network paths.

---

## Key terms

- **GSA**: Global Secure Access — the umbrella product name in the Microsoft Entra admin center
- **Global Secure Access Client**: the endpoint software used to forward supported traffic through the service
- **SSE**: Security Service Edge — the service class that Global Secure Access belongs to
- **Microsoft Entra Private Access**: the ZTNA capability for private resource access
- **Microsoft Entra Internet Access**: the SWG capability for internet and SaaS traffic
- **ZTNA**: Zero Trust Network Access
- **SWG**: Secure Web Gateway
- **CASB**: Cloud Access Security Broker
- **Conditional Access**: the Microsoft Entra policy engine used to control access based on identity and device conditions

---

# Basic architecture of the Global Secure Access solution

Global Secure Access uses Microsoft's global network presence to connect managed users to private
resources, Microsoft 365, and internet destinations through a unified, identity-aware service plane.

## The Global Secure Access Client

The **Global Secure Access Client** is installed on managed endpoints and is responsible for
forwarding supported traffic to the Global Secure Access service. It integrates at the operating
system level and is typically deployed and managed through endpoint management tools such as
Microsoft Intune or another MDM solution. Updates to the client are managed by Microsoft.

The client forwards traffic according to the enabled **traffic forwarding profiles** and the
applicable policy configuration. Traffic handling depends on which profiles are enabled and what
scenarios are supported — it is not simply a function of the client being present on the device.

At a high level, the client establishes three separate channels to the SSE service, one for each
traffic category:

- **Private traffic** — access to private resources via Microsoft Entra Private Access
- **Microsoft 365 traffic** — access to supported Microsoft 365 SaaS services
- **Internet traffic** — access to internet destinations and third-party SaaS via Microsoft Entra Internet Access

These channels are transparent to the user. Each category of traffic is processed independently by
the service according to the policies and capabilities configured for the tenant.

The client authenticates the user to the Microsoft Entra ID tenant and uses this identity when
communicating with the service. Access tokens issued by Microsoft Entra ID accompany traffic
forwarded by the client, so the service always has the necessary claims to evaluate which policies
apply to each individual request.

## The private network connector

The **Microsoft Entra private network connector** is used specifically for Microsoft Entra Private
Access. It is a lightweight, Microsoft-managed component installed on Windows Server systems that
have line of sight to the private resources being published through the service.

The connector uses an **outbound-only** communication model. No inbound firewall rules are required
because the connector initiates the connection to the Microsoft service over ports 443 and 80.
Traffic in both directions flows over the session that the connector establishes.

Connectors should be deployed on multiple Windows Server instances for redundancy and continuous
availability during updates. They are **stateless**: performance scales with request rate, payload
size, CPU capacity, and network throughput rather than with user or session count. Microsoft states
that with standard web traffic, an average machine can handle around 2,000 requests per second. For
more advanced deployments, connectors can be organized into logical groups; the Microsoft
documentation covers this topic in detail.

---

# Conditional Access and Global Secure Access

A defining architectural property of Global Secure Access is its deep integration with
**Microsoft Entra Conditional Access**.

Global Secure Access extends Conditional Access to network traffic — not only to cloud application
sign-in events. Microsoft refers to this as **Universal Conditional Access through Global Secure Access**.

Global Secure Access supports policy-driven handling of three traffic categories:

- **Private traffic**: traffic to private applications and resources
- **Internet traffic**: traffic to internet and SaaS destinations
- **Microsoft traffic**: traffic to supported Microsoft 365 services

In practice, Conditional Access can require conditions such as:

- multifactor authentication
- compliant device
- acceptable sign-in risk level
- compliant network, where supported

The link between identity and traffic handling is important to understand. When the Global Secure
Access Client authenticates the user to the Entra tenant, it acquires **access tokens** for the
service. These tokens carry claims about the user identity, group memberships, and device state.
The service evaluates these claims at runtime to determine which policies and security profiles to
apply to the traffic it receives. This is what makes the solution identity-centric rather than
purely network-based, and what aligns it with Zero Trust principles.

---

# Tenant restrictions, compliant network, and named locations

Traditional **Named Locations** in Microsoft Entra are defined by IP ranges or geographic regions
and can be used as conditions in Conditional Access policies. They remain useful in some scenarios
but have operational limits: administrators must maintain trusted source ranges manually, and static
IP trust can be difficult to sustain in dynamic or distributed environments.

Global Secure Access improves this area with two distinct features:

- **Compliant network**
- **Universal Tenant Restrictions**

## Compliant network

[Compliant network check in Global Secure Access](https://learn.microsoft.com/en-us/entra/global-secure-access/how-to-compliant-network)
allows administrators to require that access originates from the Global Secure Access service path
for the correct tenant. This provides two practical benefits:

1. It removes the need to maintain a curated list of trusted egress IP addresses.
2. It removes the need to hairpin traffic through VPN infrastructure in order to preserve a known
   source IP for IP-based Conditional Access policies.

For example, a Conditional Access policy in `contoso.com` that requires compliant network can only
be satisfied by users whose traffic passes through the Global Secure Access service for the
`contoso.com` tenant. A user from `fabrikam.com` cannot satisfy that control even from a managed
device, and a user on a non-compliant network path is blocked regardless of device state.

**Important limitation:** compliant network check is currently **not supported for Private Access
applications**.

## Tenant Restrictions and Universal Tenant Restrictions

[Tenant Restrictions v2](https://learn.microsoft.com/en-us/entra/external-id/tenant-restrictions-v2#step-3-enable-tenant-restrictions-on-windows-managed-devices)
control how managed users and devices interact with external Microsoft Entra tenants. There are two
distinct protection layers:

### 1. Authentication plane protection

This layer blocks sign-in attempts that use unauthorized external identities from managed devices.
A practical example: a malicious insider attempting to sign in to a personal or attacker-controlled
tenant from a managed device would be blocked, preventing data from being exfiltrated through that
external account.

### 2. Data plane protection

This layer addresses attacks that bypass authentication entirely. Examples include:
- anonymously joining a Teams meeting hosted by a malicious tenant
- accessing SharePoint files via anonymous sharing links in a malicious tenant
- copying an access token from another device and replaying it on the managed device

Data plane protection forces authentication for all such attempts and blocks access if the
resulting authentication does not meet policy.

**Universal Tenant Restrictions** extend this model by integrating Tenant Restrictions with Global
Secure Access. The client tags all outbound traffic with tenant restriction policy at the operating
system level, regardless of browser, application, or remote network path. This makes enforcement
consistent across all traffic that passes through the service, and covers remote network connectivity
in addition to individual managed endpoints.

The practical outcome is that managed users can only authenticate to and access resources in
external tenants that are explicitly authorized by the organization's tenant policy.

---

# Global Secure Access: Private Access, Internet Access, and Microsoft traffic

Global Secure Access provides a unified model for managing network access from managed endpoints
through:

- the **Global Secure Access Client** on each managed endpoint
- **Conditional Access** policies in the tenant
- **traffic forwarding profiles** that determine which traffic categories the client handles
- **security policies** applied per category
- **private network connectors**, required for Private Access scenarios

Each traffic category — Private Access, Internet Access, and Microsoft 365 traffic — solves a
distinct problem and should be treated as a separate capability in architecture design.

---

# Private Access through Global Secure Access

![Global Secure Private Access Architecture](./20260311-MicrosoftGlobalSecurePrivateAccess-01.jpg)

**Microsoft Entra Private Access** is Microsoft's ZTNA capability for private resources. It is
designed for the scenario where a managed endpoint needs access to resources on a private network —
whether on-premises or in a private cloud — without relying on traditional broad-access VPN
connectivity.

The main design goals are:

1. Eliminate classic VPN-style full-network access in favour of segmented, per-resource access.
2. Allow access based on user identity and device state, not network location.
3. Support non-HTTP protocols as well as web traffic.
4. Apply access policies at a granular per-application or per-segment level.

## Private Access application model

Private Access works with **enterprise applications** that act as containers defining the private
resources to be published through the service. Microsoft documentation describes two main models:

1. **Quick Access**: a single broad access segment that allows organizations to migrate quickly by
   publishing a wider set of resources under one application object. Useful when the overhead of
   defining individual segments is not yet justified.

2. **Global Secure Access applications**: individual enterprise application objects that each define
   a precise access segment. Each application is specified by a combination of:
   - IP addresses or IP ranges / CIDR blocks
   - Fully Qualified Domain Names (FQDNs)
   - protocols
   - port ranges

   These definitions should be non-overlapping to ensure predictable policy evaluation. This model
   allows organizations to progressively move from broad access toward precise, least-privilege
   access segments — replacing the implicit broad access of a VPN with explicit, policy-controlled
   access to each resource.

## Relationship to Application Proxy

Private Access is **not** the same thing as Microsoft Entra Application Proxy, and the two are not
interchangeable.

- **Application Proxy** publishes web applications and provides application-layer capabilities such
  as header-based and integrated web SSO scenarios. It operates at Layer 7.
- **Private Access** is designed for broader private resource access and covers TCP- and UDP-based
  scenarios that go beyond HTTP and HTTPS. It operates at **Layer 4 (Transport Layer)**, meaning it
  is aware of IP, port, and protocol but not of application-layer content.

Because of this architectural difference:
- Private Access is **not** a universal replacement for every Application Proxy scenario (SSO
  capabilities differ).
- Internet Access is **not** a replacement for Application Proxy either — Internet Access is a SWG
  for internet-bound traffic, not a web application publishing tool.

## Protocol coverage

Because Private Access operates at Layer 4, it can cover a wide range of protocols beyond HTTP,
including:

- RDP
- SSH
- SMB
- FTP
- custom TCP applications
- custom UDP applications

This is one of the main reasons why it is a strong candidate for replacing many VPN-based access
patterns, particularly for infrastructure access (RDP, SSH) and file share access (SMB).

## Private name resolution

An important architectural challenge in Private Access is **private DNS resolution**. A remote
user on a managed device needs to resolve private domain names maintained by the organization's
internal DNS server — but Private Access is not a VPN, so there is no traditional full-network
path between the endpoint and the private DNS server.

Microsoft Entra Private Access addresses this through a **service-assisted name resolution model**
in which the client, the SSE service, the private network connector, and the private DNS server
cooperate. The mechanism works at a high level as follows:

1. For each Private Access application, a local DNS suffix (`.globalsecureaccess.local`) is
   configured in the client.
2. When the user on the managed endpoint attempts to resolve a private name, the client appends the
   suffix and forwards the query to the SSE DNS service.
3. The SSE DNS service checks its cache:
   - If a cached result exists, it responds immediately to the client.
   - If no cached result exists, it strips the suffix and forwards the resolution request to the
     on-premises private network connector via the connector's relay mechanism.
4. The connector resolves the name against the private DNS server it is configured to use.
5. The result is returned to the SSE DNS service, cached, and forwarded to the client.

The important outcome is that the endpoint can resolve private names without requiring a full
network tunnel to the corporate network. The Microsoft documentation is the authoritative reference
for the exact current behaviour and configuration requirements.

---

# Internet Access through Global Secure Access

![Global Secure Internet Access Architecture](./20260311-MicrosoftGlobalSecureInternetAccess-01.jpg)

**Microsoft Entra Internet Access** is the **identity-aware Secure Web Gateway** component of
Global Secure Access. It controls user access to internet destinations and SaaS services through
centrally managed, identity-driven policies.

## Web filtering policies

Web filtering policies define individual access rules. Each policy specifies:

- a set of **web categories** (e.g., Social Platforms, Gambling, Entertainment)
- specific **FQDNs**
- an **action**: Allow or Block

Example policies:

| Name | Categories | FQDNs | Action |
|---|---|---|---|
| Allow Social | Social Platforms | — | Allow |
| Allow Work Sites | — | `*.microsoft.com`, `*.mycorporate.*` | Allow |
| Block Social and Gambling | Gambling, Entertainment, Social | — | Block |

## Internet Access security profiles

Web filtering policies are grouped into **security profiles** (also called Internet Access Security
Profiles). Each security profile collects a set of web filtering policies and assigns each a
relative priority within the profile.

Each security profile is assigned an overall **priority value** (between 0 and 65,000). Lower
priority values are evaluated first, so blocking profiles with low priority values will filter
traffic before any subsequent allow profiles are evaluated.

Example security profiles:

**Profile: Marketing Work** — priority 500
- Priority 100: Allow Social
- Priority 200: Allow Work Sites
- Priority 300: Block Social and Gambling

**Profile: General Work** — priority 600
- Priority 200: Allow Work Sites
- Priority 300: Block Social and Gambling

This allows different user groups to have different internet access behavior. In the example above,
marketing users are allowed access to social platforms, while general employees are not.

## Linking security profiles to users via Conditional Access

The connection between security profiles and user groups is implemented through **Conditional Access
policies**. Each Conditional Access policy for Internet Access:

- targets a specific user group
- specifies `GSA-Internet` as the traffic type
- assigns one Internet Access security profile
- must have its action set to **Allow**

Example policies:

**CAP: Internet - Marketing**
- Group: Marketing
- Type: GSA-Internet
- Security Profile: Marketing Work
- Action: Allow

**CAP: Internet - All Employees**
- Group: Employees
- Type: GSA-Internet
- Security Profile: General Work
- Action: Allow

The `Allow` / `Block` action at the Conditional Access level is a traffic-forwarding switch:
- **Allow** means the traffic for that user group is forwarded from the client to the service.
- **Block** means all internet traffic for that group is stopped before it reaches the service —
  regardless of any web filtering policies.

The Allow/Block action in the individual **web filtering policies** applies to specific categories
or FQDNs within the traffic that has already been forwarded.

## Identity-centric enforcement

The attentive reader might ask: how does the SSE service know which security profile to apply to
a given client's traffic?

The answer is in the access tokens. When the Global Secure Access Client authenticates on the
tenant, it acquires **access tokens** with an expiration of approximately one hour. These tokens
carry identity and group membership claims. When the client forwards internet traffic to the service,
these tokens accompany the traffic. The service evaluates the claims to determine which Conditional
Access policies apply, which security profile is assigned, and therefore which web filtering
policies to enforce.

This is what makes Internet Access identity-centric rather than network-location-centric, and
what satisfies the Zero Trust principle of continuous verification.

---

# Remote network locations with Global Secure Access

[Remote network connectivity](https://learn.microsoft.com/en-us/entra/global-secure-access/concept-remote-network-connectivity)
provides an alternative to installing the Global Secure Access Client on every individual endpoint.
It is designed for **branch office or campus scenarios** where all network traffic from a site can
be routed into Global Secure Access through a configured network device (for example, a router or
firewall that supports the IPsec connectivity model required by the service).

In this model, the network device at the site acts as the entry point to the SSE service. Individual
machines on the site do not require the client. The Global Secure Access policy configuration in
the tenant remains the same; the operational difference is in how traffic reaches the service.

## Important limitation

At the time of writing:

- **Private Access traffic can only be acquired with the Global Secure Access Client.**
- **Remote networks cannot be assigned to the Private Access traffic forwarding profile.**

Remote network connectivity is therefore **not** a substitute for the client in Private Access
scenarios. Its current scope is:

- **Microsoft 365 traffic** scenarios
- **Internet traffic** scenarios

In addition, remote network connectivity uses a bulk bandwidth licensing model rather than the
per-user model that applies to the client. The specific conditions and thresholds should be verified
against current Microsoft licensing documentation before adoption.

---

# Licensing and cost considerations

Licensing for Global Secure Access should be verified carefully before implementation, as feature
availability can change and entitlements vary by capability.

As a general baseline, Microsoft states that users need **Microsoft Entra ID P1 or P2** to use
Microsoft Entra Private Access and Microsoft Entra Internet Access. The following licenses include
the relevant entitlements:

- Microsoft Entra ID P1
- Microsoft Entra ID P2
- Microsoft Entra Suite

Feature-specific licensing may vary. Remote network connectivity has its own conditions and minimum
thresholds. Always verify current Microsoft licensing guidance before finalizing an architecture
or deployment plan.

---

# Current limitations and design caveats

Before adopting Global Secure Access in production, these current limitations should be understood:

- **Private Access is not a drop-in replacement for every Application Proxy scenario.** SSO and
  application-layer capabilities available in Application Proxy are not present in Private Access.
- **Compliant network check is currently not supported for Private Access applications.** It applies
  to internet and Microsoft 365 traffic scenarios.
- **Remote networks cannot currently acquire Private Access traffic.** Private Access requires the
  Global Secure Access Client on the endpoint.
- **Traffic handling depends on enabled forwarding profiles**, not simply on client presence.
- **Feature availability and licensing entitlements should always be validated** against current
  Microsoft documentation before design sign-off.

---

# Conclusion

Global Secure Access represents a significant step in Microsoft's Zero Trust strategy. It brings
together private resource access, internet access, and Microsoft 365 traffic control under a single
identity-aware, cloud-delivered service model.

For architects, the key value is not only feature consolidation but the structural shift from
broad, network-location-based trust to policy-driven, segmented, identity-centric access enforcement.

The three capabilities should be understood as distinct:

- **Microsoft Entra Private Access** — ZTNA for private resources, covering TCP/UDP protocols, a
  strong candidate to replace many VPN-based access patterns.
- **Microsoft Entra Internet Access** — SWG for internet and SaaS traffic, applying identity-aware
  web filtering through Conditional Access.
- **Universal controls** — tenant restrictions, compliant network, and named locations — applied
  consistently across endpoints and supported network paths where the features are available.

Used correctly, Global Secure Access can reduce dependency on legacy connectivity models, improve
security posture through continuous identity verification, and simplify access architecture by
consolidating policy management in the Microsoft Entra admin center.

---

# References

[Secure Access to Applications with Azure — NetEye Blog](https://www.neteye-blog.com/2025/09/secure-access-to-applications-with-azure/)
[Microsoft Entra Global Secure Access](https://learn.microsoft.com/en-us/entra/global-secure-access/)
[What is Global Secure Access?](https://learn.microsoft.com/en-us/entra/global-secure-access/overview-what-is-global-secure-access)
[Microsoft Entra Private Access](https://learn.microsoft.com/en-us/entra/global-secure-access/concept-private-access)
[Global Secure Access admin center quickstart](https://learn.microsoft.com/en-us/entra/global-secure-access/quickstart-access-admin-center)