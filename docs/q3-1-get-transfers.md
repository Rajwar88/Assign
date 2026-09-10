# Q3.1: RESTful Redesign — Retrieve Transfers (Single & Collection)

<div class="endpoint-box">
  <span class="api-badge api-get">GET</span>
  <span class="endpoint-url">https://docs.lmnl.app/v2/transfers/{tx_id}</span>
</div>
<div class="endpoint-box" style="margin-top: 8px;">
  <span class="api-badge api-get">GET</span>
  <span class="endpoint-url">https://docs.lmnl.app/v2/transfers</span>
</div>

!!! info "Architectural Standardization (Legacy Migration)"
    **Legacy Route**: `POST /api/wallet/get-transfer` (`/reference/gettransfers`)  
    **Redesigned Routes**:  
    - `GET /v2/transfers/{tx_id}` (Single-Resource Lookup)  
    - `GET /v2/transfers` (Collection Listing — **[v2 Feature Enhancement]**)  
    **Improvements**: Corrected legacy RPC verb-in-path (`/get-transfer` using HTTP `POST` for read-only lookups) into canonical REST resource patterns. Introduced `vlt_` / `trsf_` resource prefixes while preserving numeric ID aliases (`walletId: 14999`).

---

## 1. Retrieve Single Transfer (`GET /v2/transfers/{tx_id}`)

### Path Parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :---: | :--- |
| `tx_id` | `string` | Yes | Transaction hash (`txHash`), system sequence UUID (`sequenceId`), or prefixed transfer ID (`trsf_90812349120`). |

### Response Example <span class="status-badge status-200">200 OK</span>

```json
{
  "id": "trsf_90812349120",
  "object": "transfer",
  "wallet_id": "vlt_14999",
  "amount": "1.500000",
  "asset": "ETH",
  "chain": "ETH",
  "symbol": "ETH",
  "destination_address": "0x71C7656EC7ab88b098defB751B7401B5f6d8976F",
  "status": "COMPLETED",
  "tx_hash": "0xa38f12c45e98b71d23ef890214a1253a6b89e71239ab71230dff71238912384a",
  "sequence_id": "124056b0-fba1-42dc-9595-012537de5ba3",
  "created_at": "2026-09-10T14:20:00Z"
}
```

---

## 2. Retrieve Transfers Collection (`GET /v2/transfers`) — [v2 Enhancement]

### Query Parameters

| Parameter | Type | Required | Default | Description |
| :--- | :--- | :---: | :--- | :--- |
| `wallet_id` | `string` | No | — | Filter transfers by source wallet vault ID (e.g. `vlt_14999` or `14999`). |
| `sequence_id` | `string` | No | — | Filter transfers by client sequence UUID. |
| `status` | `string` | No | — | Filter by transfer state: `PENDING`, `COMPLETED`, `FAILED`, `BLOCKED`. |
| `asset` | `string` | No | — | Filter by asset symbol (e.g. `ETH`, `SOL`, `USDT`). |
| `limit` | `integer` | No | `50` | Maximum items to return per page (Range: 1–250). |
| `starting_after` | `string` | No | — | Cursor object ID for fetching the next page of results. |

---

## Request Examples

=== "cURL (Single Transfer)"
    ```bash
    curl -X GET "https://docs.lmnl.app/v2/transfers/0xa38f12c45e98b71d23ef890214a1253a6b89e71239ab71230dff71238912384a" \
      -H "Authorization: Bearer liminal_sk_live_98a7sdf98a7sdf98" \
      -H "Accept: application/json"
    ```

=== "cURL (Transfers Collection)"
    ```bash
    curl -X GET "https://docs.lmnl.app/v2/transfers?wallet_id=vlt_14999&status=COMPLETED&limit=10" \
      -H "Authorization: Bearer liminal_sk_live_98a7sdf98a7sdf98" \
      -H "Accept: application/json"
    ```

=== "Node.js (Axios)"
    ```javascript
    const axios = require('axios');

    async function getTransfer(txId) {
      const response = await axios.get(`https://docs.lmnl.app/v2/transfers/${txId}`, {
        headers: { Authorization: 'Bearer liminal_sk_live_98a7sdf98a7sdf98' }
      });
      console.log(response.data);
    }
    ```

=== "Python (requests)"
    ```python
    import requests

    headers = {"Authorization": "Bearer liminal_sk_live_98a7sdf98a7sdf98"}
    response = requests.get("https://docs.lmnl.app/v2/transfers/trsf_90812349120", headers=headers)
    print(response.json())
    ```

---

## Collection Response Example <span class="status-badge status-200">200 OK</span>

```json
{
  "object": "list",
  "data": [
    {
      "id": "trsf_90812349120",
      "object": "transfer",
      "wallet_id": "vlt_14999",
      "amount": "1.500000",
      "asset": "ETH",
      "chain": "ETH",
      "symbol": "ETH",
      "destination_address": "0x71C7656EC7ab88b098defB751B7401B5f6d8976F",
      "status": "COMPLETED",
      "tx_hash": "0xa38f12c45e98b71d23ef890214a1253a6b89e71239ab71230dff71238912384a",
      "sequence_id": "124056b0-fba1-42dc-9595-012537de5ba3",
      "created_at": "2026-09-10T14:20:00Z"
    }
  ],
  "has_more": false,
  "next_cursor": null
}
```
