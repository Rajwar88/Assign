# Q3.2: RESTful Redesign — Create Batch Transfer

<div class="endpoint-box">
  <span class="api-badge api-post">POST</span>
  <span class="endpoint-url">https://docs.lmnl.app/v2/transfers/batch</span>
</div>

!!! info "Architectural Standardization & Idempotency"
    **Legacy Route**: `POST /api/wallet/send-many-transaction` (`/reference/sendmanytransaction`)  
    **Redesigned Route**: `POST /v2/transfers/batch`  
    **Improvements**: Replaced camelCase RPC naming (`sendmanytransaction`) with pluralized collection path `/v2/transfers/batch`. Introduced mandatory `Idempotency-Key` headers for financial retry safety and integrated Cube3 risk screening.

---

## Endpoint Overview

Executes an atomic batch payout transaction sending digital assets to one or more destination addresses. Includes real-time **Cube3 Security Inspector** address threat screening. If any destination address exceeds the risk threshold (Risk Score > 80), the entire batch request fails atomically with HTTP `422 Unprocessable Entity`.

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

---

## Response Example <span class="status-badge status-200">201 Created</span>

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

## Rejection Error Example <span class="status-badge status-400">422 Unprocessable Entity</span>

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
