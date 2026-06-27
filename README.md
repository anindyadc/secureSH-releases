<div align="center">

<img src="https://raw.githubusercontent.com/anindyadc/secureSH-releases/main/assets/logo.png" alt="secureSH" width="96" height="96" />

# secureSH

**A fast, secure SSH session manager for DevOps engineers**

Built with Tauri v2 · Rust · React · xterm.js

[![Latest Release](https://img.shields.io/github/v/release/anindyadc/secureSH-releases?label=latest&color=6c91c2&style=flat-square)](https://github.com/anindyadc/secureSH-releases/releases/latest)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Windows%20%7C%20Linux-cba6f7?style=flat-square)](#installation)
[![Built with Tauri](https://img.shields.io/badge/built%20with-Tauri%20v2-24c8db?style=flat-square)](https://tauri.app)

[**Download**](#installation) · [Release Notes](#release-history) · [Features](#features) · [Security](#security)

</div>

---

## What is secureSH?

secureSH is a cross-platform SSH session manager inspired by SecureCRT, designed for developers and DevOps engineers who need a fast, keyboard-friendly terminal client with a built-in encrypted credential vault.

Sessions, passwords, keys, and tunnel configs are stored in an **AES-256-GCM encrypted vault** protected by a master password — nothing is stored in plaintext on disk. The UI is built on **xterm.js with a WebGL renderer** for smooth, high-throughput terminal output.

> Source code is maintained in a private repository. This repository hosts release binaries only.

---

## Installation

### macOS

| Chip | Download |
|---|---|
| Apple Silicon (M1/M2/M3/M4) | [secureSH_0.3.0_aarch64.dmg](https://github.com/anindyadc/secureSH-releases/releases/latest/download/secureSH_0.3.0_aarch64.dmg) |
| Intel | [secureSH_0.3.0_x64.dmg](https://github.com/anindyadc/secureSH-releases/releases/latest/download/secureSH_0.3.0_x64.dmg) |

1. Open the `.dmg` and drag `secureSH.app` to `/Applications`
2. On first launch macOS may show **"secureSH is damaged and can't be opened"** — the app is not damaged, this is Gatekeeper blocking an unsigned build. Run once in Terminal:
   ```bash
   xattr -cr /Applications/secureSH.app
   ```
3. Double-click the app normally.

> The right-click → Open bypass works for "unidentified developer" warnings but does **not** fix the "damaged" error. Only `xattr -cr` fixes it.

**System requirements:** macOS 12 Monterey or later

---

### Windows

| Installer | Download |
|---|---|
| NSIS (recommended) | [secureSH_0.3.0_x64-setup.exe](https://github.com/anindyadc/secureSH-releases/releases/latest/download/secureSH_0.3.0_x64-setup.exe) |
| MSI (silent / enterprise) | [secureSH_0.3.0_x64_en-US.msi](https://github.com/anindyadc/secureSH-releases/releases/latest/download/secureSH_0.3.0_x64_en-US.msi) |

Run the installer. If Windows SmartScreen appears, click **More info → Run anyway**.

**System requirements:** Windows 10 64-bit or later with [WebView2 Runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/) (pre-installed on Windows 11; download separately for Windows 10)

---

### Linux

| Package | Download | For |
|---|---|---|
| AppImage | [secureSH_0.3.0_amd64.AppImage](https://github.com/anindyadc/secureSH-releases/releases/latest/download/secureSH_0.3.0_amd64.AppImage) | Any x86_64 distro (Fedora, Arch, openSUSE…) |
| Debian / Ubuntu | [secureSH_0.3.0_amd64.deb](https://github.com/anindyadc/secureSH-releases/releases/latest/download/secureSH_0.3.0_amd64.deb) | Ubuntu 22.04+ / Debian 12+ |

```bash
# AppImage — no install needed, runs anywhere
chmod +x secureSH_0.3.0_amd64.AppImage
./secureSH_0.3.0_amd64.AppImage

# Debian / Ubuntu
sudo dpkg -i secureSH_0.3.0_amd64.deb
```

**System requirements:** Ubuntu 22.04+ / Debian 12+ (x86_64). AppImage is self-contained and runs on any modern x86_64 Linux distribution.

> **Auto-update on Linux:** the Tauri updater supports AppImage auto-update. `.deb` installs require manually downloading the new `.deb` from the releases page.

---

## In-App Updates

Once you are on v0.3.0 or later, future updates can be applied without visiting this page:

1. Open **Settings** (`Ctrl+,` or the gear icon at the bottom of the sidebar)
2. Go to the **About** tab
3. Click **Check for Updates**
4. If an update is available → click **Install & Restart**

The app downloads the signed update in the background, verifies the cryptographic signature, applies it, and relaunches automatically.

---

## Features

<details>
<summary><strong>SSH Connections & Authentication</strong></summary>

- **Multi-tab sessions** — each sidebar click opens a new independent SSH connection; unlimited concurrent sessions
- **Auth methods (in priority order):** Public Key (RSA-4096, ED25519, ECDSA), GSSAPI/Kerberos, Keyboard-Interactive / 2FA / OTP, Password, SSH Agent
- **Jump host / bastion** — independent credentials per jump host; supports password, key, or agent auth on the jump independently of the target
- **Host key verification** — Trust On First Use (TOFU) with hard reject on changed keys; stored in `~/.ssh/securesh_known_hosts`; no "accept anyway" path for changed keys
- **Agent forwarding** — propagates `SSH_AUTH_SOCK` to the remote host
- **Send-on-connect** — configurable startup commands typed automatically after authentication
- **Auto-reconnect** — exponential backoff (5 attempts, 1 s → 30 s) for key/agent auth sessions on unexpected disconnects; clean exits (`exit`, `Ctrl+D`) show a manual reconnect overlay instead
- **Application keepalive** — optional no-op exec sent at a configurable interval (Settings → Terminal → Session Keepalive) to prevent AWS EC2 / cloud firewall idle timeouts

</details>

<details>
<summary><strong>Terminal</strong></summary>

- **WebGL renderer** — xterm.js with WebGL for fast, smooth output on large scrollbacks (default 10,000 lines)
- **Full colour support** — xterm-256color, true colour, Unicode 11, clickable web links, inline images
- **Multi-pane layout** — single, side-by-side, stacked, or 2×2 grid; switch with a one-click layout picker; draggable split ratio
- **Find bar** — `Ctrl+F` opens in-terminal search with highlighted matches (next: `Enter`, prev: `Shift+Enter`)
- **Font controls** — `Ctrl+=`/`Ctrl+-`/`Ctrl+0` zoom in/out/reset; persisted across sessions
- **Right-click context menu** — Copy, Paste, Select All & Copy, Search Selection, Clear Scrollback
- **PuTTY mouse mode** — right-click pastes, selection auto-copies (optional, off by default)
- **Tab rename** — double-click any tab label to rename inline
- **Cursor style** — Block / Bar / Underline, with optional blink; changes apply to live terminals instantly
- **Per-session terminal config** — override font size, scrollback, line height, cursor style per session profile
- **Broadcast input** — Radio button in toolbar sends keystrokes to all active sessions simultaneously
- **Session recording** — record a session to a `.log` file; replay it later with the built-in recording player (step controls, click-to-seek progress bar)

</details>

<details>
<summary><strong>Encrypted Vault & Credential Storage</strong></summary>

- All credentials encrypted with **AES-256-GCM** (random 96-bit nonce per entry)
- Master password → **Argon2id** (m=64 MB, t=3, p=4) → key never stored on disk
- Derived keys zeroed from RAM after use (`zeroize` crate)
- Wrong password returns a generic "Unlock failed" — no timing or content oracle
- **Vault auto-locks** after configurable inactivity timeout (default 5 min)
- SSH key passphrases stored in the **OS keychain** (macOS Keychain, Windows DPAPI, Linux libsecret)
- Master password never written to disk, logs, crash reports, or clipboard

</details>

<details>
<summary><strong>Port Forwarding & Tunnels</strong></summary>

- **Local forwarding** (`-L`) — forward a local port to a remote host:port through the SSH tunnel
- **Remote forwarding** (`-R`) — expose a local service on a port of the remote server
- **Dynamic SOCKS5** (`-D`) — full SOCKS5 proxy through the SSH connection
- **Tunnel auto-start** — configure tunnels to open automatically when a session connects
- Tunnel panel in the sidebar shows live status for all active forwards

</details>

<details>
<summary><strong>SFTP File Browser</strong></summary>

- Full remote file browser with folder navigation, editable path bar
- **Drag-and-drop upload** — drop files from Finder/Explorer into the panel
- **Real-time upload progress** — streams in 256 KB chunks with a live byte counter
- **Transfer queue** — multiple concurrent transfers with individual progress bars
- **File search** — recursive glob search across the remote filesystem
- **File preview** — inline UTF-8 or hex dump for text/binary files
- Multiple tabs to the same server share one SFTP connection

</details>

<details>
<summary><strong>Live Session Sharing</strong></summary>

Share a read-only live view of your terminal with another person — useful for pair debugging or walkthroughs.

Three sharing modes:

| Mode | How | Who can view |
|---|---|---|
| **Local** | localhost WebSocket | Same machine only |
| **LAN** | TLS (self-signed cert) | Anyone on your local network |
| **Internet** | Relay server (Fly.io / Raspberry Pi) | Anyone with the URL |

- 256-bit random token in the viewer URL — brute-force infeasible
- Token comparison uses constant-time equality — no timing oracle
- **Pause output** before typing passwords (notice shown in the share modal)
- Viewer count badge in the toolbar; pause / resume / stop controls

</details>

<details>
<summary><strong>Macro Scripting</strong></summary>

Automate repetitive workflows with a built-in scripting engine. No external dependencies — custom DSL interpreted in Rust.

```
# Example: deploy script
sendln cd /opt/myapp
expect $ 3000
sendln git pull
expect $ 10000
sendln sudo systemctl restart myapp
sleep 2000
emit Deployment complete
```

**DSL keywords:**

| Keyword | Description |
|---|---|
| `sendln TEXT` | Send TEXT followed by a newline (also the default for bare lines) |
| `send TEXT` | Send TEXT without a newline (for interactive prompts) |
| `sleep N` | Pause N milliseconds |
| `expect PAT MS` | Wait up to MS ms for substring PAT to appear in PTY output |
| `emit MESSAGE` | Send a progress message to the frontend progress bar |
| `# comment` | Ignored |

Live progress bar with step label and cancel button shown while a script runs.

</details>

<details>
<summary><strong>SSH Key Manager</strong></summary>

- Generate RSA-4096, ED25519, and ECDSA P-256/384 key pairs from within the app
- Import existing keys from disk
- Passphrases stored in the OS keychain — never in the vault or on disk in plaintext
- Keys shared across sessions that reference the same file path

</details>

<details>
<summary><strong>Session Organisation</strong></summary>

- **Groups / folders** — drag sessions into named groups; collapse/expand in sidebar
- **Tags** — assign multiple tags per session; filter by tag with a single click
- **Pin sessions** — pinned sessions appear at the top of the list
- **Recent sessions** — last 10 opened sessions shown in a quick-access section
- **Drag-drop reorder** — rearrange sessions and groups by dragging
- **Fuzzy search** — ranked search with word-boundary and consecutive-character scoring; matched characters highlighted
- **SSH config import** — import sessions from `~/.ssh/config` in one click
- **Export / import JSON** — back up or share session profiles (credentials excluded)

</details>

<details>
<summary><strong>Audit Log</strong></summary>

Append-only JSON-lines log at `<appdata>/audit.log` recording every connect, disconnect, and auth failure with timestamp, host, port, username, and event type.

- View in **Settings → Audit Log** with colour-coded event types
- Configurable auto-retention (7 / 30 / 90 / 365 days or Forever, default 30 days)
- Export or clear from the UI

</details>

<details>
<summary><strong>Live Stats Bar</strong></summary>

Enable in **Settings → Terminal → Session Stats Bar** to show a live status bar at the bottom of each terminal tab (Linux remote servers only).

Metrics updated every 3 seconds:

| Metric | Source |
|---|---|
| Load average (1 / 5 / 15 min) | `/proc/loadavg` |
| RAM used / total | `/proc/meminfo` |
| Network ↓ / ↑ bandwidth | `/proc/net/dev` |
| CPU utilisation % | `/proc/stat` (delta) |
| Disk used / total (root) | `df -k /` |
| Process / thread count | `/proc/loadavg` field 4 |
| Uptime | `/proc/uptime` |

All metrics collected in a single exec round-trip per poll. Background tabs do not poll.

</details>

---

## Security

| Property | Implementation |
|---|---|
| Vault encryption | AES-256-GCM, random 96-bit nonce per entry |
| Key derivation | Argon2id (m=64 MB, t=3, p=4, 32-byte random salt) |
| Key material in RAM | Zeroed after use via `zeroize` crate |
| Passphrase storage | OS keychain (macOS Keychain / Windows DPAPI / Linux libsecret) |
| Host key verification | TOFU + hard reject on changed keys (no "accept anyway") |
| Session sharing tokens | 256-bit random, constant-time comparison |
| Update signatures | Ed25519 (verified before any update is applied) |
| Logging policy | Passwords, keys, and decrypted vault data are never logged |
| Database permissions | `0600` on Unix |

The SSH cipher policy only allows modern algorithms:
- **KEX:** `curve25519-sha256`, `ecdh-sha2-nistp521/384`, `diffie-hellman-group18/16-sha512`
- **Ciphers:** `chacha20-poly1305@openssh.com`, `aes256/128-gcm@openssh.com`, `aes256-ctr`
- **Banned:** all SHA-1 KEX, arcfour, blowfish, 3DES, HMAC-MD5/SHA-1

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Shift+T` | Quick Connect dialog |
| `Ctrl+Shift+W` | Close active tab |
| `Ctrl+Tab` | Next tab |
| `Ctrl+Shift+Tab` | Previous tab |
| `Ctrl+,` | Open Settings |
| `Ctrl+B` | Toggle sidebar |
| `Ctrl+F` | Find in terminal |
| `Ctrl+=` / `Ctrl+-` / `Ctrl+0` | Font size zoom in / out / reset |
| `F1` | Keyboard shortcuts reference |

---

## Data Storage

All persistent data is stored in the OS app-data directory:

| Platform | Path |
|---|---|
| macOS | `~/Library/Application Support/com.securesh.app/` |
| Windows | `%APPDATA%\com.securesh.app\` |
| Linux | `~/.local/share/com.securesh.app/` |

Contents: `sessions.db` (SQLite — sessions, vault, groups, keys) and `audit.log`.

Upgrading to a new version preserves all sessions, vault, settings, and keys — no migration needed.

---

## Release History

| Version | Date | Highlights |
|---|---|---|
| [v0.3.0](https://github.com/anindyadc/secureSH-releases/releases/tag/v0.3.0) | 2026-06-25 | CPU/disk/process stats bar, session keepalive, one-click update, recording step controls, macOS key repeat fix, Linux binaries |
| [v0.2.0](https://github.com/anindyadc/secureSH-releases/releases/tag/v0.2.0) | 2026-06-01 | Live session sharing (local/LAN/relay), collapsible sidebar, graceful tab close |
| [v0.1.0](https://github.com/anindyadc/secureSH-releases/releases/tag/v0.1.0) | — | Initial release: SSH, vault, SFTP, tunnels, macros, accessibility |

---

<div align="center">
<sub>secureSH is built with <a href="https://tauri.app">Tauri v2</a>, <a href="https://github.com/warp-tech/russh">russh</a>, and <a href="https://xtermjs.org">xterm.js</a></sub>
</div>
