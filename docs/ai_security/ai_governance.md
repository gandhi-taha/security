# AI governance for security people

Governance frameworks look like paperwork until you notice how much of it is security work you already do. This page maps the main frameworks to that work. It's a study aid, not legal advice.

## The frameworks

**EU AI Act** (Regulation (EU) 2024/1689). Risk-based: some practices are prohibited, high-risk systems carry most of the obligations, some systems have transparency duties, and general-purpose AI models have their own rules. Obligations differ for providers and deployers.

**NIST AI RMF 1.0.** Voluntary US framework with four functions: Govern, Map, Measure, Manage. The Generative AI Profile (NIST AI 600-1) adds risks specific to generative models.

**ISO/IEC 42001.** A certifiable management system for AI, built like ISO/IEC 27001: policy, risk assessment, controls (Annex A), and continual improvement.

## Where security work fits

| Security activity | AI Act | NIST AI RMF | ISO/IEC 42001 |
| --- | --- | --- | --- |
| Threat modelling, risk assessment | Risk management system (Art. 9) | Map, Measure | AI risk and impact assessment |
| Red teaming, robustness tests | Accuracy, robustness and cybersecurity (Art. 15) | Measure | Verification and validation controls |
| Logging and monitoring | Record-keeping (Art. 12), post-market monitoring | Manage | Operation and monitoring |
| Access control, data protection | Data governance (Art. 10) | Govern, Manage | Data controls |
| Incident response | Serious incident reporting | Manage | Nonconformity and corrective action |
| Supplier and model inventory | Provider and deployer duties | Govern | Supplier and third-party controls |

## What I'd bring to a governance review

- An inventory of AI systems with owners, models used and data touched
- A threat model per system, kept next to the risk assessment
- Test evidence: prompt injection and abuse tests, with results and fixes
- Logging and retention decisions, agreed with data protection
- A clear line for who approves new models, tools and data sources
