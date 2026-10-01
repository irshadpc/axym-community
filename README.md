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
| Axym TUI | `0.11.8` | [tui-v0.11.8](../../releases/tag/tui-v0.11.8) · [all TUI releases](../../releases?q=tui-v) |
| Axym Desktop | `12.8.0` | [desktop-v12.8.0](../../releases/tag/desktop-v12.8.0) · [all desktop releases](../../releases?q=desktop-v) |

> **New in TUI 0.7–0.11** — real terminal scrollback (full history, sticky
> input); TodoWrite renders as a live checklist; LLM council (`/council`)
> gets parallel multi-provider review with merged output; ACP on by
> default; uniform approval gate with per-tool always-allow that persists;
> per-provider image gating; git permissions + `/pull`; stall watchdog and
> long-session OOM fix; model selection forwarded to ACP providers.
> Still here from 0.6.0: `axym-tui web` phone pairing over QR (live
> transcript, approvals, optional Cloudflare tunnel), wake-lock unattended
> runs with ntfy push, and quota-aware provider failover with cooldowns
> and circuit breaker.
>
> **New in Desktop 12.8.0** — agent provider layer (Copilot + Cursor join
> the registry, ACP transport, `/doctor` diagnostics), TUI parity
> (searchable model picker, task checklist, status bar, mode switcher,
> council toggle, tool timeline, transcript export), multimodal input, and
> quota-aware failover that rides out session-limit walls instead of
> retrying into them.

## Axym TUI — install

Pick the file for your platform (`<v>` = version, e.g. `0.11.8`):

| File | Platform |
|---|---|
| `axym-tui-<v>-macos.tar.gz` | macOS — contains both Apple Silicon (`-arm64`) and Intel (`-x64`) binaries |
| `axym-tui-<v>-linux-x86_64.tar.gz` | Linux x86_64 |
| `axym-tui-<v>-windows-x86_64.zip` | Windows x86_64 |

**macOS / Linux**

```sh
tar -xzf axym-tui-<v>-macos.tar.gz        # or the linux tarball
./axym-tui-macos-arm64 --version          # Apple Silicon (use -x64 on Intel, no suffix juggling on Linux)
./axym-tui-macos-arm64 --install          # optional: copy to ~/.local/bin + install the man page
```

**Windows** — extract the zip and run `axym-tui-windows-x86_64.exe`
from PowerShell or `cmd`.

### TUI prerequisite: an agent CLI

The TUI is a driver and UI for agent CLIs — it does not ship a model.
Install **at least one** of these first and make sure it's on your `PATH`
(check with e.g. `which claude`):

- `claude` (Claude Code) · `opencode` · `codex` · `gemini`
- `cursor-agent` (Cursor) · `copilot` (GitHub Copilot CLI)

With none installed the TUI starts but cannot act. Inside the TUI,
`/doctor` diagnoses your environment (found providers, auth state, PATH).

### TUI first run

```sh
axym-tui                                # interactive session in the current project
axym-tui --provider claude --mode plan  # explicit provider + read-only mode
axym-tui -p "summarize uncommitted changes"   # headless, for scripts and CI
axym-tui web --host 0.0.0.0 --pin 4821  # phone bridge: scan the QR it prints
```

> **macOS Gatekeeper:** the binaries are unsigned, so macOS may block the
> first launch. Run `xattr -d com.apple.quarantine axym-tui-macos-arm64`
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

**macOS** — open the `.dmg` and drag Axym to Applications, or:

```sh
curl -fsSL https://github.com/irshadpc/axym-community/releases/latest/download/install-macos.sh | bash
```

> Builds are ad-hoc signed, not notarized. If macOS says the app
> “is damaged”, clear the quarantine flag once:
> `sudo xattr -dr com.apple.quarantine /Applications/Axym.app`

**Linux**

```sh
chmod +x Axym_<v>_amd64.AppImage && ./Axym_<v>_amd64.AppImage   # no install needed
sudo apt install ./Axym_<v>_amd64.deb                          # or: sudo dnf install ./Axym-<v>-1.x86_64.rpm
```

> The AppImage needs FUSE to run. On Ubuntu 22.04+ install it first:
> `sudo apt install libfuse2`.

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
- **TUI:** `/update` inside the TUI (or `axym-tui --update`) downloads the
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
