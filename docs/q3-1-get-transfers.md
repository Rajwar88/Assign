# Q3.1: RESTful Redesign — Retrieve Transfers Collection

<div class="endpoint-box">
  <span class="api-badge api-get">GET</span>
  <span class="endpoint-url">https://docs.lmnl.app/v2/transfers</span>
</div>

!!! info "Architectural Standardization (Legacy Migration)"
    **Legacy Route**: `POST /api/wallet/get-transfer-list` (`/reference/gettransfers`)  
    **Redesigned Route**: `GET /v2/transfers`  
    **Improvements**: Switched from HTTP `POST` to `GET` for safe, cacheable data retrieval. Replaced verb-in-path (`get-transfer-list`) with pluralized collection noun (`/transfers`). Added standard cursor-based pagination.

---

## Endpoint Overview

Retrieves a paginated list of historical fund transfers associated with your organization's vaults. Supports filtering by wallet ID, transaction status, asset type, and time ranges.

---

## Query Parameters

| Parameter | Type | Required | Default | Description |
| :--- | :--- | :---: | :--- | :--- |
| `wallet_id` | `string` | No | — | Filter transfers by source wallet vault ID (e.g. `vlt_892341029384`). |
| `status` | `string` | No | — | Filter by transfer state: `PENDING`, `COMPLETED`, `FAILED`, `BLOCKED`. |
| `asset` | `string` | No | — | Filter by asset symbol (e.g. `ETH`, `SOL`, `USDT`). |
| `limit` | `integer` | No | `50` | Maximum items to return per page (Range: 1–250). |
| `starting_after` | `string` | No | — | Cursor object ID for fetching the next page of results. |

---

## Request Example

=== "cURL"
    ```bash
    curl -X GET "https://docs.lmnl.app/v2/transfers?wallet_id=vlt_892341029384&status=COMPLETED&limit=10" \
      -H "Authorization: Bearer liminal_sk_live_98a7sdf98a7sdf98" \
      -H "Accept: application/json"
    ```

=== "Node.js (Axios)"
    ```javascript
    const axios = require('axios');

    async function getTransfers() {
      const response = await axios.get('https://docs.lmnl.app/v2/transfers', {
        headers: { Authorization: 'Bearer liminal_sk_live_98a7sdf98a7sdf98' },
        params: {
          wallet_id: 'vlt_892341029384',
          status: 'COMPLETED',
          limit: 10
        }
      });
      console.log(response.data);
    }
    ```

=== "Python (requests)"
    ```python
    import requests

    headers = {"Authorization": "Bearer liminal_sk_live_98a7sdf98a7sdf98"}
    params = {
        "wallet_id": "vlt_892341029384",
        "status": "COMPLETED",
        "limit": 10
    }

    response = requests.get("https://docs.lmnl.app/v2/transfers", headers=headers, params=params)
    print(response.json())
    ```

---

## Response Example <span class="status-badge status-200">200 OK</span>

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
