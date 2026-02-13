# Balancer v3 EVM Scan Pack (Ethereum mainnet)

This repo is a public scan pack for Balancer v3 contracts: upstream source entrypoint + deployed-address dossiers + Etherscan-verified source + ABIs + runtime bytecode dumps.

## Important: Deployed vs Upstream

- If you want to scan what is **actually deployed on Ethereum mainnet**, use:
  - `etherscan_verified/` (Etherscan API v2 verified source + ABI per deployed address)
  - `bytecode/` (runtime bytecode hex + hashes fetched from an Ethereum RPC)
  - `deployments/balancer-deployments/` (official deployment outputs + artifacts)

- `upstream/balancer-v3-monorepo/` is an **upstream source snapshot** (open-source entrypoint) and can differ from
  the specific code currently deployed at any given address.

Included:
- Curated upstream source snapshot from `balancer/balancer-v3-monorepo` under `upstream/` (see `UPSTREAM_COMMIT.txt`).
- Official deployment outputs/artifacts (subset) under `deployments/balancer-deployments/`.
- Etherscan API v2 `getsourcecode` dumps (verified source + ABI) under `etherscan_verified/`.
- Runtime bytecode hex + hashes under `bytecode/` (from an Ethereum RPC).
- A Balancer v3 top-3 pools by TVL snapshot under `scans/ethereum_top3/` (includes on-chain TVL estimate and verified source).

## Mainnet Addresses (high signal)

- `20241204-v3-vault:Vault`: `0xbA1333333333a1BA1108E8412f11850A5C319bA9`
- `20241204-v3-vault:VaultExtension`: `0x0E8B07657D719B86e06bF0806D6729e3D528C9A9`
- `20241204-v3-vault:VaultAdmin`: `0x35fFB749B273bEb20F40f35EdeB805012C539864`
- `20250407-v3-vault-explorer-v2:VaultExplorer`: `0xFc2986feAB34713E659da84F3B1FA32c1da95832`
- `20241204-v3-vault:ProtocolFeeController`: `0xa731C23D7c95436Baaae9D52782f966E1ed07cc8`
- `20250307-v3-router-v2:Router`: `0xAE563E3f8219521950555F5962419C8919758Ea2`
- `20241205-v3-batch-router:BatchRouter`: `0x136f1EFcC3f8f88516B9E94110D56FDBfB1778d1`
- `20250123-v3-composite-liquidity-router-v2:CompositeLiquidityRouter`: `0xb21A277466e7dB6934556a1Ce12eb3F032815c8A`
- `20260116-v3-stable-pool-v3:StablePoolFactory`: `0x4eFcd8bcE8AC9b94bd76648e2c85bEf6c40F3228`
- `20260115-v3-weighted-pool-v2:WeightedPoolFactory`: `0x332694Ef46D880DF6Ea9593e04CB8ABEE5F81D99`
- `20260117-v3-stable-surge-pool-factory-v3:StableSurgePoolFactory`: `0x187a05fb9e4234Dd310ae74215743560D1BAA6Ac`
- `20250403-v3-stable-surge-hook-v2:StableSurgeHook`: `0xBDbADc891BB95DEE80eBC491699228EF0f7D6fF1`
- `top3_pool#1`: `0x85b2b559bc2d21104c4defdd6efca8a20343361d`
- `top3_pool#2`: `0xae255db04ba78519f33871c557d8fd6bafdb83bd`
- `top3_pool#3`: `0x1ea5870f7c037930ce1d5d8d9317c670e89e13e3`

## Where To Look For Critical Bugs

Vault-centric risk surface (highest priority):
- Balance accounting invariants across `swap`, `addLiquidity`, `removeLiquidity`, settlement, and fee-charging.
- Rounding direction mismatches between Vault and pool math (classic Balancer exploit class).
- Yield fee + swap fee aggregation (double-charging or bypass).
- Reentrancy and lock/unlock assumptions (any path that mutates balances outside the expected unlock session).
- Hook call ordering and hook-adjusted amounts: hooks that can grief/DoS, steal via delta misreporting, or permanently trap liquidity.

Pool math risks:
- Stable math edge cases: extreme imbalance, token decimals/rates, amplification/fee params.
- Weighted math: weight normalization, scaling factors, swap limits, join/exit math.

Rate providers / ERC4626 / wrappers:
- Any token with a rate provider or ERC4626 wrapping: stale rates, rounding, and “useUnderlying vs useWrapped” toggles.
