# Q2: Solana Staking Guide — Comprehensive Audit & Rewritten Guide

This document provides a **two-part technical evaluation** of Liminal's Solana Staking Guide:

1. **Part A: In-Depth Audit & Points of Improvement** — Structural, technical, and UX critique of the existing guide (`/docs/stake-solana-assets-in-liminal-vaults`).
2. **Part B: Rewritten Solana Staking Master Guide** — A redesigned, step-by-step, task-oriented user guide for Liminal Dashboard users.

---

# Part A: In-Depth Audit & Points of Improvement

After reviewing Liminal’s existing Solana Staking Guide via live MCP documentation audit, **8 critical gaps** were identified from the perspective of an institutional exchange operator using Liminal Vaults. 

Below is the structured audit matrix detailing each gap, its operational impact, and recommended remedies:

### Audit Matrix

| # | Improvement Area | Identified Gap in Original Guide | Recommended Technical & Documentation Remedy |
| :-: | :--- | :--- | :--- |
| **1** | **Prerequisites & Rent-Exempt Reserve** | Does not specify that Solana requires a **Rent-Exempt Minimum Reserve** (~0.00228288 SOL) per stake account, plus transaction fee buffer (~0.005 SOL). Users attempting to stake 100% of their balance experience silent transaction failures. | Add a prominent **Prerequisites Checklist** explicitly detailing mandatory minimum un-staked SOL reserves required for stake account creation and gas fees. |
| **2** | **Solana Warm-Up & Cool-Down Epoch Lifecycle** | Omits explanation of Solana's **Epoch Warm-Up** (~2 days) and **Unbonding Period** (~2-day cool-down). Users expect immediate reward generation after clicking "Stake". | Include a dedicated **Staking State Machine Diagram** explaining the `Activating` state, 2-day reward credit frequency, and epoch transition timing. |
| **3** | **Multi-Sig & Cold Vault Quorum Workflow** | Cold storage and MPC vaults require multi-signature approval. The original guide treats staking as a single-click action, confusing institutional users whose vaults require threshold signoffs from secondary keyholders. | Detail the **Multi-Sig Quorum Approval Workflow**, explaining how initiators and co-signers approve requests via the Vaults web platform and Mobile Co-Signer App. |
| **4** | **Validator Partner & Commission Transparency** | Lacks clarity on the integrated validator partner (**Figment**), omitting details on validator commission rates, Figment Terms of Service acceptance, and reward yields (~6.42% APY). | Provide explicit **Figment Validator Integration** guidance, including mandatory TOS signoff and yield expectations. |
| **5** | **De-delegation & Unbonding Procedure** | Focuses solely on *initiating* a stake, omitting instructions for **Unstaking (De-delegating)**, the 2-day cool-down epoch period, and withdrawing principal + rewards back to the main wallet. | Add a complete **Section for De-delegation & Withdrawals**, detailing cool-down timing and rent-exempt SOL reclamation. |
| **6** | **Visual Step-by-Step UI Guidance** | Uses placeholder images or lacks clear UI screenshots with callout indicators for Liminal Vaults navigation. | Incorporate authentic **Liminal UI Screenshots** for wallet enablement, account creation, mobile co-signing, and delegation. |
| **7** | **Rewards Calculator Tool** | Does not explain how exchange operations teams can estimate projected SOL yields prior to locking up capital. | Add a **Liminal Rewards Calculator** guide detailing inputs (Amount, Duration, Asset Price, APR %) and projected yield outputs. |
| **8** | **Troubleshooting & Error Recovery** | Contains no troubleshooting table for common UI errors (e.g. `Single staking process limit`, `Insufficient SOL for Stake Account creation`, `Pending Co-Signer Timeout`). | Add a comprehensive **Troubleshooting & FAQ** reference table at the end of the guide. |

---

# Part B: Rewritten Solana Staking Master Guide

```
DOCUMENT METADATA
Title: Staking Solana (SOL) Assets via Liminal Institutional Vaults
Audience: Exchange Operations, Custody Managers, Treasury Leads
Product: Liminal Dashboard / Vaults Staking Hub
Validator Partner: Figment
Last Updated: September 2026
```

