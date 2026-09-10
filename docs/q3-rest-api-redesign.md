# Q3: RESTful API Standardization & Redesign

This document presents a comprehensive **RESTful API Architectural Redesign** for three legacy Liminal API endpoints identified in the Technical Writer Evaluation Assignment:

1. `https://docs.lmnl.app/reference/gettransfers` (`POST /api/wallet/get-transfer-list`)
2. `https://docs.lmnl.app/reference/sendmanytransaction` (`POST /api/wallet/send-many-transaction`)
3. `https://docs.lmnl.app/reference/getwalletbalance` (`POST /api/wallet/balance`)

---

## Architectural Audit & Non-RESTful Anti-Patterns

The three legacy endpoints violate standard REST architectural principles in several key areas:

### Identified Non-REST Anti-Patterns

```
1. VERB-IN-PATH & RPC-OVER-HTTP ANTI-PATTERN
   Legacy Docs:   GET/POST  /reference/gettransfers
   Legacy API:    POST      /api/wallet/get-transfer-list
   Legacy Docs:   POST      /reference/sendmanytransaction
   Legacy API:    POST      /api/wallet/send-many-transaction
   Legacy Docs:   GET/POST  /reference/getwalletbalance
   Legacy API:    POST      /api/wallet/balance
   Critique: In REST, the HTTP Verb (GET, POST, PUT, DELETE) defines the action. 
             Including verbs ("get", "send") in the URI and using HTTP POST for read-only 
             data queries creates RPC-style endpoints that prevent standard HTTP caching.

2. NON-PLURAL RESOURCE NOUNS
   Legacy:  sendmanytransaction, getwalletbalance
   Critique: REST resources represent collections of entities and should use lowercase, 
             pluralized nouns (e.g. /transfers, /transactions, /wallets).

3. LACK OF SUB-RESOURCE HIERARCHY
   Legacy:  POST /api/wallet/balance (passing wallet ID in body or query)
   Critique: Balances are dependent child attributes of a specific Wallet entity. 
             The URI structure should reflect the resource hierarchy (/wallets/{wallet_id}/balances).

4. INCONSISTENT ERROR SCHEMAS & MISSING IDEMPOTENCY
   Legacy:  Ad-hoc JSON error structures without standardized HTTP status codes or retry safety headers.
   Critique: Modern enterprise APIs mandate RFC 7807 problem details, HTTP 422 status codes for threat screening, and Idempotency-Key headers for financial operations.
```

---

## The Redesigned REST API Suite

The matrix below maps each legacy endpoint to its standardized RESTful equivalent:

| Legacy Endpoint (Docs & API Route) | Redesigned RESTful Resource URI | HTTP Method | Resource Description |
| :--- | :--- | :---: | :--- |
| `POST /api/wallet/get-transfer-list` <br>`(/reference/gettransfers)` | `GET /v2/transfers` | <span class="api-badge api-get">GET</span> | Retrieve a paginated collection of historical fund transfers. |
| `POST /api/wallet/send-many-transaction` <br>`(/reference/sendmanytransaction)` | `POST /v2/transfers/batch` | <span class="api-badge api-post">POST</span> | Create a new atomic multi-destination batch transfer resource. |
| `POST /api/wallet/balance` <br>`(/reference/getwalletbalance)` | `GET /v2/wallets/{wallet_id}/balances` | <span class="api-badge api-get">GET</span> | Fetch token balances for a specific vault wallet resource. |

---

## Endpoint Specifications & Redesign Details

### 1. Retrieve Transfers Collection

#### Legacy vs. Redesigned Comparison

- **Legacy**: `POST /api/wallet/get-transfer-list` *(Using POST for data fetch)*
- **Redesigned**: <span class="api-badge api-get">GET</span> `https://docs.lmnl.app/v2/transfers`

#### Query Parameters

Use standard REST query parameters for filtering, sorting, and cursor pagination:

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `wallet_id` | `string` | — | Filter transfers by source wallet vault ID. |
| `status` | `string` | — | Filter by state (`PENDING`, `COMPLETED`, `FAILED`, `BLOCKED`). |
| `asset` | `string` | — | Filter by token asset symbol (e.g. `ETH`, `SOL`, `USDT`). |
| `limit` | `integer` | `50` | Maximum items to return per page (Max `250`). |
| `starting_after` | `string` | — | Cursor object ID for fetching the next page of results. |

