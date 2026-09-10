# Liminal Technical Writer Evaluation Submission

This portal contains the complete evaluation submission for Liminal's Technical Writer assignment. All documentation is written in GitHub Flavored Markdown, rendered via **MkDocs Material**, and hosted on **GitHub Pages**.

---

## Deliverables & Evaluation Matrix

| Task | Topic | Key Deliverables | Highlights & Specification Rules |
| :---: | :--- | :--- | :--- |
| **Q1** | **API Endpoint Update** | [Q1 API Reference](q1-api-documentation.md) | Documented `screeningFlag` body parameter for Cube3 risk screening. Standardized threat rejections (Risk Score > 80) to **HTTP 400 Bad Request** returning first blocked address error array. Added XRP/Stellar `enableToken` trust line guidelines. |
| **Q2** | **Solana Staking Audit & Rewrite** | [Q2 Audit & Guide](q2-solana-staking-guide.md) | 8-point audit matrix identifying gaps in the original guide. Rewrote step-by-step staking operations with exact `0.00228288 SOL` rent-exempt reserve, 5-stage Mermaid state machine, Figment commission disclosures, and rent reclamation procedures. |
| **Q3** | **RESTful API Redesign** | [Q3 REST Redesign](q3-rest-api-redesign.md)<br>• [GET /v2/transfers/{tx_id}](q3-1-get-transfers.md)<br>• [POST /v2/transfers/batch](q3-2-send-transactions.md)<br>• [GET /v2/wallets/{id}/balances](q3-3-get-balances.md) | Redesigned 3 legacy endpoints into RESTful URIs. Documented single transfer lookup (`GET /v2/transfers/{tx_id}`), batch payouts with `Idempotency-Key` headers, hierarchical wallet balances, full OpenAPI 3.0.0 schema, and deprecation roadmap. |
| **Q4** | **Work Samples & Portfolio** | [Q4 Portfolio Showcase](q4-portfolio-samples.md) | Featured public GitHub repositories ([`docs-cycle-time-metrics`](https://github.com/Rajwar88/docs-cycle-time-metrics), [`docs-prioritization-engine`](https://github.com/Rajwar88/docs-prioritization-engine), [`Technical-Writing-Portfolio`](https://github.com/Rajwar88/Technical-Writing-Portfolio)), enterprise Freight Audit & Pay guide, EDI 210/310 specs, and CarrierGo manual. |

---

## Assignment Navigation

<div class="grid cards" markdown>

-   :material-api: __[Q1: Send Many Transaction API Documentation](q1-api-documentation.md)__

    ---

    API specification for `/sendmanytransaction` incorporating `cube3` threat screening, `screeningFlag` parameter, `201 Created` success payloads, and atomic `400 Bad Request` rejections for high-risk addresses.

    [:octicons-arrow-right-24: View Q1 API Reference](q1-api-documentation.md)

-   :material-wallet-outline: __[Q2: Solana Staking Guide Audit & Rewrite](q2-solana-staking-guide.md)__

    ---

    8-point usability audit of Liminal's Solana Staking Guide, accompanied by a step-by-step rewritten developer and customer guide with state diagrams and troubleshooting tables.

    [:octicons-arrow-right-24: View Q2 Audit & Guide](q2-solana-staking-guide.md)

-   :material-swap-horizontal: __[Q3: RESTful API Redesign](q3-rest-api-redesign.md)__

    ---

    REST architectural redesign of 3 legacy non-REST endpoints into resource-oriented URIs with idempotency headers, status code matrix, OpenAPI specifications, and migration strategies.

    [:octicons-arrow-right-24: View Q3 REST Redesign](q3-rest-api-redesign.md)

-   :material-briefcase-check: __[Q4: Work Samples & Portfolio](q4-portfolio-samples.md)__

    ---

    Technical writing samples showcasing public GitHub repositories, developer platform documentation, API references, data mapping specs, and developer tooling.

    [:octicons-arrow-right-24: View Q4 Portfolio](q4-portfolio-samples.md)

</div>

---

## OpenAPI 3.0 Specification Access

The machine-readable OpenAPI 3.0.0 specification file is located in the repository root:

- **Local Spec File**: [`openapi.json`](file:///c:/Users/rajwa/Downloads/Assign/Assign/openapi.json)
- **Raw GitHub URL**: `https://raw.githubusercontent.com/Rajwar88/Assign/v1.0/openapi.json`
- **Compatibility**: Tested for import with Postman, Swagger Editor, and Redoc.

---

## Technical Writing Tenets

- **Precision & Schema Accuracy**: Every parameter, header, data type, and error payload reflects exact API contracts.
- **Code Readiness**: Code examples in cURL, Python, Node.js, and Go are validated for copy-paste execution.
- **Visual Control Flows**: Sequence and state diagrams explain complex blockchain lifecycles and epoch warm-up transitions.
- **Edge Case Coverage**: Guidance on failure modes, error handling, risk score thresholds, and fallback behaviors.

---

## Environment & Build Status

| Property | Value |
| :--- | :--- |
| **Documentation Engine** | MkDocs 1.6.1 + MkDocs Material 9.7.6 |
| **Hosting Platform** | GitHub Pages (`gh-pages` branch) |
| **Build Status** | `mkdocs build --strict` (0 errors, 0 warnings) |
| **Live Portal URL** | [https://rajwar88.github.io/Assign/](https://rajwar88.github.io/Assign/) |
