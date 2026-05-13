# p-token

A landing page for [p-token](https://github.com/solana-program/token/tree/main/pinocchio) — a reimplementation of the SPL Token program using the [Pinocchio](https://github.com/anza-xyz/pinocchio) framework.

The page pulls live SPL Token program transactions from Solana mainnet-beta as they confirm, computes what those same transactions would have consumed running on p-token, and renders the savings in real time.

## Run locally

```bash
npx serve . -p 4321
```

Then open <http://localhost:4321>.

No build step. Pure HTML + Tailwind via CDN + `@solana/web3.js` via CDN.

## What's real vs. projected

- **Real:** every `computeUnitsConsumed` value in the live feed comes from `connection.getTransaction()` against the canonical SPL Token program (`TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA`) on mainnet-beta. Slot and TPS come from `getSlot` and `getRecentPerformanceSamples`.
- **Projected:** p-token CU values are computed as `real_cu × 0.45`, an average of the per-instruction reductions documented in the p-token benchmark suite. p-token is not deployed to mainnet — the canonical SPL Token program ID is still served by the original program.

## Architecture

- `index.html` — the entire page (HTML + Tailwind + inline JS)
- `logo.svg` — favicon
- RPC fallback chain: `solana.publicnode.com` → `api.mainnet-beta.solana.com` → `rpc.ankr.com/solana`
- Smooth animated counters via `requestAnimationFrame`, extrapolating forward at the observed CU-per-second rate
- Pure-SPL-Token detection via log parsing (`Program X invoke [1]`)

## Contract

`gENFYvwL1qN8qFJxewhCzyvNXonEHreFXiE6AK8pino`

## License

MIT for this page. Upstream p-token is Apache-2.0.
