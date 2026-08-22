# aave-v3

A [nuthatch](https://github.com/nightswatchhq/nuthatch) nest: **Aave V3 on Ethereum**.

The Pool contract: supplies, borrows, repayments, liquidations, flash loans and reserve updates.

One binary, one config file, no graph-node, no gateway, no query fees.

## What it indexes

**Chain:** `mainnet`. **1 contract**, **15 tables**.

| alias | address |
|---|---|
| `c0` | `0x87870bca3f3fd6335c3f4ce8392d69350b4fa4e2` |

## Verified

Indexed blocks **25,791,631 to 25,811,567** and sealed **51,253 events**. Every table below is generated from the vendored ABIs, and the run above is what this nest actually decoded, not an estimate.

## Run it

```sh
nuthatch init --from https://github.com/nightswatchhq/aave-v3
cd aave-v3
nuthatch dev --dir . --backfill 50000 --seal-direct
nuthatch sql --dir . "SELECT count(*) FROM \"c0__borrow\""
```

The endpoint in `nuthatch.toml` is keyless and public, so this file is publishable: a `nuthatch.toml` is pinned into the nest's content address and must never carry a credential. It is enough to follow the tip. A **backfill** wants archive depth it may not have: pass your own with `--rpc`, and check it first with `nuthatch doctor --rpc <url>`.

## Tables

```
c0__borrow
c0__deficit_covered
c0__deficit_created
c0__flash_loan
c0__liquidation_call
c0__minted_to_treasury
c0__position_manager_approved
c0__position_manager_revoked
c0__repay
c0__reserve_data_updated
c0__reserve_used_as_collateral_disabled
c0__reserve_used_as_collateral_enabled
c0__supply
c0__user_e_mode_set
c0__withdraw
```
