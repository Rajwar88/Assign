# Q3.3: RESTful Redesign — Retrieve Wallet Balances

<div class="endpoint-box">
  <span class="api-badge api-get">GET</span>
  <span class="endpoint-url">https://docs.lmnl.app/v2/wallets/{wallet_id}/balances</span>
</div>

!!! info "Sub-Resource Hierarchy Rationale"
    **Legacy Route**: `POST /api/wallet/balance` (`/reference/getwalletbalance`)  
    **Redesigned Route**: `GET /v2/wallets/{wallet_id}/balances`  
    **Improvements**: Balances are dependent child attributes of a specific Wallet entity. Moving to a hierarchical sub-resource URI (`/v2/wallets/{wallet_id}/balances`) enforces proper REST resource ownership, HTTP caching, and role-based access controls. Preserves legacy fields (`chain`, `coin`, `symbol`) while adding v2 institutional balance breakdowns.

---

## Endpoint Overview

Fetches coin and token balances for all addresses within a specific Liminal vault wallet resource. Returns liquid, available, staked, and pending withdrawal balances.

---

## Path Parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :---: | :--- |
| `wallet_id` | `string` | Yes | Unique identifier of the vault wallet (e.g. `vlt_14999` or `14999`). |

---

## Request Example

=== "cURL"
    ```bash
    curl -X GET "https://docs.lmnl.app/v2/wallets/vlt_14999/balances" \
      -H "Authorization: Bearer liminal_sk_live_98a7sdf98a7sdf98" \
      -H "Accept: application/json"
    ```

=== "Node.js (Axios)"
    ```javascript
    const axios = require('axios');

    async function getWalletBalances(walletId) {
      const response = await axios.get(`https://docs.lmnl.app/v2/wallets/${walletId}/balances`, {
        headers: { Authorization: 'Bearer liminal_sk_live_98a7sdf98a7sdf98' }
      });
      console.log(response.data);
    }
    ```

=== "Python (requests)"
    ```python
    import requests

    wallet_id = "vlt_14999"
    headers = {"Authorization": "Bearer liminal_sk_live_98a7sdf98a7sdf98"}

    response = requests.get(f"https://docs.lmnl.app/v2/wallets/{wallet_id}/balances", headers=headers)
    print(response.json())
    ```

---

## Response Example <span class="status-badge status-200">200 OK</span>

```json
{
  "object": "wallet_balance",
  "wallet_id": "vlt_14999",
  "vault_name": "Primary Hot Exchange Vault",
  "balances": [
    {
      "asset": "ETH",
      "coin": "eth",
      "chain": "ETH",
      "symbol": "ETH",
      "total_balance": "150.750000",
      "available_balance": "148.500000",
      "staked_balance": "0.000000",
      "pending_withdrawals": "2.250000"
    },
    {
      "asset": "SOL",
      "coin": "sol",
      "chain": "SOL",
      "symbol": "SOL",
      "total_balance": "10500.000000",
      "available_balance": "500.000000",
      "staked_balance": "10000.000000",
      "pending_withdrawals": "0.000000"
    }
  ],
  "updated_at": "2026-09-10T14:26:10Z"
}
```

*Note: `staked_balance` and `pending_withdrawals` are **[v2 Feature Enhancements]** providing institutional custody visibility for delegated staking and locked vault operations.*
