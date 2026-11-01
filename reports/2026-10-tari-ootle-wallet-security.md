# Tari Ootle Layer-2 Confidential Wallet & Daemon Security Review (`tari-project/tari-ootle`)

- **Researcher:** Maaz (`@myhubstat`)
- **Upstream Issues & Fix PRs:**
  - [`tari-project/tari-ootle#3135`](https://github.com/tari-project/tari-ootle/pull/3135) (**MERGED**) — Fixes [#3134](https://github.com/tari-project/tari-ootle/issues/3134)
  - [`tari-project/tari-ootle#3158`](https://github.com/tari-project/tari-ootle/pull/3158) — Fixes [#3150](https://github.com/tari-project/tari-ootle/issues/3150)
  - [`tari-project/tari-ootle#3157`](https://github.com/tari-project/tari-ootle/pull/3157) — Fixes [#3156](https://github.com/tari-project/tari-ootle/issues/3156)
  - [`tari-project/tari-ootle#3155`](https://github.com/tari-project/tari-ootle/pull/3155) — Fixes [#3154](https://github.com/tari-project/tari-ootle/issues/3154)

---

## Summary of Findings & Fixes

1. **`walletd` Approval Card Value Summary Spoofing ([#3150](https://github.com/tari-project/tari-ootle/issues/3150) / [PR #3158](https://github.com/tari-project/tari-ootle/pull/3158)):**
   - `value_summaries` credited pre-existing `Unspent`/`Spent` outputs, non-owner script outputs, and workspace outputs toward `change_total`, allowing a malicious dApp transaction request to display `amount_leaving: 0` while spending the user's vault funds.
2. **SQLite Locked Output Cross-Account Overwrite ([#3156](https://github.com/tari-project/tari-ootle/issues/3156) / [PR #3157](https://github.com/tari-project/tari-ootle/pull/3157)):**
   - `stealth_outputs_get_locked_by_lock_id` and `confidential_outputs_get_locked_by_lock_id` resolved only the first row's `account_id` and overwrote every subsequent locked output's account address.
3. **Unrecorded Blinded Outputs in Stealth Transfer Statements ([#3154](https://github.com/tari-project/tari-ootle/issues/3154) / [PR #3155](https://github.com/tari-project/tari-ootle/pull/3155)):**
   - `accounts.create_stealth_transfer_statement` locked input funds without recording newly generated wallet-owned blinded outputs as `LockedUnconfirmed`.
4. **Live Broadcast During `--dry-run` Transfers ([#3134](https://github.com/tari-project/tari-ootle/issues/3134) / [PR #3135](https://github.com/tari-project/tari-ootle/pull/3135) — MERGED):**
   - `wallet_cli` ignored `--dry-run` in `transactions send` and `confidential-transfer`, broadcasting live transactions instead of simulating execution.
