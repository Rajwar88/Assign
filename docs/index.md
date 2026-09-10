# Liminal Technical Writer Candidate Assignment

Welcome to the **Liminal Technical Writer Assignment & WebHelp Portal**. This interactive documentation suite presents solutions for the Liminal Technical Writer Evaluation Assignment, hosted as a production-ready documentation site using **MkDocs Material** and deployed live via **GitHub Pages**.

---

## Executive Evaluation Matrix & Task Deliverables

| Task # | Evaluated Topic | Primary Deliverables | Key Architectural / Documentation Highlights | Status |
| :---: | :--- | :--- | :--- | :---: |
| **Q1** | **API Endpoint Update** *(Cube3 Risk Screening)* | [Q1 API Reference](q1-api-documentation.md) | Standardized **HTTP 400 Bad Request** threat rejections for Risk Score > 80; documented `screeningFlag` parameter; array-based error schema returning first blocked address details. | <span class="status-badge status-200">100% Complete</span> |
| **Q2** | **Solana Staking Audit & Rewrite** | [Q2 Audit & Guide](q2-solana-staking-guide.md) | 8-point usability audit matrix; exact `0.00228288 SOL` rent-exempt reserve checklist; Mermaid epoch state diagram (`Funded` -> `Activating` -> `Active` -> `Deactivating` -> `Inactive/Withdrawable`); Figment commission disclosure; rent reclamation details. | <span class="status-badge status-200">100% Complete</span> |
| **Q3** | **RESTful API Redesign** | [Q3 REST Redesign](q3-rest-api-redesign.md)<br>• [GET /v2/transfers/{tx_id}](q3-1-get-transfers.md)<br>• [POST /v2/transfers/batch](q3-2-send-transactions.md)<br>• [GET /v2/wallets/{id}/balances](q3-3-get-balances.md) | Single transfer lookup (`GET /v2/transfers/{tx_id}`) + collection listing; batch transfer with root-level `asset` and `Idempotency-Key` headers; hierarchical wallet balances; complete OpenAPI 3.0.0 spec; phased deprecation Gantt chart. | <span class="status-badge status-200">100% Complete</span> |
| **Q4** | **Work Samples & Portfolio** | [Q4 Portfolio Showcase](q4-portfolio-samples.md) | Highlights top public GitHub repositories ([`docs-cycle-time-metrics`](https://github.com/Rajwar88/docs-cycle-time-metrics), [`docs-prioritization-engine`](https://github.com/Rajwar88/docs-prioritization-engine), [`Technical-Writing-Portfolio`](https://github.com/Rajwar88/Technical-Writing-Portfolio)), live FAP guide, EDI 210/310 mapping, and CarrierGo user guide. | <span class="status-badge status-200">100% Complete</span> |

---

## Assignment Navigation Hub

<div class="grid cards" markdown>

-   :material-api: __[Q1: Send Many Transaction API Documentation](q1-api-documentation.md)__

    ---

    Documentation update for `/sendmanytransaction` incorporating `cube3` threat screening, `screeningFlag` body parameters, `201 Created` success schemas, and atomic `400 Bad Request` rejection rules for high-risk addresses (> 80 risk score).

    [:octicons-arrow-right-24: View Q1 API Reference](q1-api-documentation.md)

-   :material-wallet-outline: __[Q2: Solana Staking Guide Audit & Rewrite](q2-solana-staking-guide.md)__

    ---

    8-point usability audit of Liminal's Solana Staking Guide, accompanied by a complete step-by-step rewritten developer and customer guide with state diagrams and troubleshooting tables.

    [:octicons-arrow-right-24: View Q2 Audit & Guide](q2-solana-staking-guide.md)

-   :material-swap-horizontal: __[Q3: RESTful API Redesign](q3-rest-api-redesign.md)__

    ---

    Architectural standardization of 3 non-REST legacy endpoints into resource-oriented RESTful URIs with idempotency, status code matrix, complete OpenAPI specification, and migration strategies.

    [:octicons-arrow-right-24: View Q3 REST Redesign](q3-rest-api-redesign.md)

-   :material-briefcase-check: __[Q4: Work Samples & Portfolio](q4-portfolio-samples.md)__

    ---

    Selected technical writing samples showcasing GitHub repositories, developer platforms, API references, architecture guides, and developer tooling.

    [:octicons-arrow-right-24: View Q4 Portfolio](q4-portfolio-samples.md)

</div>

---

## OpenAPI Specification Download & Integration

The complete machine-readable **OpenAPI 3.0.0 Specification** for this assignment submission is available directly in the repository root:

- **OpenAPI File**: [`openapi.json`](file:///c:/Users/rajwa/Downloads/Assign/Assign/openapi.json)
- **Raw Spec URL**: `https://raw.githubusercontent.com/Rajwar88/Assign/v1.0/openapi.json`
- **Import Ready**: Compatible with Postman, Swagger UI, Insomnia, and Redoc.

---

## Technical Documentation Standards & Principles

All documentation produced in this submission adheres to the following core tenets:

- **Accuracy & Completeness**: Every parameter, header, data type, and error response is explicitly defined without ambiguity.
- **Developer-First Ergonomics**: Code examples are provided in cURL, Python, Node.js, and Go with copy-paste readiness.
- **Visual Clarity**: Complex control flows and state transitions are illustrated using **Mermaid sequence & state diagrams**.
- **Edge Case Governance**: Clear guidance on failure modes, error handling, risk thresholds, and fallback behaviors.

---

## Quick Site Info & Production Build Guarantee

| Attribute | Specification |
| :--- | :--- |
| **Documentation Engine** | MkDocs 1.6.1 + MkDocs Material 9.7.6 |
| **Target Hosting** | GitHub Pages (`gh-pages` branch via GitHub Actions) |
| **Diagram Engine** | Mermaid.js (Native Markdown integration) |
| **Build Status** | `mkdocs build --strict` passed cleanly with 0 errors |
| **Live Portal URL** | [https://rajwar88.github.io/Assign/](https://rajwar88.github.io/Assign/) |
