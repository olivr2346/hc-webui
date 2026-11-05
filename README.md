<h1 align="center">hc-webui</h1>

<p align="center">
  <strong>Herdr-Core Web & Mobile Client</strong><br>
  Live herdr-core workspaces, panes, terminals and AI coding agents in your browser and on your phone.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-666666?labelColor=333333" alt="MIT license"></a>
  <a href="https://github.com/olivr2346/hc-webui"><img src="https://img.shields.io/badge/fork-olivr2346%2Fhc--webui-3b82f6?labelColor=1e293b" alt="Fork"></a>
  <a href="https://github.com/devswha/ovis"><img src="https://img.shields.io/badge/upstream-devswha%2Fovis-64748b?labelColor=1e293b" alt="Upstream"></a>
  <a href="https://github.com/herdrdev/herdr"><img src="https://img.shields.io/badge/runtime-herdr--core-10b981?labelColor=064e3b" alt="herdr-core"></a>
</p>

---

## Overview

**hc-webui** is a fork of [Ovis](https://github.com/devswha/ovis) maintained by [@olivr2346](https://github.com/olivr2346). It is tailored specifically for headless and desktop workflows with **herdr-core** (`hc`), Linux desktop integration (Omarchy / Hyprland), AI coding agents, and Tailscale remote access.

### Fork Enhancements & Differences

- **Antigravity (`agy`) First-Class Chat Support:** Seamless transcript parsing, turn streaming, and fallback conversation discovery when multiple CLI instances or shared presence locks are active.
- **Standalone `herdr-core` Focus:** Operates directly against `herdr-core` IPC socket and daemons, removing reliance on legacy TUI installations.
- **Dedicated Port Management for Tailscale Serve:** Clean non-privileged HTTPS proxy support (e.g. port `7317`), preventing conflicts with port 443 web servers.
- **Systemd User Service Integration:** Managed as `hc-webui.service` with clean environment separation (`~/.config/hc-webui`, `~/.local/state/hc-webui`).

---

## Architecture & System Paths

On Linux / Omarchy, `hc-webui` uses the following directory layout:

| Component | Path | Description |
| :--- | :--- | :--- |
| **App Source / Checkout** | `~/.local/share/hc-webui` | Application build & runtime checkout |
| **Development Workspace** | `~/github/hc-webui` | Git fork workspace for custom development |
| **Configuration** | `~/.config/hc-webui/env` | Persistent environment variables & tokens |
| **Runtime State** | `~/.local/state/hc-webui` | Server logs, pid file, locks |
| **Systemd Service** | `~/.config/systemd/user/hc-webui.service` | Background user service |

### Network & Access

- **Local HTTP:** `http://127.0.0.1:7317`
- **Tailscale HTTPS:** `https://<tailnet-node>.ts.net:7317`

---

## Installation & Setup

### Requirements

- [Bun](https://bun.sh) (`bun-bin` package on Arch Linux / Omarchy)
- [herdr-core](https://herdr.dev/core) (`hc` binary on `PATH`)
- Tailscale (optional, for remote browser & mobile access)

### Quick Install / Reinstall

```bash
# Clone the repository
git clone https://github.com/olivr2346/hc-webui.git ~/.local/share/hc-webui
cd ~/.local/share/hc-webui

# Install dependencies and build client assets
bun install
bun run build

# Run the installer script to register systemd service & configure environment
sh install.sh
```

### Configuration (`~/.config/hc-webui/env`)

```ini
OVIS_RUNTIME=herdr-core
OVIS_HC_BIN=~/.local/bin/herdr-core
OVIS_TAILSCALE_HTTPS_PORT=7317
```

---

## Service Management

Manage the background daemon using systemd:

```bash
# Check status and logs
systemctl --user status hc-webui
journalctl --user -u hc-webui -f

# Start / stop / restart
systemctl --user start hc-webui
systemctl --user stop hc-webui
systemctl --user restart hc-webui
```

Direct script controls:

```bash
bun scripts/plugin.ts status
bun scripts/plugin.ts start
bun scripts/plugin.ts stop
```

---

## Development

Work inside your active dev workspace (`~/github/hc-webui`):

```bash
cd ~/github/hc-webui

# Install dependencies
bun install

# Run Vite frontend dev server (port 5173)
bun run dev

# Run backend API server in terminal (port 7317)
bun run server

# Run build verification
bun run build
```

### Synchronizing with Upstream

To pull updates from the original `devswha/ovis` repository:

```bash
# Fetch latest upstream commits
git fetch upstream

# Rebase or merge changes into main
git checkout main
git rebase upstream/main

# Verify build and tests
bun install
bun run build

# Push to your fork
git push origin main
```

---

## License & Acknowledgments

- **License:** [MIT](LICENSE)
- Original software created by [@devswha](https://github.com/devswha/ovis) and contributors.
- Built on top of [herdr](https://github.com/herdrdev/herdr), [xterm.js](https://xtermjs.org), [React](https://react.dev), and [Bun](https://bun.sh).
