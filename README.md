<div align="center">

<img src="https://github.com/SPiceZ21/spz-core-media-kit/raw/main/Banner/Banner%232.png" alt="SPiceZ-Core Banner" width="100%"/>

<br/>

# spz-txrecipe

### txAdmin Deployment Recipe

*One-click server deployment for the full SPiceZ-Core stack. Handles dependencies, module downloads, database initialization, and `server.cfg` generation automatically.*

<br/>

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-orange.svg?style=flat-square)](https://www.gnu.org/licenses/gpl-3.0)
[![FiveM](https://img.shields.io/badge/FiveM-Compatible-orange?style=flat-square)](https://fivem.net)
[![txAdmin](https://img.shields.io/badge/txAdmin-Recipe-orange?style=flat-square)](https://github.com/tabarra/txAdmin)
[![Status](https://img.shields.io/badge/Status-In%20Development-green?style=flat-square)]()

</div>

---

## Overview

`spz-txrecipe` is the official **txAdmin** YAML recipe for deploying a complete SPiceZ-Core racing server from scratch. Instead of manually cloning repositories, importing SQL schemas, and writing `server.cfg`, the recipe automates the entire process through the txAdmin setup wizard.

After running the recipe, you'll have a production-ready racing server with all modules installed, all database tables created, and all resources loading in the correct order.

---

## Quick Start

1. Open your **txAdmin** dashboard.
2. Go to **Server Setup** → **Remote URL Template**.
3. Paste the recipe URL:

   ```
   https://raw.githubusercontent.com/SPiceZ21/spz-txrecipe/main/spz-recipe.yaml
   ```

4. Follow the prompts to configure your server name, license key, and database connection.
5. txAdmin handles the rest — sit back and let it run.

---

## What the Recipe Automates

### Resource Downloads
- `cfx-server-data` — base server data
- `oxmysql` — MySQL async driver
- `ox_lib` — shared library (callbacks, notifications, menus, dialogs)
- `fivem-appearance` — standalone character creator and clothing
- `pma-voice` — proximity voice
- `screenshot-basic`, `vMenu`

### SPiceZ Module Installation

All `spz-*` modules are installed and written into `server.cfg` in dependency
order — `spz-rpc` → `spz-loading` → `spz-core` → `spz-identity` →
`spz-appearance` → `spz-spawn`, then the racing modules
(`spz-speedcam`, `spz-vehicles`, `spz-races`, `spz-progression`, `spz-nametag`,
`spz-poll`, `spz-raceUI`, `spz-leaderboard`, `spz-carspawner`, `spz-fpscap`,
`spz-speedometer`, `spz-nos`, `spz-vehfunc`), with `vMenu` last.

### Database Initialization

The recipe only stores the connection credentials (`connect_database`). The
schema is owned by **spz-core**: every file in `spz-core/migrations/` is applied
on first boot and recorded in the `spz_migrations` ledger, so restarts and
upgrades are no-ops rather than re-imports.

| Table | Purpose |
|---|---|
| `players` | Player profiles, stats, license tiers |
| `crews` | Crew groups and ownership |
| `driver_licenses` | License unlock history per player |
| `vehicle_customizations` | Saved per-vehicle cosmetic setups |
| `race_sessions` | Per-race metadata |
| `race_results` | Per-player race results |
| `track_records` | Personal bests per track and class |
| `economy_transactions` | Full credits ledger |
| `player_outfits` | Saved personal outfits |
| `player_badges` | Awarded badges |
| `speedcam_bests` | Speed camera personal bests and records |

To change the schema, add a new numbered file to `spz-core/migrations/` and list
it in `spz-core/server/migrations.lua`. Never edit a migration that has shipped.

### Auto-Generated `server.cfg`

The recipe writes a production-ready config including:

- `sv_enforceGameBuild 3407`
- `sv_stateBagStrictMode true` (clients cannot write replicated state bags)
- Pre-configured ACE permissions for `group.admin`
- All resources loading in the correct dependency order
- Commented sections for easy customization

---

## Requirements

| Requirement | Notes |
|---|---|
| Server artifact | Build `27926` or higher ([Download](https://runtime.fivem.net/artifacts/fivem/build_server_windows/master/)) |
| MySQL / MariaDB | Running instance (XAMPP works locally) |
| FiveM license key | Obtain at [keymaster.fivem.net](https://keymaster.fivem.net/) |

---

## Manual Install

If you prefer to install without txAdmin, follow the manual load order:

```cfg
# ── Dependencies ──────────────────────────────────────
ensure oxmysql
ensure ox_lib
ensure fivem-appearance
ensure pma-voice
ensure screenshot-basic        # optional

# ── SPiceZ Core ───────────────────────────────────────
ensure spz-rpc
ensure spz-loading
ensure spz-core
ensure spz-identity
ensure spz-appearance
ensure spz-spawn

# ── Racing Modules ────────────────────────────────────
ensure spz-speedcam
ensure spz-vehicles
ensure spz-races
ensure spz-progression
ensure spz-nametag
ensure spz-poll
ensure spz-raceUI
ensure spz-leaderboard
ensure spz-carspawner
ensure spz-physics
ensure spz-fpscap
ensure spz-speedometer
ensure spz-nos
ensure spz-vehfunc

# ── Admin (always last) ───────────────────────────────
ensure vMenu
```

---

## Developer Setup

If you want to edit modules live while using the recipe deployment:

1. Run the recipe as normal to get the base installation.
2. Delete the specific module folder(s) from `resources/[spz]/` that you want to modify.
3. Replace them with **symlinks** pointing to your local repository clones.

This gives you a full server environment while still editing from your working directory.

---

## Post-Install Configuration

After the recipe completes, these files are the primary places to tune your server:

| File | Purpose |
|---|---|
| `resources/[spz]/spz-core/config.lua` | Core settings (intermission time, max players, allowed classes) |
| `resources/[spz]/spz-races/config.lua` | Race settings (poll duration, timeout, cycle order) |
| `resources/[spz]/spz-physics/config.lua` | Powertrain simulation, gearing, PP rating |
| `resources/[spz]/spz-speedometer/config.lua` | Speedometer units and display |
| `server.cfg` | ACE permissions, RCON password, server name |

---

<div align="center">

*Part of the [SPiceZ-Core](https://github.com/SPiceZ21) ecosystem*

**[Docs](https://github.com/SPiceZ21/spz-docs) · [Discord](https://discord.gg/) · [Issues](https://github.com/SPiceZ21/spz-txrecipe/issues)**

</div>
