# Microsoft IQ Offerings: A Technical Overview

---

## The Big Picture: What Are Microsoft IQ Offerings?

Microsoft has been steadily assembling a portfolio of **IQ-branded intelligence layers** across its platform — each one targeting a distinct domain of a modern enterprise. Rather than a single AI product, the Microsoft IQ family represents a strategic pattern: take a well-established Microsoft workload surface, embed AI reasoning and agentic capabilities into it, and expose that intelligence through a coherent, governed interface.

The common thread is the idea of an *intelligence quotient* applied to a business domain: data analytics, business workflows, developer productivity, web experiences, and the foundational AI infrastructure itself. Together, these offerings compose a unified AI strategy that spans the full breadth of how a business operates — from the data warehouse to the meeting room, from the developer's terminal to the customer-facing website.

---

## The Offerings

### Microsoft Fabric IQ

**Fabric IQ** is the intelligence layer built into [Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/), Microsoft's unified analytics platform. It introduces a novel concept to the data world: an **ontology** — a formal, graph-based model that describes business entities (such as customers, orders, or products) and the relationships between them in a semantically rich way.

Unlike a traditional semantic model (a tabular Power BI dataset), an ontology in Fabric IQ is queryable via **GQL (Graph Query Language)** and can be traversed to answer questions that span entity relationships in ways that flat tabular models struggle with. Fabric IQ sits above the data lake and the semantic model, providing a business-context layer that AI agents and reporting tools can reason over.

**Area of application:** Enterprise analytics, data governance, knowledge graphs, BI modernisation.  
**Business benefit:** Enables AI agents and analysts to query business knowledge, not just data tables. Reduces the gap between raw data and business meaning.

---

### Foundry IQ

