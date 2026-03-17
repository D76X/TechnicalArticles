# Global Secure Access with Microsoft Entra

![Global Secure Acces Architecture](./20260311-MicrosoftGlobalSecureAccess-01.jpg)

This article expands on the more specific case described by the blog post [Secure Access to Applications with Azure](https://www.neteye-blog.com/2025/09/secure-access-to-applications-with-azure/), 
where the main aim is to secure access to an on-premises application using Microsoft Entra Application Proxy.

[Global Secure Acces](https://learn.microsoft.com/en-us/entra/global-secure-access/) is a unified solution that conisderably 
extends the general concepts discussed in the context of the Microsoft Entra Application Proxy, which covers only the scenario
of private access to (on-premises) web applications.

Global Secure Acces makes it possible and easy to achieve the following goals:

1. completely remove the need for a VPN and the corresponding...
2. 
 

SWG: Secure Web Gateway to control access to resources available on the public Internet
ZTNA: Zero Trust Network Access to control access to private resources
CASB: Cloud Access Security Broker to enforce access policies, discover private resources and understand threats
SDWAN to provide WAN S2S connectivity (NOT IN FOCUS)


One advantage is the massive scale of the Microsoft Global WAN, where Global Access runs.

This works with the Global Secure Access Client software (GSAC) installed on the user's system, which establishes a secure tunnel to the Global Secure Edge and all its (managed) services. It is important to point out that the GSAC integrates at the OS level and not at the application software level; therefore,it no application on the user's system can bypass it once installed. All the traffic originating from and coming to the user's machine will be routed through the GSAC.





The Access Policy resource in Microsoft Entra ID has been updated so that, during its definition, one of the selectable options for the target of the access policy is the Global Secure Access. By selecting the Global Secure Access as the target of an access policy, the options available for selection at the step of the traffic profiles to which the policy will be applied will be the following:

Microsoft 365 traffic
Internet traffic
Private traffic
The GSAC establishes dedicated channels to the Microsoft Entra Security Edge for each of these three types, and the SSE will apply to each traffic the corresponding set of controls and policies.

Tenant restrictions defined on the tenant of the employee to whom the managed device is given are enforced through the GSAC at the operating system level. This means that it will be impossible to circumvent these restrictions on the managed device.

Adaptive Access: Enable Global Secure Access signaling in Conditional Access

Named Locations is a tenant-level Microsoft Entra ID feature in which locations can be defined in different ways, such as ranges of IP addresses or entire geographical regions, and then used in the definition of access policies to apply traffic controls to allow, disallow, or require further actions such as the typical MFA interaction.

The Enable Global Secure Access signaling in Conditional Access setting enhances and simplifies the use of locations in access policy definitions; It is possible to select all compliant network locations, which in practice ????



This compliant network check is specific to the tenant in which it is configured. For example, if you define a Conditional Access policy requiring compliant network in contoso.com, only users with the Global Secure Access or with the Remote Network configuration are capable of passing this control. A user from fabrikam.com will not be able to pass contoso.com's compliant network policy.

The compliant network is different than IPv4, IPv6, or geographic locations you might configure in Microsoft Entra. Administrators are not required to review and maintain compliant network IP addresses/ranges, strengthening the security posture and minimizing the administrative overhead.

Microsoft Entra private network connectors
Connectors only send outbound requests. The outbound traffic is sent to the service and to the published resources and applications. You don't have to open inbound ports because traffic flows both ways after a session is established. You also don't have to configure inbound access through your firewalls.

Connectors are stateless, and the number of users or sessions doesn't affect them. Instead, they respond to the number of requests and the payload size. With standard web traffic, an average machine can handle 2,000 requests per second.

CPU and the network define connector performance. CPU performance is needed for TLS encryption and decryption, whereas networking is important to get fast connectivity to the applications and the online service.


Private Access

The most distinctive fact about Private Access is that it is designed to work hand in hand with the Microsoft Entra Application Registrations (Apps), which in this context will be referred to as Private Access Applications.

The overarching goal of Private Access is to allow organizations to replace their classic broad-access VPN with a completely identity-centric, zero-trust selective segmented access without overlaps.


There are two types of Private Access Applications:

Classic Microsoft Entra Application Registrations for Private Access: each representing a segment.
Quick Access Application: a single broad traffic segment.
In the context of Private Access, the specification of each non-overlapping segment is based on the following data:

IP, IP ranges or CIDR
Fully Qualified Domain Name (FQDN)
Port ranges



Private Access covers any traffic built on top of the network protocols UDP and TCP; The important detail is that PA analyzes the traffic as LAYER-4 (Transport Layer) rather than LAYER-7 (Application Layer); therefore, it is not a substitute for Azure Application Proxy, as some of the capabilities that are available as part of the agent that powers Azure Application Proxy use the application layer.

A notable example is the Single-Sign-On (SSO), which is a feature of the Azure Application Proxy but not of the Private Access.



RDP
SSH
FTP
TCP
UDP
SMB
Printers
others

An important aspect of Private Access is how domain name resolution (DNS) is accomplished; this is important because it allows a GSAC to resolve private domains, whose records are on a private domain server owned by the organization and possibly internal to the corporate network, without having a reference to this private DNS service or even being actually tunneled to the corporate network at all.

Internet Access

This works for all the traffic at LAYER-7 (Application Layer); therefore, HTTP(S) and the controls are based on Web Filtering Policies (WFP) that are defined in Microsoft Entra ID.

Web filtering policies are essentially named groupings of web categories or FQDNs for which a certain action is specified.

For example, a WFP can be created with the following details:

Name: Social
Categories: Social Platforms
FQDN: https://www.facebook.com/
Action: Allow
Name: WorkOnly
Categories: Any
FQDN: https://www.**microsoft**.com/, https://www.**mycorporate**.**/,
Action: Allow
Name: Ban Social, Entertainment and Gambling
Categories: Gambling, Entertainment, Social
FQDN: https://www.**microsoft**.com/, https://www.**mycorporate**.**/, **gambling**,...
Action: Block


Internet Access Security Profiles (IASP) are collections of web filtering policies that will be applied to sets of groups of users within a tenant. Each IASP specifies its own unique value of priority between 0 and 65000, and each arranges its set of WFP by specifying for each a number that indicates its relative priority with respect to the other WFP within the same WFP.

IASP Name: Marketing Work
Priority: 500
WFP-Priority-100: Social
WFP-Priority-200: Work
WFP-Priority-300: Ban Social, Entertainment and Gambling
IASP Name: General Work
Priority: 600
WFP-Priority-200: Work
WFP-Priority-300: Ban Social, Entertainment and Gambling

This scheme guarantees that the Internet traffic originating from different groups of users can be treated by the WFP without conflicts. For example, the tenant administrators might want to forbid all gambling activities for all users, and while for some users also the social sites should be barred, for some other users, such as for the marketing team, they should not be barred.

The smaller the priority value, the sooner the IASP is applied to the traffic; therefore, blocking actions with the small priority values will filter out the corresponding traffic. This also means that the IASP with allowed actions will generally have smaller priorities than the corresponding block actions; for example, when the tenant wants to block access to social media for all user groups except the marketing team.

The last missing piece of this mechanism is the link between the IASP and the user groups. This is important because it is implemented through the well-established construct of Conditional Access Policy. The tenant administrator can now create CAPs such as the following, with the only restrictions being that each of these CAPs can only be assigned one Internet Access Security Profile (IASP) and that their action must be set to allow:


CAP Name: Internet Marketing
Group: Marketing
Type: GSA-Internet
Internet Access Security Profile (IASP): Marketing Work
Action: Allow
CAP Name: Internet All Employees
Group: Employee
Type: GSA-Internet
Internet Access Security Profile (IASP): General Work
Action: Allow
The restriction that the Internet Access Security Profile (IASP) and that their action must be set to allow may appear counterintuitive and somehow redundant, as the WFPs on the IASPs already express their own action as Action: Block or Action: Allow.

The action specified at the Conditional Access Policy level for the GSA-Internet traffic is a switch that either blocks completely the traffic for that group of users when set to Block or allows the traffic to be forwarded from the GSA client to the set of web filtering policies specified for the corresponding Internet Access Security Profile (IASP). This means that if set to block all the Internet traffic for the users with the GSA client that are part of the Entra group, i.e., marketing, it will be blocked, and therefore they will not be able to reach any resource on the Internet at all.
The action specified at the web filtering policy instead is applied when the traffic routed from the GSA client for the specified group of Entra users through the Entra Secure Edge matches the definitions in the Category or FQDN specifiers.
The attentive reader at this point might wonder how this Global Secure Internet Access feature of Microsoft Entra can possibly be identity-centric and adhere to the tenets of Zero Trust. On the surface it might seem that this feature might actually work at the network level whereby the traffic is routed through the GSA over to the Microsoft Entra Security Edge, and then it is inspected to decide which traffic must be filtered out and therefore prevented from reaching out to its destination on the Internet.

This model is almost right but misses one very important detail: How can the Microsoft Entra Security Edge determine which set of web filtering policies should be applied to the traffic that it receives from a specific GSA client?

User log in to their managed devices with their Microsoft Entra ID identity, which is also the identity used by the GSA Client installed on the device. During the authentication of the GSA Client on the corresponding tenant the authentication token that are assigned to the GSA Client will be used to acquire access tokens, which also have an expiration time of about 1 hour, with the expanded claims for the Microsoft Entra Security Edge. These access tokens hold the information that Microsoft Entra needs to deteremine which web filtering policies should be applied to the traffic that it receives from a specific GSA client.

The result of this mechanism is the oveall solution satisfy the requirement of being identity-centric and also the Zero Trust tenet of continual verification.



Cost Aspects of the GSA
Licensing overview

Microsoft Entra Internet Access for Microsoft services capabilities are included in the following per-user licenses:

Microsoft Entra ID P1
Microsoft Entra ID P2
Microsoft Entra Suite license




References

Microsoft Entra Global Secure Access
Microsoft Entra Private Access
Microsoft Entra Internet Access for all apps


Global Secure Access Remote Network Connectivity
Common remote network connectivity scenarios

Microsoft Entra private network connectors

Microsoft Entra Security Service Edge Overview - John Savill - YouTube
Deep Dive on Microsoft Entra Private Access - John Savill - YouTube
Deep Dive on Microsoft Entra Internet Access - John Savill - YouTube
