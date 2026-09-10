# Q4: Technical Writer Work Samples & Portfolio

!!! summary "Live Portfolio Site — Prakash Chand Rajwar"
    **Senior Technical Writer**  
    **Live Portfolio**: [https://prakash92.netlify.app/](https://prakash92.netlify.app/)  
    **Experience & Resume**: [prakash92.netlify.app/resume/](https://prakash92.netlify.app/resume/)  
    **Skills & Certifications**: [prakash92.netlify.app/skills_certs/](https://prakash92.netlify.app/skills_certs/)  
    **Direct Work Samples**: [prakash92.netlify.app/work-samples/fap-guide/](https://prakash92.netlify.app/work-samples/fap-guide/introduction_to_freight_audit_and_pay/)

---

## Technical Writing Portfolio Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CORE WRITING SPECIALIZATIONS                    │
│ ──────────────────────────┬──────────────────────────┬─────────────────┤
│ REST & Webhook APIs       │ FinTech & Supply Chain   │ System Specs    │
│ OpenAPI 3.0/3.1           │ EDI 210/310 Mappings     │ Architecture    │
│ Docs-as-Code CI/CD        │ Python & JSON Scripts    │ User Workflows  │
└──────────────────────────┴──────────────────────────┴─────────────────┘
```

---

## Featured Live Work Samples

### Sample 1: Freight Audit and Pay (FAP) Master Guide

<div class="feature-card" markdown>

#### Overview & Context
Designed and authored the end-to-end enterprise documentation suite for **Freight Audit and Pay (FAP)** systems, covering financial auditing, multi-carrier invoice processing, data flow architectures, and payment reconciliation.

- **Live Link**: [Freight Audit and Pay Guide](https://prakash92.netlify.app/work-samples/fap-guide/introduction_to_freight_audit_and_pay/)
- **Document Type**: Enterprise Integration & Operational Guide
- **Target Audience**: Systems Integrators, Financial Audit Operations, Logistics Product Managers
- **Tools & Tech**: MkDocs Material, Markdown, Mermaid.js (Data Flow Diagrams), PlantUML

#### Key Documentation Modules
- **Introduction**: [Overview of FAP Workflows](https://prakash92.netlify.app/work-samples/fap-guide/introduction_to_freight_audit_and_pay/)
- **Data Flow Diagrams**:
  - [Rate and Pay Data Flow](https://prakash92.netlify.app/work-samples/fap-guide/rate_and_pay/)
  - [Match and Pay Data Flow](https://prakash92.netlify.app/work-samples/fap-guide/match_and_pay/)
  - [Execute and Pay Data Flow](https://prakash92.netlify.app/work-samples/fap-guide/execute_and_pay/)
- **Functional Flows**:
  - [UI Flow & Audit Flow](https://prakash92.netlify.app/work-samples/fap-guide/ui_flow/)
  - [Invoice 2.0 EDI & Functional Flow](https://prakash92.netlify.app/work-samples/fap-guide/invoice_2_0_edi_flow/)

</div>

---

### Sample 2: Technical Integration & Automation (EDI 210/310 & Python Scripts)

<div class="feature-card" markdown>

#### Overview & Context
Authored developer-facing integration specifications and automated payload generator guides for enterprise EDI systems and custom REST microservices.

- **Live Links**:
  - [Python Script for Invoice JSON Generation](https://prakash92.netlify.app/work-samples/fap-guide/invoice_create_json/)
  - [EDI 210 & 310 Mapping Guide](https://prakash92.netlify.app/work-samples/fap-guide/edi_210_and_310_mapping/)
  - [IBM Sterling Layer Configuration](https://prakash92.netlify.app/work-samples/fap-guide/sterling_layer_configuration/)
- **Document Type**: Technical Integration Specification & Developer Guide
- **Target Audience**: EDI Engineers, Backend Developers, Enterprise Integration Specialists

#### Scope of Work & Contributions
- **Automated Scripting**: Documented Python automation scripts that convert unstructured carrier invoices into validated JSON payloads.
- **Data Element Mapping**: Formatted complex X12 EDI 210 (Motor Freight) and 310 (Ocean Freight) loop/segment mapping matrices.
- **Middleware Config**: Authored step-by-step setup guides for IBM Sterling B2B Integrator communication channels.

</div>

---

### Sample 3: CarrierGo Platform User Guide

<div class="feature-card" markdown>

#### Overview & Context
Created a comprehensive user onboarding and system operational manual for **CarrierGo**, a multi-modal freight booking and tracking platform.

- **Live Links**:
  - [Introduction to CarrierGo](https://prakash92.netlify.app/work-samples/carriergo-guide/IntroductiontoCarrierGo/)
  - [Getting Started with CarrierGo](https://prakash92.netlify.app/work-samples/carriergo-guide/getting_started_with_carriergo/)
  - [CarrierGo Change Control Log](https://prakash92.netlify.app/work-samples/carriergo-guide/ChangeControl/)
- **Document Type**: End-User Manual & Release Change Log
- **Target Audience**: Logistics Dispatchers, Carrier Partners, Operations Managers

</div>

---

### Sample 4: "The Tech Writer as an Engineer" — Automated Docs-as-Code Pipeline

<div class="feature-card" markdown>

#### Overview & Context
Engineered a complete **Docs-as-Code** continuous integration and deployment pipeline to publish developer webhelp sites automatically.

- **Live Details**: [Tech Writer as an Engineer Pipeline](https://prakash92.netlify.app/#the-tech-writer-as-an-engineer-automated-docs-pipeline)
- **Document Type**: Engineering Architecture & Tooling Pipeline
- **Tools & Tech**: MkDocs Material, Git, GitHub Actions, PyMdown Extensions, Python, Custom CSS badges

#### Key Capabilities & Impact
- **Automated CI/CD**: GitHub Actions workflow builds, tests links, and publishes static site assets on git push to `main`.
- **Quality Gates**: Pre-commit linters enforce Markdown style standards and prevent broken internal links.
- **Custom Styling**: Developed responsive CSS badges for HTTP methods, API response codes, and callout blocks.

</div>

---

## Live Work Samples Directory

| Project Title | Documentation Focus | Direct Live Access |
| :--- | :--- | :---: |
| **Freight Audit & Pay Guide** | Enterprise Audit, Data Flows, & Functional Workflows | [View Live Guide](https://prakash92.netlify.app/work-samples/fap-guide/introduction_to_freight_audit_and_pay/) |
| **Python Invoice JSON Generator** | Automation Script & Payload Specification | [View Python Doc](https://prakash92.netlify.app/work-samples/fap-guide/invoice_create_json/) |
| **EDI 210/310 Mapping Spec** | X12 EDI Data Element Mapping Matrix | [View EDI Mapping](https://prakash92.netlify.app/work-samples/fap-guide/edi_210_and_310_mapping/) |
| **CarrierGo User Manual** | Platform Onboarding & Operations | [View CarrierGo Guide](https://prakash92.netlify.app/work-samples/carriergo-guide/IntroductiontoCarrierGo/) |
| **Prakash's Full Portfolio Site** | Complete Resume, Skills, & Portfolio | [Visit Portfolio Site](https://prakash92.netlify.app/) |

---

## Technical Writing Methodologies & Best Practices

1. **Docs-as-Code Workflow**: Writing documentation in Markdown, version-controlled via Git, validated via linters, and deployed automatically via GitHub Actions pipelines.
2. **Task-Oriented Structure**: Organizing content around specific user goals (e.g. *Rate & Pay*, *Match & Pay*, *Execute & Pay*) with clear prerequisites and step-by-step instructions.
3. **Automated Verification**: Validating all API request payloads, code snippets, and script parameters against live environments prior to deployment.