**Foundry IQ** is the intelligence layer of [Azure AI Foundry](https://azure.microsoft.com/en-us/products/ai-foundry), Microsoft's platform for building, evaluating, and operating AI applications. Foundry IQ extends the Foundry platform with deeper reasoning capabilities, agent orchestration, and the ability to build AI workflows that consume grounded knowledge from enterprise sources.

Foundry IQ is positioned as the backbone for organisations that want to build **custom AI agents**: connecting large language models (LLMs) to internal tools, APIs, and data — with the governance, observability, and safety controls expected in an enterprise setting.

**Area of application:** AI application development, agent orchestration, LLM ops, enterprise AI governance.  
**Business benefit:** Accelerates the path from AI prototype to production-grade agent, with built-in evaluation, tracing, and responsible-AI guardrails.

---

### Azure OpenAI (as an IQ foundation)

**Azure OpenAI Service** is the cloud-hosted delivery of OpenAI's frontier models — including GPT-4o, DALL-E, and Whisper — wrapped in Microsoft Azure's enterprise controls. It acts as the **model substrate** that many of the IQ offerings rely on under the hood.

What distinguishes Azure OpenAI from OpenAI's direct API is the enterprise envelope: data stays within the customer's Azure tenant, models can be fine-tuned on proprietary data, and the service inherits Azure's compliance portfolio (ISO 27001, SOC 2, GDPR, and more).

**Area of application:** Natural language processing, code generation, image synthesis, speech-to-text, across any workload.  
**Business benefit:** Access to state-of-the-art models without sacrificing data residency, compliance, or security posture.

---

### Web IQ

**Web IQ** brings AI intelligence to the **web and digital experience** layer. It is Microsoft's answer to the question: *how does a business expose AI-powered search, summarisation, and discovery to its customers and users through web channels?*

Web IQ integrates retrieval-augmented generation (RAG) patterns, grounded in a business's own content, into web experiences — enabling conversational search, intelligent content recommendations, and AI-assisted navigation across public-facing and internal web properties.

**Area of application:** Customer-facing portals, intranet search, e-commerce, knowledge bases, digital marketing.  
**Business benefit:** Elevates web experiences from keyword search to intent-aware, conversational interaction — without requiring the business to build and host its own AI infrastructure.

---

### Work IQ

**Work IQ** is the intelligence layer for **Microsoft 365 and workplace productivity**. It is closely related to Microsoft 365 Copilot but operates as a broader framework that also incorporates integrations with developer tooling, including GitHub Copilot CLI.

Work IQ applies AI to the daily tasks of knowledge workers: drafting documents, summarising meetings, extracting action items, searching across organisational knowledge, and automating repetitive workflows in Teams, Outlook, SharePoint, and beyond.

**Area of application:** Knowledge worker productivity, document creation, meeting intelligence, enterprise search, workflow automation.  
**Business benefit:** Reclaims time for high-value work by automating routine cognitive tasks, with grounding in organisational data and governance through Microsoft 365's security model.

---

### Microsoft IQ (the Platform Layer)

**Microsoft IQ** is the overarching intelligence platform that ties the domain-specific IQ offerings together. It represents the governance, identity, and orchestration fabric that ensures AI actions across Fabric IQ, Foundry IQ, Work IQ, Web IQ, and Azure OpenAI are coherent, auditable, and aligned with organisational policy.

Think of Microsoft IQ as the **control plane**: it enforces who can invoke which AI capabilities, what data those capabilities can access, how outputs are logged, and how AI agents are composed across domain boundaries.

**Area of application:** Enterprise AI governance, cross-workload agent orchestration, compliance, identity-aware AI.  
**Business benefit:** Provides the single pane of glass for managing AI risk, cost, and usage across the entire Microsoft IQ portfolio.

---

## How the Offerings Fit Together

The Microsoft IQ family is not a set of isolated products — it is a layered architecture:

```
┌─────────────────────────────────────────────────────────────┐
│                     Microsoft IQ (Control Plane)            │
│         Governance · Identity · Orchestration · Audit       │
├──────────────┬──────────────┬──────────────┬────────────────┤
│  Fabric IQ   │  Foundry IQ  │   Work IQ    │    Web IQ      │
│ (Analytics)  │ (AI Dev)     │ (Workplace)  │ (Web/Digital)  │
├──────────────┴──────────────┴──────────────┴────────────────┤
│                   Azure OpenAI Service                       │
│              (Model Infrastructure Layer)                    │
└─────────────────────────────────────────────────────────────┘
```

A concrete example of this interplay: a business analyst uses **Work IQ** (via Microsoft 365 Copilot) to ask a question about customer churn. The query is routed through **Microsoft IQ** (control plane) which checks permissions and then invokes **Fabric IQ** to traverse the business ontology. The answer is generated by a model served through **Azure OpenAI**, with the interaction logged for compliance. Meanwhile, the same churn model was built by a data science team using **Foundry IQ**, and its insights surface on the company's customer portal via **Web IQ**.

This composability is the strategic differentiation: each IQ offering delivers standalone value, but the combined platform enables AI that spans the full enterprise without creating data silos or governance gaps.

---

## Comparison at a Glance

| Offering | Primary Domain | Target Audience | Core Capability | Deployment Surface |
|---|---|---|---|---|
| **Fabric IQ** | Analytics & Data | Data engineers, analysts | Ontology, graph queries, knowledge modelling | Microsoft Fabric |
| **Foundry IQ** | AI Development | AI engineers, developers | Agent building, LLM ops, evaluation | Azure AI Foundry |
| **Azure OpenAI** | Model Infrastructure | All of the above | LLM inference, fine-tuning | Azure (all regions) |
| **Web IQ** | Digital Experience | Web teams, marketers | AI-powered search, RAG for web | Web apps, portals |
| **Work IQ** | Workplace Productivity | Knowledge workers, IT | Copilot in M365, workflow automation | Microsoft 365, Teams |
| **Microsoft IQ** | Platform Governance | Enterprise architects, admins | Cross-IQ orchestration, governance | Azure / M365 tenant |

---

## Cost Considerations

Pricing across the IQ family follows Azure's and Microsoft 365's standard consumption and subscription models. The table below summarises the cost model for each offering at the time of writing; always verify current pricing on Microsoft's official pages as these figures are subject to change.

| Offering | Pricing Model | Key Cost Drivers | Notes |
|---|---|---|---|
| **Fabric IQ** | Fabric capacity units (CUs) / reserved | Capacity tier, query volume, storage | Part of Microsoft Fabric SKU; dedicated capacity recommended for production |
| **Foundry IQ** | Pay-as-you-go + reserved capacity | Token consumption, agent executions, storage | Costs scale with LLM invocations routed through Azure OpenAI |
| **Azure OpenAI** | Per-token (input + output) | Model choice, prompt size, fine-tuning jobs | GPT-4o significantly cheaper than GPT-4 Turbo per token; PTU (provisioned throughput) available |
| **Web IQ** | Consumption-based | Search index size, query volume, AI enrichment steps | Builds on Azure AI Search and Azure OpenAI; separate pricing for each |
| **Work IQ** | Per-user/month add-on | Seat count, M365 plan tier | Microsoft 365 Copilot licence required; currently ~$30 USD/user/month (subject to change) |
| **Microsoft IQ** | Included within tenant / platform | Governance overhead; no separate direct cost | Cost is the sum of the underlying IQ services consumed |

### Cost Optimisation Tips

- **Right-size Fabric capacity:** Use auto-pause on Fabric capacity during off-hours for dev/test environments.
- **Use PTU for Azure OpenAI** when you have predictable, high-volume inference workloads — provisioned throughput units offer lower per-token cost at scale.
- **Cache RAG results** in Web IQ scenarios where the same queries repeat frequently, reducing Azure OpenAI call volume.
- **Pilot Work IQ** with a targeted group before broad rollout to validate productivity gains against licence cost.
- **Foundry IQ evaluation costs** can add up quickly during experimentation phases — use smaller, cheaper models for bulk evaluation runs.

---

## Summary

The Microsoft IQ portfolio represents a coherent, layered bet on enterprise AI: rather than a single product, Microsoft is embedding intelligence into every major workload surface its customers already use. For businesses already invested in the Microsoft ecosystem, the IQ offerings provide a path to AI adoption that preserves existing governance, identity, and compliance investments.

The key to unlocking the full value is treating the offerings not as point solutions but as **composable building blocks** — with Microsoft IQ as the governing layer that ensures they work together safely, accountably, and at enterprise scale.

---

*Article prepared for the Würth Phoenix / Neteye blog — Q3 2026*
