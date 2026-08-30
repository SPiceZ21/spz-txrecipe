# spz-txrecipe

> txAdmin deployment recipe and generated `server.cfg`

## Overview

`spz-txrecipe` is the official txAdmin YAML recipe for deploying a full SPiceZ-Core racing
server. Instead of cloning repos, importing SQL and hand-writing `server.cfg`, the wizard
downloads every dependency and module, stores the database connection, and writes a
production-ready config in the correct load order.

## Quick start

1. txAdmin dashboard → **Server Setup** → **Remote URL Template**.
2. Paste:
   ```
   https://raw.githubusercontent.com/SPiceZ21/spz-txrecipe/main/spz-recipe.yaml
   ```
3. Enter server name, license key and database connection.
4. Let it run, then boot the server.

## Requirements

| Requirement | Notes |
|---|---|
| Server artifact | Build `27926` or newer |
| MySQL / MariaDB | Running instance (XAMPP is fine locally) |
| FiveM license key | [keymaster.fivem.net](https://keymaster.fivem.net/) |

## Contents

| File | Purpose |
|---|---|
| `spz-recipe.yaml` | The txAdmin recipe |
| `server.cfg` | Generated server config with full load order |
| `permissions.cfg` | ACE permission defaults |

## What the recipe installs

**Third-party** — `cfx-server-data`, `oxmysql`, `ox_lib`, `fivem-appearance`, `pma-voice`,
`screenshot-basic`, `screencapture`, `vMenu`.

**Modules** — installed into `resources/[spz]/` and written into `server.cfg` in dependency
order: `spz-rpc` → `spz-loading` → `spz-core` → `spz-identity` → `spz-appearance` →
`spz-spawn`, then `spz-speedcam`, `spz-vehicles`, `spz-races`, `spz-progression`,
`spz-nametag`, `spz-poll`, `spz-raceUI`, `spz-leaderboard`, `spz-carspawner`, `spz-fpscap`,
`spz-raceline`, `spz-speedometer`, `spz-vehfunc`, `spz-tunners`, `spz-spectate`,
`spz-spectate` — with `vMenu` last.

> **Not included:** `spz-physics` is not downloaded by the recipe. Its `ensure` line in
> `server.cfg` ships commented out — install the resource manually and uncomment the line
> if you want the powertrain simulation.

## Database

The recipe stores connection credentials only (`connect_database`). The schema is owned by
[spz-core](../spz-core/README.md): every file in `spz-core/migrations/` is applied on first
boot and recorded in `spz_migrations`, so restarts and upgrades are no-ops.

| Table | Purpose |
|---|---|
| `players` | Profiles, stats, license tiers |
| `crews` | Crew groups and ownership |
| `driver_licenses` | License unlock history |
| `vehicle_customizations` | Saved cosmetic setups |
| `race_sessions` · `race_results` | Race metadata and per-player results |
| `track_records` | Personal bests per track and class |
| `racelines` | Stored best-lap lines |
| `economy_transactions` | Credits ledger |
| `player_outfits` · `player_badges` | Outfits and awards |
| `speedcam_bests` | Speed camera records |

To change the schema, add a new numbered file to `spz-core/migrations/`. Never edit one
that has shipped.

## Generated `server.cfg`

- `sv_enforceGameBuild 3407`
- `sv_stateBagStrictMode true` — clients cannot write replicated state bags
- ACE permissions pre-configured for `group.admin`
- All resources in dependency order, with commented sections

## Post-install tuning

| File | Purpose |
|---|---|
| `resources/[spz]/spz-core/config.lua` | Core settings — intermission, max players, classes |
| `resources/[spz]/spz-races/config.lua` | Poll duration, timeouts, cycle order |
| `resources/[spz]/spz-physics/config.lua` | Powertrain, gearing, PP bands |
| `server.cfg` | ACE permissions, RCON password, server name |

## Developing against a deployed server

1. Run the recipe as normal.
2. Delete the module folders you want to work on from `resources/[spz]/`.
3. Symlink them to your local clones.

## Version notes

`v3.0.0` — `spz-lib` dropped for `ox_lib`; `illenium-appearance` replaced by
`fivem-appearance`; `spz-orb`, `spz-stance` and `spz-tablet` removed; schema moved to
`spz-core` migrations.

---

Part of [SPiceZ-Core](../README.md) · GPL-3.0
