# Axym — downloads for the TUI and desktop app

Axym is a terminal-first AI engineering assistant. This repo is the public
front door: grab the latest builds below, no account or build tools needed.

- **Axym TUI** — a standalone terminal agent. Single-file executables, no
  Node.js or runtime to install. It drives the AI coding CLIs you already
  use and gives them a shared session UI, permissions gate, and headless
  mode for scripts and CI.
- **Axym Desktop** — the full desktop app (pipeline execution, mission
  control, security review, and an embedded terminal) with automatic
  in-app updates.

> Looking for the source or want to report a bug? You're in the right
> place — use [Issues](../../issues) for bugs, crashes, and feature
> requests. Everything downloadable lives under
> [Releases](../../releases).

## Download

| Product | Current version | Get it |
|---|---|---|
| Axym TUI | `0.15.0` | [tui-v0.15.0](../../releases/tag/tui-v0.15.0) · [all TUI releases](../../releases?q=tui-v) |
| Axym Desktop | `12.12.0` | [desktop-v12.12.0](../../releases/tag/desktop-v12.12.0) · [all desktop releases](../../releases?q=desktop-v) |

## What's new

**TUI 0.15.0 / Desktop 12.12.0 — fail-closed reliability.** No new
features; Axym now reports the truth about every turn and repairs provider
integrations that upstream CLI changes had broken:

- A provider that crashes, is killed, or stops mid-stream is reported as an
  error, never as a successful (empty) answer. A success followed by an
  abnormal exit keeps the answer and shows a warning.
- No more lost or hung turns: the first Claude turn after a model change,
  runtime events that race a submit, and a crashed ACP agent all recover.
- **codex** auto mode works again (codex removed `--full-auto`), and resumed
  codex turns work in the desktop app. **copilot** failure reasons such as
  an exhausted quota are shown and trigger failover.
- Headless `axym -p` prints the provider's failure reason instead of exiting
  silently (`--format json` adds `outcome.error` / `outcome.warning`).
- Desktop runtime: a run that was supposed to change code but wrote nothing
  is no longer accepted as complete.

Full changelogs for every prior version ship with
[each release](../../releases) — this page only covers the current one.

## Axym TUI — install

Pick the file for your platform (`<v>` = version, e.g. `0.15.0`):

| File | Platform |
|---|---|
| `axym-<v>-macos.tar.gz` | macOS — contains both Apple Silicon (`axym-macos-arm64`) and Intel (`axym-macos-x64`) binaries |
| `axym-<v>-linux-x86_64.tar.gz` | Linux x86_64 (`axym-linux-x86_64`) |
| `axym-<v>-windows-x86_64.zip` | Windows x86_64 (`axym-windows-x86_64.exe`) |

**macOS / Linux**

```sh
tar -xzf axym-<v>-macos.tar.gz        # or the linux tarball
./axym-macos-arm64 --version          # Apple Silicon (use -x64 on Intel, no suffix juggling on Linux)
./axym-macos-arm64 --install          # optional: copy to ~/.local/bin + install the man page
```

Latest direct links:

```sh
# macOS (Apple Silicon + Intel)
curl -fL -o axym.tar.gz https://github.com/irshadpc/axym-community/releases/download/tui-v0.15.0/axym-0.15.0-macos.tar.gz
tar -xzf axym.tar.gz && ./axym-macos-arm64 --version
```

> TUI links are pinned to the `tui-v0.15.0` tag rather than
> `releases/latest/download/`. GitHub allows only one *latest* release per
> repository and that slot belongs to the desktop app — which also needs
> `/latest/` to resolve `install-macos.sh`.

**Verify what you downloaded** — every TUI release ships `SHA256SUMS.txt`
(check it with `shasum -a 256 -c SHA256SUMS.txt`) plus a machine-readable
`release-manifest.json` carrying the product, version, source commit, and
per-artifact hashes.

**Windows** — extract the zip and run `axym-windows-x86_64.exe` from
PowerShell or `cmd`. `axym-windows-x86_64.exe --install` copies it to
`%LOCALAPPDATA%\Axym\bin\axym.exe` and tells you if that's not on your
`PATH` yet.

### TUI prerequisite: an agent CLI

The TUI is a driver and UI for agent CLIs — it does not ship a model.
Install **at least one** of these first and make sure it's on your `PATH`
(check with e.g. `which claude`):

- `claude` (Claude Code) · `opencode` · `codex` · `gemini`
- `cursor-agent` (Cursor) · `copilot` (GitHub Copilot CLI) · `agy` (Google Antigravity)

With none installed the TUI starts but cannot act. Inside the TUI,
`/doctor` diagnoses your environment (found providers, auth state, PATH).

