# Q3.2: RESTful Redesign — Create Batch Transfer

<div class="endpoint-box">
  <span class="api-badge api-post">POST</span>
  <span class="endpoint-url">https://docs.lmnl.app/v2/transfers/batch</span>
</div>

!!! info "Architectural Standardization & Idempotency"
    **Legacy Route**: `POST /api/wallet/send-many-transaction` (`/reference/sendmanytransaction`)  
    **Redesigned Route**: `POST /v2/transfers/batch`  
    **Improvements**: Replaced camelCase RPC naming (`sendmanytransaction`) with pluralized collection path `/v2/transfers/batch`. Placed `asset` at the batch level to match gas computation models. Introduced mandatory `Idempotency-Key` HTTP headers for financial retry safety and integrated Cube3 risk screening.

---

## Endpoint Overview

Executes an atomic batch payout transaction sending digital assets from a source wallet to one or more destination addresses. Includes real-time **Cube3 Security Inspector** address threat screening. If any destination address exceeds the risk threshold (Risk Score > 80), the entire batch request fails atomically with **HTTP 400 Bad Request**.

---

## Migration Note: `sequenceId` to `Idempotency-Key`

In v1, clients passed `sequenceId` inside the JSON body. In v2:
- Clients are instructed to pass `Idempotency-Key: <UUIDv4>` as a standard HTTP header.
- For backward compatibility, if a client body includes `sequenceId`, v2 uses it as a fallback idempotency key.

---

## Request Headers

| Header | Type | Required | Description |
| :--- | :--- | :---: | :--- |
| `Authorization` | `string` | Yes | Bearer token formatted as `Bearer <API_KEY>`. |
| `Idempotency-Key` | `string` | Yes | Unique UUIDv4 string preventing duplicate transactions on network retries. |
| `Content-Type` | `string` | Yes | Must be `application/json`. |

---

## Request Body Parameters

```json
{
  "wallet_id": "vlt_14999",
  "asset": "ETH",
  "screening_flag": true,
  "recipients": [
    {
      "destination_address": "0x71C7656EC7ab88b098defB751B7401B5f6d8976F",
      "amount": "1.500000"
    },
    {
      "destination_address": "0xB9dF6D174d6f1f3A61484762E7131F46Ec85b0d1",
      "amount": "0.750000"
    }
  ]
}
```

---

## Response Example <span class="status-badge status-200">201 Created</span>

```json
{
  "id": "btx_89123049123",
  "object": "transfer_batch",
  "wallet_id": "vlt_14999",
  "status": "PROCESSING",
  "asset": "ETH",
  "screening": {
    "enabled": true,
    "result": "passed",
    "screening_id": 14823
  },
  "total_amount": "2.250000",
  "recipient_count": 2,
  "created_at": "2026-09-10T14:25:00Z"
}
```

---

## Rejection Error Example <span class="status-badge status-400">400 Bad Request — Threat Screening Blocked</span>

```json
{
  "type": "https://docs.lmnl.app/errors/address-threat-detected",
  "title": "Address Threat Blocked",
  "status": 400,
  "detail": "The transaction request was aborted because destination address 0xB9dF6D174d6f1f3A61484762E7131F46Ec85b0d1 exceeded the maximum allowed risk score threshold (Risk Score 89 > 80).",
  "instance": "/v2/transfers/batch",
  "code": "RISK_SCREENING_FAILED",
  "invalid_params": [
    {
      "name": "recipientsData.recipients[1].address",
      "reason": "Flagged by Cube3 Security Inspector with Risk Score 89 (Sanctioned / Malicious Activity)",
      "screening_id": 14825
    }
  ]
}
```