## Overview

Liminal’s Staking Hub allows institutional exchanges to stake SOL directly from warm MPC and cold vaults. Staking SOL enables you to earn network inflation rewards (**~6.42% APY** powered by **Figment**) while retaining full multi-signature custody of your underlying private keys.

Key staking parameters:
- **Reward Rate**: ~6.42% APR (subject to network inflation dynamics).
- **Reward Credit Frequency**: Every ~2 days (at epoch boundaries).
- **Unbonding Period**: ~2 days (cool-down epoch before withdrawal).
- **Compounding**: Rewards automatically compound within your dedicated Stake Account.

!!! note "Custody Safety & Multi-Sig Rules"
    - **No Custody Loss**: Liminal never acquires or retains custody of your staked assets at any time.
    - **Single Process Rule**: You can initiate only **one staking process** for a wallet at a time.
    - **Immutable Stake Amount**: Once staked, the delegation amount cannot be modified; additional stakes require a new staking account.

---

## Solana Rewards Calculator

Before delegating assets, you can calculate projected yields using the built-in **Liminal Rewards Calculator**:

1. Log into [Liminal Vaults](https://vaults.lmnl.app/).
2. Navigate to **Staking** > **Rewards Calculator**.
3. Fill in the parameters:
   - **Asset**: Select `SOL (Solana)`.
   - **Amount of Tokens**: Enter your intended staking volume (e.g., `1,000 SOL`).
   - **Staking Duration**: Set target lockup period (e.g., `365 Days`).
   - **Price of Asset**: Enter current market price in USD.
   - **Rewards Rate (APR)**: Set network APR (~6.42%).
4. The panel displays projected total rewards in both USD equivalent and native SOL.

![Liminal Rewards Calculator](https://files.readme.io/08d598befd6468309f501b8d1e8fd58e4d1eb5dcbbb078541ae9ca447ff94628-image.png)

---

## Solana Stake Account Lifecycle

Solana does not stake tokens directly from your primary vault address. Instead, Liminal creates a dedicated on-chain **Stake Account** funded by your wallet and delegates it to the Figment validator node.

```mermaid
stateDiagram-v2
    [*] --> Inactive: Fund & Create Stake Account
    Inactive --> Activating: Delegate to Figment (Submit Tx)
    note right of Activating
      Warm-Up Period: ~2 Days (1 Epoch)
      No yield earned during activation.
    end note
    Activating --> Active: Epoch Transition Complete
    note right of Active
      Active Staking State
      Rewards credited & auto-compounded every ~2 days.
    end note
    Active --> Deactivating: Initiate De-delegation (Unstake)
    note right of Deactivating
      Cool-Down Period: ~2 Days (1 Epoch)
      Tokens locked until epoch completes.
    end note
    Deactivating --> Inactive: Cool-Down Complete
    Inactive --> [*]: Withdraw Principal + Yield to Main Wallet
```

---

## Prerequisites Checklist

Before initiating a Solana staking transaction, verify your vault setup:

- [x] **Staking Enabled for Org**: Confirm your organisation has enabled Solana staking with Liminal (<sales@lmnl.app>).
- [x] **Rent-Exempt Reserve**: Maintain at least **~0.0023 SOL** un-staked in the main wallet for on-chain stake account rent exemption.
- [x] **Gas Fee Reserve**: Preserve an additional **~0.005–0.01 SOL** in liquid balance for future de-delegation and withdrawal gas fees.
- [x] **Signing Quorum Ready**: For MPC and multi-sig wallets, ensure required threshold initiators and signers are available to approve transaction requests via web or mobile app.

---

## Step-by-Step Staking Guide

### Step 1: Enable Staking for your Solana Wallet

Once staking is activated for your organisation, enable the SOL asset chain in Liminal Vaults:

1. Log into [Liminal Vaults](https://vaults.lmnl.app/).
2. Go to **Staking**.
3. Locate **SOL** asset and select **Start Staking**.
4. In the **Wallet** field, select your target Solana wallet.
5. Check mark the **Figment Terms of Service** agreement.
6. Select **Enable Staking**.
7. Enter your **Two-Factor Authentication (2FA)** code and select **Continue**.
8. Refresh the page to confirm the wallet is enabled for SOL staking.

![Enable Staking for Solana Wallet](https://files.readme.io/efe9410f0a18eed444327b426b662aa4c4ba21852cacc40a3f163ad5db79cf81-image.png)

---

### Step 2: Create an On-Chain Staking Account

A staking account is a dedicated sub-account created within your Solana wallet to manage staked funds, accumulate rewards, and process withdrawals.

1. In the **Staking** dashboard, expand the **SOL** asset row.
2. Select **Create Account**.
3. Complete the form:
   - **Amount**: Enter the SOL amount to transfer from your main wallet to the staking account.
   - **Note**: Add a transaction description for auditing purposes.
4. Select **Next** > **Sign**.

![Create Staking Account Modal](https://files.readme.io/ca4961c033b673343b113f72d3e0af7810e9c605e20454557c43b53fc2c8678d-image.png)

5. **Multi-Sig Quorum Signoff**:
   - **Initiators**: Log into Vaults web, find the pending transaction under **Pending Actions**, select **Approve**, and enter 2FA.
   - **Signers**: Log into the **Vaults Mobile App**, locate the notification under **Pending Approvals**, and tap **Approve**.

<div align="center">
  <img width="300" src="https://files.readme.io/c9a133d02610f84a12e0474e72e659ed1d95b4afeeee7e32275a3fdc6c7983d9-Screenshot_20241028-155948.png" alt="Vaults Mobile App Approval" />
</div>

Once quorum signoff is complete, the SOL amount is transferred to your newly created staking account.

---

### Step 3: Stake Solana Assets (Delegate to Figment)

Once the staking account is created and funded, delegate the assets to Figment:

1. Go to **Staking** and expand the **SOL** asset.
2. Locate your newly created staking account and select **Stake**.
3. In the **Note** field, enter a description and click **Next**.
4. Click **Sign** and enter your 2FA code.
5. Complete the threshold multi-sig approval via Vaults Web / Mobile App as performed in Step 2.

![Stake Solana Assets Delegation](https://files.readme.io/2f5f3b547bab9e009709c4e5adf90e85275b736393fd475a8063ac1ef3cf3b36-image.png)

6. Upon successful broadcast, the staking balance displays as **Activating**. 
7. After ~2 days (epoch shift), the status updates to **Active**, and inflation rewards begin compounding automatically.

---

## Unstaking (De-delegating) & Principal Withdrawal

When you wish to return staked SOL back to your main liquid wallet balance:

1. Navigate to **Staking** > expand **SOL**.
2. Locate the target active staking account and click **Unstake (De-delegate)**.
3. Sign and complete multi-sig approval. Status shifts to **Deactivating**.
4. **Cool-Down Period**: Wait approximately **2 days** (until the current Solana epoch ends).
5. Once the status updates to **Inactive**, select **Withdraw** to transfer the principal + accrued rewards back into your main wallet account.

---

## Troubleshooting & Common Support Inquiries

| Issue / Error Message | Root Cause | Resolution Step |
| :--- | :--- | :--- |
| `Insufficient SOL for Rent Exemption` | Attempted to stake entire wallet balance without leaving reserve for stake account creation (~0.0023 SOL). | Reduce stake amount to leave at least 0.01 SOL liquid balance in the main wallet. |
| `Single Staking Process Limit` | Attempted to initiate a second staking request while a previous transaction is still pending approval. | Complete or cancel the pending staking transaction before starting a new request. |
| `Staking status stuck in Activating` | Solana epoch boundary (~2 days) has not yet elapsed. | Rewards and active delegation begin at epoch shift. Monitor epoch status under Staking dashboard. |
| `Pending Quorum Timeout` | Multi-sig initiators or mobile signers did not approve within the approval window. | Re-initiate transaction and ensure co-signers approve via the Vaults Mobile App. |
| `Rewards not in Main Wallet` | Rewards accumulate directly inside the Stake Account, not liquid wallet balance. | Staked balance grows inside the Stake Account. Funds return to main wallet upon de-delegation & withdrawal. |