### TUI first run

```sh
axym                                # interactive session in the current project
axym --provider claude --mode plan  # explicit provider + read-only mode
axym -p "summarize uncommitted changes"   # headless, for scripts and CI
axym web --host 0.0.0.0 --pin 4821  # phone bridge: scan the QR it prints
```

> **macOS Gatekeeper:** the binaries are unsigned, so macOS may block the
> first launch. Run `xattr -d com.apple.quarantine axym-macos-arm64`
> (or `-x64`) once and you're set.

## Axym Desktop — install

| File | Platform |
|---|---|
| `Axym_<v>_universal.dmg` | macOS — Apple Silicon + Intel in one image |
| `Axym_<v>_amd64.AppImage` | Linux — runs anywhere, no install |
| `Axym_<v>_amd64.deb` | Linux — Debian / Ubuntu |
| `Axym-<v>-1.x86_64.rpm` | Linux — Fedora / RHEL |
| `axym-<v>-linux-x86_64.tar.gz` | Linux — portable, just the binary |
| `axym-<v>-windows-x86_64.zip` | Windows — portable (`axym.exe` + WebView2 loader) |

> **12.12.0 Linux packaging:** this release was built on macOS, so it ships the
> macOS universal `.dmg`, the Linux **portable tarball**, and the Windows zip.
> The Linux `.deb` / `.AppImage` / `.rpm` installers are produced on a Linux
> build host — the newest of those on this page are `12.8.0`. Grab the tarball
> below if you want 12.12.0 on Linux today; it needs no installer.

**macOS** — open the `.dmg` and drag Axym to Applications, or:

```sh
curl -fsSL https://github.com/irshadpc/axym-community/releases/latest/download/install-macos.sh | bash
```

> Builds are ad-hoc signed, not notarized. If macOS says the app
> “is damaged”, clear the quarantine flag once:
> `sudo xattr -dr com.apple.quarantine /Applications/Axym.app`

**Linux**

```sh
# portable tarball — works on any x86_64 distro, nothing to install
curl -fL -o axym-linux.tar.gz https://github.com/irshadpc/axym-community/releases/download/desktop-v12.12.0/axym-12.12.0-linux-x86_64.tar.gz
tar -xzf axym-linux.tar.gz
./axym-linux/axym                  # or ./axym-linux/install.sh

# older installers (12.8.0), if you prefer an installed package
chmod +x Axym_12.8.0_amd64.AppImage && ./Axym_12.8.0_amd64.AppImage
sudo apt install ./Axym_12.8.0_amd64.deb        # or: sudo dnf install ./Axym-12.8.0-1.x86_64.rpm
```

> The AppImage needs FUSE to run. On Ubuntu 22.04+ install it first:
> `sudo apt install libfuse2`.

> The desktop app needs the system-tray library and exits at launch without
> it. Desktop Ubuntu already has it; on minimal/server images install it
> first: `sudo apt install libayatana-appindicator3-1` (Fedora:
> `sudo dnf install libayatana-appindicator-gtk3`).

**Windows** — extract the zip and run `axym.exe`. It needs the
WebView2 runtime, which ships with Windows 10 (1803+) and Windows 11 —
nothing extra to install on a normal up-to-date system.

> First launch on any OS may show a user-dismissable “unknown publisher”
> warning (SmartScreen / Gatekeeper). No admin rights are needed — Axym
> installs per-user.

## Updating

- **Desktop (macOS, AppImage):** the app checks for updates itself and
  offers one-click install + relaunch. `.deb` / `.rpm` users can also just
  install the newer package over the old one.
- **TUI:** `/update` inside the TUI (or `axym --update`) downloads the
  latest release, verifies its SHA-256, and installs in place.

## Requirements at a glance

| | Needs | No admin? | Auto-updates? |
|---|---|---|---|
| TUI | One agent CLI on `PATH` (see above) | Yes | Yes — `/update` |
| Desktop macOS | — (quarantine clear on first run) | Yes | Yes, in-app |
| Desktop Linux AppImage | FUSE (`libfuse2` on Ubuntu 22.04+) | Yes | Yes, in-app |
| Desktop Linux deb/rpm | root for install | No | Via re-install |
| Desktop Windows | WebView2 (built into Win 10/11) | Yes (portable) | Via re-download |

## Help & feedback

- [Open an issue](../../issues) — bugs, crashes, installer problems.
  Include your OS, the exact file you downloaded, and (for the TUI) the
  output of `/doctor`.
- [Browse releases](../../releases) — changelogs ship with every version.
- Verify any download with the `SHA256SUMS.txt` published next to it:
  `shasum -a 256 -c SHA256SUMS.txt` (macOS/Linux).
