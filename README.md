# aave-v3

A [nuthatch](https://github.com/nuthatch-org/nuthatch) nest: **Aave V3 on Ethereum**.

The Pool contract: supplies, borrows, repayments, liquidations, flash loans and reserve updates.

One binary, one config file, no graph-node, no gateway, no query fees.

## What it indexes

**Chain:** `mainnet`. **1 contract**, **15 tables**.

| alias | address |
|---|---|
| `c0` | `0x87870bca3f3fd6335c3f4ce8392d69350b4fa4e2` |

## Verified

Indexed blocks **25,791,631 to 25,811,567** and sealed **51,253 events**. Every table below is generated from the vendored ABIs, and the run above is what this nest actually decoded, not an estimate.

## Checked

On 2026-11-18, with nuthatch 5.22.0, a `--backfill 7200` run indexed from one public RPC (Tenderly's gateway). Over blocks **26,434,000 to 26,439,999** inclusive, the nest's row counts were compared with `eth_getLogs` for the Pool address and each event's topic0, asked of a different public RPC (publicnode) in 1,000-block chunks:

| event | nest | eth_getLogs |
|---|---|---|
| `Supply` | 1,513 | 1,513 |
| `Borrow` | 745 | 745 |

Both match.

```sh
nuthatch sql --dir . 'SELECT count(*) FROM "c0__supply" WHERE block_number BETWEEN 26434000 AND 26439999'
nuthatch sql --dir . 'SELECT reserve, count(*) AS supplies FROM "c0__supply" GROUP BY reserve ORDER BY supplies DESC LIMIT 5'
nuthatch sql --dir . 'SELECT block_number, tx_hash, "user", reserve, amount FROM "c0__borrow" ORDER BY block_number DESC, log_index DESC LIMIT 3'
```

## Run it

```sh
nuthatch init --from https://github.com/nuthatch-org/aave-v3
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