#### Redesigned Response Example <span class="status-badge status-200">200 OK</span>

```json
{
  "object": "list",
  "data": [
    {
      "id": "trsf_90812349120",
      "object": "transfer",
      "wallet_id": "vlt_892341029384",
      "amount": "1.500000",
      "asset": "ETH",
      "destination_address": "0x71C7656EC7ab88b098defB751B7401B5f6d8976F",
      "status": "COMPLETED",
      "tx_hash": "0xa38f12c45e98b71d23ef890214a1253a6b89e71239ab71230dff71238912384a",
      "created_at": "2026-09-10T14:20:00Z"
    }
  ],
  "has_more": true,
  "next_cursor": "trsf_90812349120"
}
```

---

### 2. Create Batch Transfer (Send Many)

#### Legacy vs. Redesigned Comparison

- **Legacy**: `POST /api/wallet/send-many-transaction`
- **Redesigned**: <span class="api-badge api-post">POST</span> `https://docs.lmnl.app/v2/transfers/batch`

#### Key Architectural Enhancements

1. **Idempotency Guarantee**: Accepts an `Idempotency-Key: <UUIDv4>` HTTP header to prevent duplicate payouts caused by network timeouts or automatic retries.
2. **Pluralized Sub-Resource**: Grouping under `/v2/transfers/batch` clearly designates a batch operation on the `transfers` collection.

#### Request Headers

```http
POST /v2/transfers/batch HTTP/1.1
Host: docs.lmnl.app
Authorization: Bearer liminal_sk_live_98a7sdf98a7sdf98
Idempotency-Key: 7b9e4a12-8c3d-4f5e-9a1b-2c3d4e5f6a7b
Content-Type: application/json
```

#### Request Payload

```json
{
  "wallet_id": "vlt_892341029384",
  "screening_flag": true,
  "recipients": [
    {
      "destination_address": "0x71C7656EC7ab88b098defB751B7401B5f6d8976F",
      "amount": "1.500000",
      "asset": "ETH"
    },
    {
      "destination_address": "0xB9dF6D174d6f1f3A61484762E7131F46Ec85b0d1",
      "amount": "0.750000",
      "asset": "ETH"
    }
  ]
}
```

#### Redesigned Response Example <span class="status-badge status-200">201 Created</span>

```json
{
  "id": "btx_89123049123",
  "object": "transfer_batch",
  "wallet_id": "vlt_892341029384",
  "status": "PROCESSING",
  "screening": {
    "enabled": true,
    "result": "passed",
    "screening_id": 14823
  },
  "total_amount": "2.250000",
  "asset": "ETH",
  "recipient_count": 2,
  "created_at": "2026-09-10T14:25:00Z"
}
```

---

### 3. Retrieve Wallet Balances

#### Legacy vs. Redesigned Comparison

- **Legacy**: `POST /api/wallet/balance` *(Passing wallet ID in query or body)*
- **Redesigned**: <span class="api-badge api-get">GET</span> `https://docs.lmnl.app/v2/wallets/{wallet_id}/balances`

#### Sub-Resource Hierarchy Rationale

In REST, a wallet balance cannot exist independently of a wallet entity. Routing via `/v2/wallets/{wallet_id}/balances` leverages standard REST sub-resource paths, making resource scoping, permissions, and caching straightforward.

#### Redesigned Response Example <span class="status-badge status-200">200 OK</span>

```json
{
  "object": "wallet_balance",
  "wallet_id": "vlt_892341029384",
  "vault_name": "Primary Hot Exchange Vault",
  "balances": [
    {
      "asset": "ETH",
      "total_balance": "150.750000",
      "available_balance": "148.500000",
      "staked_balance": "0.000000",
      "pending_withdrawals": "2.250000"
    },
    {
      "asset": "SOL",
      "total_balance": "10500.000000",
      "available_balance": "500.000000",
      "staked_balance": "10000.000000",
      "pending_withdrawals": "0.000000"
    }
  ],
  "updated_at": "2026-09-10T14:26:10Z"
}
```

