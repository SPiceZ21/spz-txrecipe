<img src="https://github.com/SPiceZ21/spz-core-media-kit/raw/main/Banner/Banner%232.png" alt="SPiceZ-Core Banner" width="100%">


# 🏎️ SPiceZ-Core: txAdmin Recipe

An open-source racing core for FiveM. This repository contains the official **txAdmin** recipe to automate the deployment of SPiceZ-Core, including all dependencies, modules, and database structures.

## 🚀 Quick Start

To install SPiceZ-Core using this recipe:

1. Open your **txAdmin** dashboard.
2. Go to **Server Setup** and choose **Remote URL Template**.
3. Paste the following RAW URL:
   ```text
   https://raw.githubusercontent.com/SPiceZ21/spz-txrecipe/main/spz-recipe.yaml
   ```
4. Follow the prompts to configure your server name, license key, and database connection.

---

## 🛠️ What it Automates

This recipe handles the "heavy lifting" so you can get to racing faster:

- **Resource Download**: Fetches `cfx-server-data`, `oxmysql`, and `ox_lib`.
- **SPZ Modules**: Installs the full SPiceZ suite:
  - `spz-lib`, `spz-core`, `spz-identity`, `spz-vehicles`, `spz-races`, `spz-progression`, `spz-hud`.
- **Database Initialization**: Creates 8 essential tables for player profiles, racing sessions, vehicle customization, and economy tracking.
- **Auto-Config**: Writes a production-ready `server.cfg` with:
  - `sv_enforceGameBuild 3095` (Agents of Sabotage).
  - Pre-configured ACE permissions for `group.admin`.
  - Automatic loading of all resources in the correct order.

---

## 📋 Requirements

- **Server Artifact**: Build `27926` or higher is recommended ([Download](https://runtime.fivem.net/artifacts/fivem/build_server_windows/master/27926-b6a8e13b4b56af0658e605d9e5c68a3c24e67616/server.zip)).
- **Database**: A running MySQL/MariaDB instance (e.g., XAMPP).
- **FiveM License Key**: Get one at [keymaster.fivem.net](https://keymaster.fivem.net/).

---

## 🛠️ Development Setup

If you are a developer looking to edit the modules live:

1. Run the recipe as normal.
2. Delete the folders in `resources/[spz]/` that you wish to modify.
3. Replace them with **symlinks** pointing to your local repository clones.

---

## ⚖️ License

SPiceZ-Core is open-source. See the repository modules for individual licenses.
