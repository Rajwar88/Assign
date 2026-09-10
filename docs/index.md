# Liminal Technical Writer Candidate Assignment

Welcome to the **Liminal Technical Writer Assignment & WebHelp Portal**. This interactive documentation suite presents solutions for the Liminal Technical Writer Evaluation Assignment, hosted as a production-ready documentation site using **MkDocs Material** and deployed via **GitHub Pages**.

---

## Executive Summary

Liminal is a premier crypto wallet infrastructure provider enabling exchanges to manage hot wallets, automate fund transfers, and stake institutional digital assets securely. Technical documentation at Liminal bridges the gap between complex blockchain infrastructure, API schemas, security enforcement, and developer experience.

This documentation suite demonstrates a holistic approach to technical writing:

1. **Precision API Engineering Documentation** (Q1): Formally specifying address risk screening integrations with security partner `cube3`.
2. **User-Centric Product Guide Redesign** (Q2): Conducting an in-depth audit of the Solana Staking Guide and delivering a complete task-oriented rewrite.
3. **RESTful Architecture & API Standardization** (Q3): Evaluating legacy non-REST endpoints and redesigning them according to modern REST standards, HTTP semantics, and RFC 7807 error specifications.
4. **Portfolio Showcase & Track Record** (Q4): Highlighted technical documentation projects with clear scope breakdown and impact metrics.

---

## Assignment Navigation Hub

<div class="grid cards" markdown>

-   :material-api: __[Q1: Send Many Transaction API Documentation](q1-api-documentation.md)__

    ---

    Documentation update for `/sendmanytransaction` incorporating `cube3` threat screening, `screeningFlag` body parameters, `200 OK` success schemas, and atomic `400 Bad Request` rejection rules for high-risk addresses (> 80 risk score).

    [:octicons-arrow-right-24: View Q1 API Reference](q1-api-documentation.md)

-   :material-wallet-outline: __[Q2: Solana Staking Guide Audit & Rewrite](q2-solana-staking-guide.md)__

    ---

    8-point usability audit of Liminal's Solana Staking Guide, accompanied by a complete step-by-step rewritten developer and customer guide with state diagrams and troubleshooting tables.

    [:octicons-arrow-right-24: View Q2 Audit & Guide](q2-solana-staking-guide.md)

-   :material-swap-horizontal: __[Q3: RESTful API Redesign](q3-rest-api-redesign.md)__

    ---

    Architectural standardization of 3 non-REST legacy endpoints (`/gettransfers`, `/sendmanytransaction`, `/getwalletbalance`) into resource-oriented RESTful URIs with idempotency, status code matrix, and migration strategies.

    [:octicons-arrow-right-24: View Q3 REST Redesign](q3-rest-api-redesign.md)

-   :material-briefcase-check: __[Q4: Work Samples & Portfolio](q4-portfolio-samples.md)__

    ---

    Selected technical writing samples showcasing documentation for developer platforms, API references, architecture guides, and developer tooling.

    [:octicons-arrow-right-24: View Q4 Portfolio](q4-portfolio-samples.md)

</div>

---

## Technical Documentation Standards & Principles

All documentation produced in this submission adheres to the following core tenets:

- **Accuracy & Completeness**: Every parameter, header, data type, and error response is explicitly defined without ambiguity.
- **Developer-First Ergonomics**: Code examples are provided in cURL, Python, and Node.js with copy-paste readiness.
- **Visual Clarity**: Complex control flows and state transitions are illustrated using **Mermaid sequence & state diagrams**.
- **Edge Case Governance**: Clear guidance on failure modes, error handling, risk thresholds, and fallback behaviors.

---

## Quick Site Info

| Attribute | Specification |
| :--- | :--- |
| **Documentation Engine** | MkDocs 1.6.1 + MkDocs Material 9.7.6 |
| **Target Hosting** | GitHub Pages (`gh-pages` branch via GitHub Actions) |
| **Diagram Engine** | Mermaid.js (Native Markdown integration) |
| **Format** | GitHub Flavored Markdown (GFM) + PyMdown Extensions |
