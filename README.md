# Smart Contract & Protocol Security Portfolio (`@myhubstat`)

Independent security research, public vulnerability disclosures, Foundry PoC harnesses, and upstream remediation pull requests by ** (`@myhubstat`)**.

- **Focus Areas:** EVM / Solidity Smart Contracts (ERC-20, ERC-223, ERC-4626, EIP-7702, AMM/DEX Concentrating Liquidity, Lending & Vault Accounting Invariants), Rust L1/L2 Confidential & Stealth Wallet Security, and Static Analysis / Compiler Soundness.
- **Methodology:** Manual line-by-line architectural review combined with stateful Foundry fork PoCs, boundary math verification, and custom AST static analysis.

---

## 1. EVM / Solidity Smart Contract Security Disclosures & Remediations

| Target Protocol / Repository | Disclosure / PR | Technical Summary | Severity / Status |
| :--- | :--- | :--- | :--- |
| **`EthereumCommonwealth/Dex223-contracts`** (`DEX223` AMM & Periphery) | **[Audit Report #36](https://github.com/rroland10/dex223-bug-bounty/issues/36)** & **[Remediation PR #138](https://github.com/EthereumCommonwealth/Dex223-contracts/pull/138)** | **4 Smart Contract Findings (`F-01..F-04`) at `@ 18fa327`:**<br>1. `Dex223QuoteLib.quoteSwap`: Exact-output (`amountSpecified < 0`) return inversion (`amount1` returned instead of `amount0` when `zeroForOne == true`).<br>2. `Dex223Pool.quoteSwap`: Standard `Error(string)` revert payload corrupted by unconditional `(int256, int256)` ABI decoding.<br>3. `ProtocolFeeCollector`: `300,000` gas stipend exhaustion (OOG) during multi-token ERC-223 conversion.<br>4. `Dex223Pool.swapExactInput`: Missing `amountSpecified > 0` enforcement allowing negative exact-input paths. | **Public Disclosure + Full Upstream Fix PR** |
| **`EthereumCommonwealth/Dex223-contracts`** (`DEX223` Exchange & Listing Registry) | **[Issue #25](https://github.com/rroland10/dex223-bug-bounty/issues/25)** & **[Issue #26](https://github.com/rroland10/dex223-bug-bounty/issues/26)** | **EIP-7702 & Registry Accounting Disclosures:**<br>1. `ERC223` / `Dex223` `extcodesize > 0` contract detection bypass and callback discrepancies on EIP-7702 (`0xef0100`) delegated EOAs.<br>2. `AutoListingsRegistry`: Duplicate fee-token registration overwriting active listing tier accounting. | **Public Disclosure + Foundry PoC** |
| **[`myhubstat/evm-bounty-guard`](https://github.com/myhubstat/evm-bounty-guard)** | **[Release `v1.0.0`](https://github.com/myhubstat/evm-bounty-guard/releases/tag/v1.0.0)** | Zero-dependency static security analyzer for EVM/Solidity smart contracts (`ERC-20`, `ERC-223`, `CREATE2` metamorphic risk, cross-function reentrancy, and unchecked low-level call detectors). | **Open-Source Security Tool** |

---

## 2. Layer-2 Protocol, Confidential Asset & Wallet Security (`Rust`)

| Target Protocol / Repository | Issue / PR | Technical Summary | Status |
| :--- | :--- | :--- | :--- |
| **`tari-project/tari-ootle`** (Tari L2 Protocol & Confidential Wallet Daemon) | **[PR #3135](https://github.com/tari-project/tari-ootle/pull/3135)** ([Issue #3134](https://github.com/tari-project/tari-ootle/issues/3134)) | **`wallet_cli` Live Broadcast on `--dry-run`:** `transactions send` and `confidential-transfer` ignored `--dry-run` and broadcasted live signed transactions to the network. | **MERGED Upstream** |
| **`tari-project/tari-ootle`** (`walletd` Transaction Approval Validation) | **[PR #3158](https://github.com/tari-project/tari-ootle/pull/3158)** ([Issue #3150](https://github.com/tari-project/tari-ootle/issues/3150)) | **Approval Card `amount_leaving: 0` Spoofing:** `value_summaries` counted pre-existing `Unspent`/`Spent` outputs, script-gated outputs, and workspace outputs toward `change_total`, allowing crafted transaction requests to display `amount_leaving: 0` while draining vault inputs. | **Upstream Fix PR + Regression Suite** |
| **`tari-project/tari-ootle`** (`wallet_storage_sqlite` Confidential/Stealth Locks) | **[PR #3157](https://github.com/tari-project/tari-ootle/pull/3157)** ([Issue #3156](https://github.com/tari-project/tari-ootle/issues/3156)) | **Cross-Account Output Lock Overwrite:** `stealth_outputs_get_locked_by_lock_id` and `confidential_outputs_get_locked_by_lock_id` overwrote every returned output's account address with the first row's account. | **Upstream Fix PR + SQLite Test** |
| **`tari-project/tari-ootle`** (`walletd` Stealth Transfer Statements) | **[PR #3155](https://github.com/tari-project/tari-ootle/pull/3155)** ([Issue #3154](https://github.com/tari-project/tari-ootle/issues/3154)) | **Unrecorded Blinded Outputs:** `accounts.create_stealth_transfer_statement` failed to persist wallet-owned blinded outputs as `LockedUnconfirmed`, causing balance desynchronization if statement submission raced. | **Upstream Fix PR** |
| **`tari-project/tari-ootle`** (`wallet_cli` Manifest Execution) | **[PR #3159](https://github.com/tari-project/tari-ootle/pull/3159)** ([Issue #3152](https://github.com/tari-project/tari-ootle/issues/3152)) | **Manifest Blob & Fee Handling:** `submit-manifest` silently dropped `instructions.blobs`, appended duplicate `pay_fee` instructions, and ignored `--dump-outputs-into`. | **Upstream Fix PR** |

---

## 3. Compiler Soundness, Ownership Verification & Static Analysis (`Rust` / `Go`)

| Target Repository | Issue / PR | Technical Summary | Status |
| :--- | :--- | :--- | :--- |
| **`limitlessprojects/Go-Hybrid`** (`hy-check` Borrow & Ownership Verifier) | **[PR #1998](https://github.com/limitlessprojects/Go-Hybrid/pull/1998)** ([Issue #1997](https://github.com/limitlessprojects/Go-Hybrid/issues/1997)) | **Soundness Bypass in `any`-Field Composites & Closure Aliases:** `any`-field composites dropped non-owned/may-share edges (`HYB-E3004`/`HYB-E3003`), and local closure aliases dropped `A3` slot-loan windows (`HYB-E3102`). | **Upstream Soundness Fix** |
| **`limitlessprojects/Go-Hybrid`** (`hy-check` Managed Block Audit Trail) | **[PR #1918](https://github.com/limitlessprojects/Go-Hybrid/pull/1918)** ([Issue #1917](https://github.com/limitlessprojects/Go-Hybrid/issues/1917)) | **Audit Metadata Newline Injection (`HYB-E0003`):** `audit_managed_block` accepted embedded newlines and blank ticket identifiers in unsafe block audit reasons. | **Upstream Soundness Fix** |
| **`limitlessprojects/Go-Hybrid`** (`hy-check` Constant Folding) | **[PR #1897](https://github.com/limitlessprojects/Go-Hybrid/pull/1897)** ([Issue #1896](https://github.com/limitlessprojects/Go-Hybrid/issues/1896)) | **Duplicate Map Key Bypass:** Folded truncated integer conversions (`int8(256)` vs `0`), `-0.0`, and `len("const")` in static duplicate map-key detection. | **Upstream Soundness Fix** |
| **`limitlessprojects/Go-Hybrid`** (`hy-check` / `hy-emit` Core Soundness) | **[PR #1753](https://github.com/limitlessprojects/Go-Hybrid/pull/1753)**, **[PR #1762](https://github.com/limitlessprojects/Go-Hybrid/pull/1762)**, **[PR #1764](https://github.com/limitlessprojects/Go-Hybrid/pull/1764)**, **[PR #1809](https://github.com/limitlessprojects/Go-Hybrid/pull/1809)** | Enforced cross-goroutine channel sendability (`O4`), closure `Result`/`Option` lowering, unary dereference precedence, and `N5` zero-value map initialization. | **`bot:land-candidate` (`9/9 Pareto`)** |
| **`SlopDotCash/slopdotcash`** (Solana Token-2022 & Governance Guards) | **[PR #1435](https://github.com/SlopDotCash/slopdotcash/pull/1435)** & **[PR #1348](https://github.com/SlopDotCash/slopdotcash/pull/1348)** | Preserved verified Solana `Token-2022` program ID across browser vault setup (`MERGED`) and enforced reviewer independence on account-ban appeal denials. | **MERGED Upstream** |
