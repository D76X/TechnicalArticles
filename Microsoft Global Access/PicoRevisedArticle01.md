# Global Secure Access with Microsoft Entra

![Global Secure Private Access Architecture](./20260311-MicrosoftGlobalSecurePrivateAccess-01.jpg)

This article expands on the topic introduced in the earlier blog post [Secure Access to Applications with Azure](https://www.neteye-blog.com/2025/09/secure-access-to-applications-with-azure/), which focused on securing access to an on-premises web application with Microsoft Entra Application Proxy.

[Global Secure Access](https://learn.microsoft.com/en-us/entra/global-secure-access/) is Microsoft's unified framework for securing access to private resources, Microsoft 365 services, and internet destinations through identity-aware controls. It brings Microsoft Entra Private Access and Microsoft Entra Internet Access together in a single administrative and policy model.【call_qqiVeij6mF4lmXIi1RzyE0lb-0】【call_qqiVeij6mF4lmXIi1RzyE0lb-1】

Global Secure Access makes it possible to achieve the following goals:

1. Replace many traditional VPN scenarios with identity-aware access to private resources.
2. Reduce the need to expose broad network access to remote users.
3. Apply granular, identity-centric access control to private and internet resources.
4. Extend policy enforcement beyond private applications to Microsoft 365 and internet traffic.

## Key terms

- **GSA**: Global Secure Access
- **Global Secure Access Client**: the endpoint client used to forward supported traffic through the service
- **SSE**: Security Service Edge
- **ZTNA**: Zero Trust Network Access
- **SWG**: Secure Web Gateway
- **CASB**: Cloud Access Security Broker

# Basic architecture of the Global Secure Access solution

Global Secure Access uses Microsoft's global network presence to connect users to private resources, Microsoft 365, and internet destinations through a unified service plane.【call_qqiVeij6mF4lmXIi1RzyE0lb-1】

## The Global Secure Access Client

The **Global Secure Access Client** is installed on managed endpoints and is responsible for forwarding supported traffic to the Global Secure Access service. It works at operating system level and is typically deployed and managed through endpoint management tools such as Microsoft Intune or another MDM solution.

The client forwards traffic according to the enabled **traffic forwarding profiles** and the applicable policy configuration. This point is important: traffic handling depends on what profiles are enabled and what scenarios are supported, not simply on the presence of the client alone.【call_qqiVeij6mF4lmXIi1RzyE0lb-4】【call_GSyrj7jUAJiktG9K4OdUI2le-9】

At a high level, the client can participate in forwarding three traffic categories:

- **Private traffic**
- **Microsoft 365 traffic**
- **Internet traffic**

These traffic categories are then processed by the service according to Microsoft Entra policy, user context, device context, and the specific capabilities enabled in the tenant.【call_qqiVeij6mF4lmXIi1RzyE0lb-1】

## The private network connector

The **Microsoft Entra private network connector** is used for Microsoft Entra Private Access. It is a lightweight Microsoft-managed component installed on Windows Server systems that have line of sight to the private resources being published through the service.【call_rDTvDpF3n8ZdGwVN6DT0Qp9S-0】【call_rDTvDpF3n8ZdGwVN6DT0Qp9S-2】

Like the Microsoft Entra Application Proxy connector, it uses an outbound-only communication model. No inbound firewall opening is required for the connector itself, because the connector initiates the connection to the Microsoft service.【call_rDTvDpF3n8ZdGwVN6DT0Qp9S-0】

Connectors should be deployed redundantly on multiple Windows Servers. They are stateless, and Microsoft recommends using more than one connector for resilience and scale.【call_rDTvDpF3n8ZdGwVN6DT0Qp9S-0】

Connector performance depends more on request rate, payload size, CPU, and network capacity than on raw user or session count. Microsoft states that, with standard web traffic, an average machine can handle around 2,000 requests per second.
