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
| Axym TUI | `0.14.3` | [tui-v0.14.3](../../releases/tag/tui-v0.14.3) · [all TUI releases](../../releases?q=tui-v) |
| Axym Desktop | `12.11.0` | [desktop-v12.11.0](../../releases/tag/desktop-v12.11.0) · [all desktop releases](../../releases?q=desktop-v) |

> **New in TUI 0.14.3** — Google's Antigravity CLI joins the provider roster:
> install `agy`, authenticate once, and drive it with
> `axym --provider antigravity` (or `--provider agy`) — model listing
> (`axym models antigravity`), `--conversation` resume, and per-mode
> permissions (`--mode plan`, yolo via `--dangerously-skip-permissions`)
> all work. Also: OpenCode 2.x compatibility (model selection and run flags
> follow the 2.x CLI), and atomic writes for the allowlist, init file, and
> saved workflows, so a crash can never leave a half-written config.
>
> **New in TUI 0.14.2** — the binaries are renamed from `axym-tui-*` to
> `axym-*` (`axym-macos-arm64`, `axym-linux-x86_64`,
> `axym-windows-x86_64.exe`), and the Windows standalone executable
> actually runs now — previously every invocation silently did nothing
> because the entry-point check never matched under `bun build --compile`
> on Windows.
>
> **New in TUI 0.14.1** — `--install` was completely broken on Windows (and
> on any standalone binary): it always tried to wrap a `dist/axym-tui.mjs`
> bundle that doesn't exist next to a compiled standalone executable, so it
> failed outright with "TUI bundle not found." Fixed to copy the running
> binary directly when no bundle is present — Windows installs now land at
> `%LOCALAPPDATA%\Axym\bin\axym-tui.exe` with a correct `PATH` check.
>
> **New in TUI 0.14.0** — custom workflows: `/workflow new <name>` opens
> your editor on a plain-text template (steps separated by a lone `---`
> line, optional `{goal}` placeholder); `/workflow save <name>` turns a
> conversation you already had into a reusable one; `/workflow run <name>
> [goal]` replays it on any provider, one step per turn, so later steps see
> earlier real output as context. Three starters ship pre-seeded
> (`engineer-flow`, `product-discovery`, `security-review`). Also: a fix for
> axym-runtime silently invoking codex instead of Claude, live per-stage
> axym-runtime progress, provider failover on untagged ACP stops, and a
> fix for `/compact`/`/new`/`/clear` silently clearing the visible
> transcript.
>
> **Earlier in TUI 0.13.0** — Claude Code-grade input editing plus a
> process-lifetime correctness series. Bracketed paste is never
> re-interpreted as keystrokes (long pastes collapse to `[Pasted text #n]`
> chips, expanded on submit); image attachments ride along as `[Image #n]`
> chips from drag-drop, clipboard, or a pasted file path (attachability is
> sniffed by magic bytes, not extension); `/copy` grabs the last response
> over OSC 52 so it works over SSH and tmux; ctrl+g / ctrl+x ctrl+e compose
> the prompt in `$VISUAL`/`$EDITOR`; `/vim` gives a persistent vim editing
> mode. Tool rows now render as colored pill badges (`BASH [git log --oneline]`)
> whose background turns red once a call fails, with descriptive labels for
> every backend. The C-01…C-10 fixes mean nothing is auto-approved in your
> name, streaming text is never reordered or dropped, finished turns hit
> disk immediately, and every child process, wake lock, and public tunnel
> the TUI starts dies with it — a cancelled background command stops its
> whole process tree. Releases are now gated on a real-pty end-to-end run of
> the shipped binary before they publish.
>
> **New in TUI 0.12.1** — a `--yolo` session now says so: a sticky flag on the
> session record means a resumed or exported transcript still shows that every
> tool-call and git-write approval gate was disabled for part of the run
> (`**--yolo was active**` in the transcript export). Headless runs are
> cancellable too — an interrupted `axym-tui -p` run exits `130` with
> `✗ interrupted` instead of hanging, and never reports success for a
> half-finished turn.
>
> **New in TUI 0.12.0** — `--yolo` (alias `--dangerously-skip-permissions`)
> turns off every approval prompt for the session: the host tool-call gate,
> git writes, and the provider's own permission prompts. It implies
> `--mode auto` and **has no undo** — use it in disposable or fully-trusted
> workspaces. Background workers (`/bg:tests`) now stream live output:
> `/status` shows the command, elapsed time, and tail; the status bar shows a
> `⏳ bg:` timer.
>
> **New in Desktop 12.11.0** — shipped alongside TUI 0.12.0 (the TUI has since
> moved to 0.13.0); this cycle's user-facing features land in the TUI, plus a
> scroll-UX rework of the legacy Python dashboard (sticky headers,
> scroll-preserving refresh, back-to-top).
>
> **Earlier in TUI 0.7–0.11** — real terminal scrollback (full history, sticky
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
> **Earlier in Desktop** — 12.10.0 Engineering Advisor (adaptive health model,
> priorities, Mission Control panel); 12.8.0 agent provider layer (Copilot +
> Cursor in the registry, ACP transport, `/doctor` diagnostics), TUI parity,
> multimodal input, and quota-aware failover that rides out session-limit
> walls instead of retrying into them.

## Axym TUI — install

Pick the file for your platform (`<v>` = version, e.g. `0.14.3`):

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
curl -fL -o axym.tar.gz https://github.com/irshadpc/axym-community/releases/download/tui-v0.14.3/axym-0.14.3-macos.tar.gz
tar -xzf axym.tar.gz && ./axym-macos-arm64 --version
```

> TUI links are pinned to the `tui-v0.14.3` tag rather than
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
`PATH` yet (requires 0.14.1+ — earlier versions failed this step outright).

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

> **12.11.0 Linux packaging:** this release was built on macOS, so it ships the
> macOS universal `.dmg`, the Linux **portable tarball**, and the Windows zip.
> The Linux `.deb` / `.AppImage` / `.rpm` installers are produced on a Linux
> build host — the newest of those on this page are `12.8.0`. Grab the tarball
> below if you want 12.11.0 on Linux today; it needs no installer.

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
curl -fL -o axym-linux.tar.gz https://github.com/irshadpc/axym-community/releases/download/desktop-v12.11.0/axym-12.11.0-linux-x86_64.tar.gz
tar -xzf axym-linux.tar.gz
./axym-linux/axym                  # or ./axym-linux/install.sh

# older installers (12.8.0), if you prefer an installed package
chmod +x Axym_12.8.0_amd64.AppImage && ./Axym_12.8.0_amd64.AppImage
sudo apt install ./Axym_12.8.0_amd64.deb        # or: sudo dnf install ./Axym-12.8.0-1.x86_64.rpm
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
