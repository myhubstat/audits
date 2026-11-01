# DEX223 Smart Contract Security Audit (`EthereumCommonwealth/Dex223-contracts @ 18fa327`)

- **Researcher:** Maaz (`@myhubstat`)
- **Public Disclosure:** [`rroland10/dex223-bug-bounty#36`](https://github.com/rroland10/dex223-bug-bounty/issues/36), [`#25`](https://github.com/rroland10/dex223-bug-bounty/issues/25), [`#26`](https://github.com/rroland10/dex223-bug-bounty/issues/26)
- **Upstream Remediation PR:** [`EthereumCommonwealth/Dex223-contracts#138`](https://github.com/EthereumCommonwealth/Dex223-contracts/pull/138)

---

## Executive Summary

During an independent security review of `EthereumCommonwealth/Dex223-contracts` (the core concentrated-liquidity AMM pool, quoting library, fee collector, and token standard adapters), we identified and submitted fixes for multiple vulnerabilities affecting swap quoting, revert decoding, protocol fee collection, and EIP-7702 account delegation:

1. **`F-01` (`Dex223QuoteLib.quoteSwap`):** Exact-output (`amountSpecified < 0`) swap simulations returned `amount1` instead of `amount0` when `zeroForOne == true`, inverting the required input quote for callers.
2. **`F-02` (`Dex223Pool.quoteSwap`):** Unconditional ABI decoding of `(int256, int256)` on revert bytes corrupted standard `Error(string)` and `Panic(uint256)` revert reasons when `swap` reverted before reaching the quote callback.
3. **`F-03` (`ProtocolFeeCollector`):** Hardcoded `300,000` gas stipend on ERC-223 token conversion calls caused Out-Of-Gas reverts for multi-hop or hook-enabled tokens.
4. **`F-04` (`Dex223Pool.swapExactInput`):** Missing `require(amountSpecified > 0)` check allowed negative `amountSpecified` values to enter the exact-input execution path.
5. **`EIP-7702` & `AutoListingsRegistry` (`#25` / `#26`):** `extcodesize > 0` checks on EIP-7702 (`0xef0100`) delegated EOAs triggered `tokenReceived` callbacks on accounts without ERC-223 receiver logic, and duplicate fee tokens in `AutoListingsRegistry` overwrote existing tier configurations.
