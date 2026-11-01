# Solana Token-2022 Vault & Appeal Governance Review (`SlopDotCash/slopdotcash`)

- **Researcher:** Maaz (`@myhubstat`)
- **Upstream PRs:**
  - [`SlopDotCash/slopdotcash#1435`](https://github.com/SlopDotCash/slopdotcash/pull/1435) (**MERGED** via `3f2bb8fc`)
  - [`SlopDotCash/slopdotcash#1348`](https://github.com/SlopDotCash/slopdotcash/pull/1348)

---

## Summary of Findings & Fixes

1. **Solana `Token-2022` Program ID Drop in Browser Vault Setup ([PR #1435](https://github.com/SlopDotCash/slopdotcash/pull/1435) — MERGED):**
   - Browser vault initialization and funding review flows hardcoded the legacy SPL Token program ID instead of preserving the verified `Token-2022` (`TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb`) program ID for Token-2022 mints.
2. **Reviewer Independence on Account-Ban Appeal Denials ([PR #1348](https://github.com/SlopDotCash/slopdotcash/pull/1348)):**
   - Enforced that the administrator who issued an initial account ban cannot unilaterally deny the user's appeal, and preserved active appeal state across quarantine transitions.
