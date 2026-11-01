# Go-Hybrid Compiler Soundness & Borrow Verification Review (`limitlessprojects/Go-Hybrid`)

- **Researcher:** Maaz (`@myhubstat`)
- **Upstream Soundness & Codegen PRs:**
  - [`limitlessprojects/Go-Hybrid#1998`](https://github.com/limitlessprojects/Go-Hybrid/pull/1998) — Fixes [#1997](https://github.com/limitlessprojects/Go-Hybrid/issues/1997)
  - [`limitlessprojects/Go-Hybrid#1918`](https://github.com/limitlessprojects/Go-Hybrid/pull/1918) — Fixes [#1917](https://github.com/limitlessprojects/Go-Hybrid/issues/1917)
  - [`limitlessprojects/Go-Hybrid#1897`](https://github.com/limitlessprojects/Go-Hybrid/pull/1897) — Fixes [#1896](https://github.com/limitlessprojects/Go-Hybrid/issues/1896)
  - [`limitlessprojects/Go-Hybrid#1809`](https://github.com/limitlessprojects/Go-Hybrid/pull/1809) — Fixes [#1808](https://github.com/limitlessprojects/Go-Hybrid/issues/1808)
  - [`limitlessprojects/Go-Hybrid#1753`](https://github.com/limitlessprojects/Go-Hybrid/pull/1753) — Fixes [#1752](https://github.com/limitlessprojects/Go-Hybrid/issues/1752)
  - [`limitlessprojects/Go-Hybrid#1762`](https://github.com/limitlessprojects/Go-Hybrid/pull/1762) — Fixes [#1761](https://github.com/limitlessprojects/Go-Hybrid/issues/1761)
  - [`limitlessprojects/Go-Hybrid#1764`](https://github.com/limitlessprojects/Go-Hybrid/pull/1764) — Fixes [#1763](https://github.com/limitlessprojects/Go-Hybrid/issues/1763)

---

## Summary of Soundness Findings & Fixes

1. **`any`-Field Composite Share Tracking & Closure Alias `A3` Loan Windows ([#1997](https://github.com/limitlessprojects/Go-Hybrid/issues/1997) / [PR #1998](https://github.com/limitlessprojects/Go-Hybrid/pull/1998)):**
   - Composites containing `any` fields dropped `HYB-E3004`/`HYB-E3003` share-tracking edges, and closure variable aliases failed to propagate active `A3` slot-loan windows (`HYB-E3102`).
2. **Managed Block Audit Metadata Injection ([#1917](https://github.com/limitlessprojects/Go-Hybrid/issues/1917) / [PR #1918](https://github.com/limitlessprojects/Go-Hybrid/pull/1918)):**
   - `audit_managed_block` accepted raw newline characters and empty ticket references in `managed` block justification strings (`HYB-E0003`).
3. **Static Duplicate Map Key Folding ([#1896](https://github.com/limitlessprojects/Go-Hybrid/issues/1896) / [PR #1897](https://github.com/limitlessprojects/Go-Hybrid/pull/1897)):**
   - Constant folding missed truncated integer casts (`int8(256)` colliding with `0`), negative zero (`-0.0`), and `len("const")` expressions in map literals.
4. **Cross-Goroutine Channel Sendability (`O4`) & Zero-Value Map Initialization (`N5`) ([PR #1753](https://github.com/limitlessprojects/Go-Hybrid/pull/1753), [PR #1809](https://github.com/limitlessprojects/Go-Hybrid/pull/1809)):**
   - Rejected non-sendable channel element types across `spawn` boundaries and enforced `N5` zero-map initialization on named return parameters and `?` error-propagation paths.
