<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Can you explain me the difference between an Azure Application Registration and its Service Principal? I need a refresher on these concepts.

An Azure App Registration is the definition (identity template) of an application in Entra ID, while the Service Principal is the actual security identity that gets permissions and is used at runtime to access resources.[^1_1][^1_4][^1_6]

## Core concepts

- **App Registration (Application object)**
    - Describes your app: name, redirect URIs, client ID, credentials, API permissions, single/multi‑tenant, etc.[^1_2][^1_4]
    - There is exactly one application object per app, and it lives in a single “home” tenant.[^1_4][^1_1]
    - Think of it as the **blueprint** or “global definition” of the app.[^1_8][^1_1]
- **Service Principal (Enterprise Application)**
    - Is the security principal that represents that app *in a specific tenant* and is what you assign roles/RBAC to.[^1_10][^1_1][^1_4]
    - There can be many service principals (one per tenant) all pointing to the same application object.[^1_1][^1_8]
    - This is the identity that actually signs in and executes, using the app’s credentials to get tokens and call resources.[^1_6][^1_1]


## How they relate

- When you create an App Registration, Entra ID automatically creates a corresponding Service Principal in the same tenant (visible under “Enterprise applications”).[^1_2][^1_4][^1_10]
- The link between them is the Application (client) ID: one app registration ↔ one local service principal in that tenant, plus optional additional service principals in other tenants if the app is multi‑tenant or consented elsewhere.[^1_10][^1_1]
- The application object acts as a template; each service principal instance inherits the shared properties but has tenant‑specific things like role assignments, consent, and policies.[^1_8][^1_1]


## Practical way to remember

- **App Registration = “Who is the app?”**
    - Identity metadata, redirect URIs, exposed APIs, client secrets/certs, supported account types.[^1_4][^1_6]
- **Service Principal = “What can it do *here*?”**
    - Assigned RBAC roles on subscriptions/resource groups/resources, directory‑specific permissions and consent, sign‑in settings per tenant.[^1_6][^1_8][^1_10]

A common analogy: the App Registration is like a class definition, and each Service Principal is an instance of that class in a given tenant.[^1_12][^1_2]
<span style="display:none">[^1_11][^1_3][^1_5][^1_7][^1_9]</span>

<div align="center">⁂</div>

[^1_1]: https://learn.microsoft.com/en-us/answers/questions/1133189/azure-ad-application-vs-service-principal-concept

[^1_2]: https://stackoverflow.com/questions/65922566/what-are-the-differences-between-service-principal-and-app-registration

[^1_3]: https://www.linkedin.com/pulse/app-registrations-service-principals-enterprise-most-sher-ali-khan-ekvhf

[^1_4]: https://learn.microsoft.com/en-us/entra/identity-platform/app-objects-and-service-principals

[^1_5]: https://www.youtube.com/watch?v=WVNvoiA_ktw

[^1_6]: https://itomation.ca/azure-app-registrations-vs-service-principals-simplified/

[^1_7]: https://www.reddit.com/r/AZURE/comments/qh2qe8/azure_differences_from_app_registration_service/

[^1_8]: https://www.linkedin.com/pulse/demystifying-azure-app-registration-service-principals-ajay-pawar-opdhf

[^1_9]: https://www.youtube.com/watch?v=uqwWtRoB6CE

[^1_10]: https://jacobtechlearn.com/understanding-app-registrations-service-principals-and-enterprise-applications-in-azure-ad/

[^1_11]: https://learn.microsoft.com/en-us/shows/on-dotnet/service-principles-and-app-registration

[^1_12]: https://heusser.pro/p/whats-the-difference-between-an-entra-id-app-registration-and-an-enterprise-app-4yyib4g1sk5a/

