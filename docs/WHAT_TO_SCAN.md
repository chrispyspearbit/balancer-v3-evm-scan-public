# What To Scan (Balancer v3)

## Vault / settlement / deltas
- Look for any path where token deltas can be created/cleared inconsistently (credit/debt accounting).
- Confirm all external calls that can move tokens are gated by the expected lock state.
- Verify fee charging happens exactly once and cannot be bypassed via hooks, buffers, or ERC4626 paths.

## Rounding and scaling
- Identify conversions between raw balances, scaled18 balances, rates, and decimal scaling factors.
- Pay special attention to “round up vs round down” conventions across Vault and pool math.

## Hooks
- Ensure hooks cannot be called by anyone except the Vault (and that the Vault only calls the hook configured for the pool).
- Confirm hook-adjusted amounts cannot be exploited to mint extra BPT or withdraw more than deposited.
- Search for grief vectors: hook reverts, gas bombs, stateful hooks that can permanently brick operations.

## Pool factories
- Validate factory deployment parameters: swap fee bounds, pause windows, role accounts, and hook addresses.
- Confirm clones/implementations cannot be mixed across factories in a way that breaks assumptions.