---

## Standardized Error Payload (RFC 7807 Problem Details)

To eliminate inconsistent error structures, all `/v2/` REST endpoints adopt the **RFC 7807 Problem Details Specification**, including **HTTP 422 Unprocessable Entity** for Cube3 security screening blocks:

```json
{
  "type": "https://docs.lmnl.app/errors/address-threat-detected",
  "title": "Address Threat Blocked",
  "status": 422,
  "detail": "The transaction request was aborted because destination address 0x35febC101123... exceeded the maximum allowed risk score threshold (Risk Score > 80).",
  "instance": "/v2/transfers/batch",
  "code": "RISK_SCREENING_FAILED",
  "invalid_params": [
    {
      "name": "recipients[0].destination_address",
      "reason": "Flagged by Cube3 Security Inspector with Risk Score 99 (Sanctioned / Malicious Activity)",
      "screening_id": 1299
    }
  ]
}
```

---

## OpenAPI 3.0.0 Specification (Redesigned /v2/ REST Suite)

Below is the OpenAPI 3.0.0 definition representing the standardized `/v2/` REST API suite:

```json
{
  "openapi": "3.0.0",
  "info": {
    "title": "Liminal REST API v2",
    "version": "2.0.0",
    "description": "Standardized RESTful API suite for Liminal custody, transfers, and wallet management."
  },
  "servers": [
    {
      "url": "https://docs.lmnl.app/v2"
    }
  ],
  "paths": {
    "/transfers": {
      "get": {
        "summary": "Retrieve Transfers Collection",
        "parameters": [
          { "name": "wallet_id", "in": "query", "schema": { "type": "string" } },
          { "name": "status", "in": "query", "schema": { "type": "string" } },
          { "name": "limit", "in": "query", "schema": { "type": "integer", "default": 50 } },
          { "name": "starting_after", "in": "query", "schema": { "type": "string" } }
        ],
        "responses": {
          "200": { "description": "Successful retrieval" }
        }
      }
    },
    "/transfers/batch": {
      "post": {
        "summary": "Create Batch Transfer",
        "parameters": [
          {
            "name": "Idempotency-Key",
            "in": "header",
            "required": true,
            "schema": { "type": "string", "format": "uuid" }
          }
        ],
        "requestBody": {
          "required": true,
          "content": { "application/json": {} }
        },
        "responses": {
          "201": { "description": "Batch transfer created" },
          "422": { "description": "Threat Screening Blocked (RISK_SCREENING_FAILED)" }
        }
      }
    },
    "/wallets/{wallet_id}/balances": {
      "get": {
        "summary": "Retrieve Wallet Balances",
        "parameters": [
          { "name": "wallet_id", "in": "path", "required": true, "schema": { "type": "string" } }
        ],
        "responses": {
          "200": { "description": "Wallet balances fetched" }
        }
      }
    }
  }
}
```

---

## API Migration & Backward Compatibility Strategy

To ensure zero disruption for existing exchange client integrations, Liminal implements a phased **Deprecation & Migration Roadmap**:

```mermaid
gantt
    title Liminal API v1 to v2 REST Migration Roadmap
    dateFormat  YYYY-MM-DD
    section Phase 1: Launch
    Deploy /v2 REST API Endpoints       :done, p1, 2026-10-01, 30d
    Update API Reference & Developer Portal :done, p2, 2026-10-15, 20d
    section Phase 2: Deprecation Notice
    Add HTTP Sunset Header to /v1 Endpoints :active, p3, 2026-11-01, 90d
    Deliver Client Migration SDKs & Guides :p4, 2026-11-15, 60d
    section Phase 3: Sunset
    Decommission Legacy /v1 Endpoints :p5, 2027-02-01, 1d
```

### HTTP Deprecation Headers

Legacy `/v1/` responses will include standard deprecation headers informing clients of the migration window:

```http
HTTP/1.1 200 OK
Deprecation: @1790812800
Sunset: Mon, 01 Feb 2027 00:00:00 GMT
Link: <https://docs.lmnl.app/v2/migration-guide>; rel="successor-version"
```
