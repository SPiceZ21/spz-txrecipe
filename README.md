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
   https://raw.githubusercontent.com/SPiceZ-Core/spz-txrecipe/main/spz-recipe.yaml
   ```

4. Follow the prompts to configure your server name, license key, and database connection.
5. txAdmin handles the rest — sit back and let it run.

---

## What the Recipe Automates

### Resource Downloads
- `cfx-server-data` — base server data
- `oxmysql` — MySQL async driver
- `ox_lib` — shared utility library

### SPiceZ Module Installation

All core `spz-*` modules installed in the correct load order:

```
spz-lib → spz-loading → spz-core → spz-identity → spz-vehicles → spz-races → spz-progression → spz-hud
```

### Database Initialization

Creates 8 essential tables:

| Table | Purpose |
|---|---|
| `players` | Player profiles, stats, license tiers |
| `crews` | Crew groups and ownership |
| `driver_licenses` | License unlock history per player |
| `economy_transactions` | Full credits ledger |
| `race_sessions` | Per-race metadata |
| `race_results` | Per-player race results |
| `track_records` | Personal bests per track and class |
| `owned_vehicles` | Garage vehicle registry |

### Auto-Generated `server.cfg`

The recipe writes a production-ready config including:

- `sv_enforceGameBuild 3095` (Agents of Sabotage build)
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
ensure screenshot-basic        # optional

# ── SPiceZ Core ───────────────────────────────────────
ensure spz-lib
ensure spz-loading
ensure spz-core

# ── Modules ───────────────────────────────────────────
ensure spz-identity
ensure spz-vehicles
ensure spz-economy
ensure spz-garage
ensure spz-tuning
ensure spz-races
ensure spz-progression
ensure spz-leaderboard
ensure spz-weather             # optional

# ── UI ────────────────────────────────────────────────
ensure spz-hud

# ── Admin (always last) ───────────────────────────────
ensure spz-admin
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
| `resources/[spz]/spz-hud/config.lua` | HUD display settings (units, animation durations) |
| `server.cfg` | ACE permissions, RCON password, server name |

---

<div align="center">

*Part of the [SPiceZ-Core](https://github.com/SPiceZ-Core) ecosystem*

**[Docs](https://github.com/SPiceZ-Core/spz-docs) · [Discord](https://discord.gg/) · [Issues](https://github.com/SPiceZ-Core/spz-txrecipe/issues)**

</div>
