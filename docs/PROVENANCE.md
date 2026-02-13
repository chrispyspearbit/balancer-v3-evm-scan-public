# Provenance

## Upstream source entrypoint
- `upstream/balancer-v3-monorepo/` is copied from `balancer/balancer-v3-monorepo` at the commit listed in `upstream/balancer-v3-monorepo/UPSTREAM_COMMIT.txt`.

## Deployed addresses
- `deployments/balancer-deployments/` is copied from the official `balancer/balancer-deployments` repo task outputs/artifacts (subset).

## Verified source
- `etherscan_verified/` is pulled from Etherscan API v2 `contract/getsourcecode` for each address. Files include ABI and either flattened source or standard-json sources (multi-file).

## Runtime bytecode
- `bytecode/` is fetched from an Ethereum RPC via `eth_getCode` at the time the pack was generated; hashes are included for reproducibility.

