# Axym

**An autonomous engineering system: give it a goal in plain English and it plans, writes, reviews, secures and tests the code — with deterministic safeguards and an audit trail for every decision.**

No account, no API keys to get started, no build tools. Download a build below.

[![Latest release](https://img.shields.io/github/v/release/irshadpc/axym-community?label=latest%20release&color=blue)](../../releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](../../blob/main/LICENSE)

Axym ships as two products that share one runtime:

| | **Axym TUI** — terminal agent | **Axym Desktop** — full app |
|---|---|---|
| What it is | One standalone executable: a session UI, permission gate and headless mode on top of the AI coding CLI you already use | The whole pipeline (plan → design → code → review → security → QA) with mission control, approval gates and in-app updates |
| Best for | Driving your own agent CLI (`claude`, `opencode`, `codex`, `gemini`, `cursor-agent`, `copilot`, `agy`) from a fast terminal UI | Running autonomous multi-stage engineering work end-to-end, with diffs to approve and tamper-evident run history |
| Version | [`0.19.0`](../../releases/tag/tui-v0.19.0) | [`12.16.0`](../../releases/tag/desktop-v12.16.0) |

## Downloads

| Platform | Axym TUI 0.19.0 | Axym Desktop 12.16.0 |
|---|---|---|
| macOS — Apple Silicon & Intel | [axym-0.19.0-macos.tar.gz](https://github.com/irshadpc/axym-community/releases/download/tui-v0.19.0/axym-0.19.0-macos.tar.gz) | [Axym_12.16.0_universal.dmg](https://github.com/irshadpc/axym-community/releases/download/desktop-v12.16.0/Axym_12.16.0_universal.dmg) |
| Linux x86_64 | [axym-0.19.0-linux-x86_64.tar.gz](https://github.com/irshadpc/axym-community/releases/download/tui-v0.19.0/axym-0.19.0-linux-x86_64.tar.gz) | [axym-12.16.0-linux-x86_64.tar.gz](https://github.com/irshadpc/axym-community/releases/download/desktop-v12.16.0/axym-12.16.0-linux-x86_64.tar.gz) |
| Windows x86_64 | [axym-0.19.0-windows-x86_64.zip](https://github.com/irshadpc/axym-community/releases/download/tui-v0.19.0/axym-0.19.0-windows-x86_64.zip) | [axym-12.16.0-windows-x86_64.zip](https://github.com/irshadpc/axym-community/releases/download/desktop-v12.16.0/axym-12.16.0-windows-x86_64.zip) |

Every release also carries `SHA256SUMS.txt`; TUI releases add a machine-readable `release-manifest.json` (version, source commit, per-file hashes). See [Verify a download](#verify-a-download).

## Install

### TUI — macOS & Linux

The macOS archive contains `axym-macos-arm64` (Apple Silicon) and `axym-macos-x64` (Intel); the Linux archive contains `axym-linux-x86_64`.

```sh
tar -xzf axym-0.19.0-macos.tar.gz     # or axym-0.19.0-linux-x86_64.tar.gz
./axym-macos-arm64 --version          # Intel Mac: ./axym-macos-x64
./axym-macos-arm64 --install          # optional: copies to ~/.local/bin + man page
axym                                  # start a session in the current project
```

If macOS Gatekeeper blocks the first launch (browser downloads only), run once:
`xattr -d com.apple.quarantine axym-macos-arm64`

### TUI — Windows

Extract the zip and run `axym-windows-x86_64.exe` from PowerShell or `cmd`. `axym-windows-x86_64.exe --install` copies it to `%LOCALAPPDATA%\Axym\bin\axym.exe` and tells you if that folder isn't on your `PATH` yet.

### Desktop — macOS (one line)

```sh
curl -fsSL https://github.com/irshadpc/axym-community/releases/latest/download/install-macos.sh | bash
```

Downloads the universal `.dmg`, installs to `/Applications`, and clears the Gatekeeper quarantine flag — no Apple developer certificate needed. Prefer manual? Open the `.dmg` and drag Axym to Applications; if macOS then says the app "is damaged", run `sudo xattr -dr com.apple.quarantine /Applications/Axym.app` once (Sequoia/Tahoe: also **System Settings → Privacy & Security → Open Anyway**).

### Desktop — Linux

```sh
tar -xzf axym-12.16.0-linux-x86_64.tar.gz
./axym-linux/axym                 # or ./axym-linux/install.sh
```

Desktop Ubuntu already has the required system-tray library; minimal/server images need `sudo apt install libayatana-appindicator3-1` (Fedora: `sudo dnf install libayatana-appindicator-gtk3`). Looking for `.AppImage`/`.deb`/`.rpm` installers? The newest are on [desktop-v12.8.0](../../releases/tag/desktop-v12.8.0) (AppImage needs `sudo apt install libfuse2`).

### Desktop — Windows

Extract the zip and run `axym.exe`. It needs the WebView2 runtime, already included on Windows 10 (1803+) and Windows 11.

## First steps

**TUI** — needs at least one agent CLI on your `PATH` (`claude`, `opencode`, `codex`, `gemini`, `cursor-agent`, `copilot`, or `agy`):

```sh
axym                                      # interactive session in this project
axym --provider claude --mode plan        # pick a provider; read-only planning mode
axym -p "summarize uncommitted changes"   # headless — for scripts and CI
axym web --host 0.0.0.0 --pin 4821        # drive it from your phone: scan the QR code
axym --doctor                             # check detected providers and setup
axym --man                                # the full manual, in your pager
```

Inside the TUI, type `/` to browse every command with tab completion. The ones worth knowing first:

- `/review`, `/qa` — automated quality and test-coverage passes
- `/security` (alias `/audit`) — evidence-grounded vulnerability scanning
- `/pipeline` — watch a run stage by stage
- `/po` — turn a fuzzy request into a scoped, evidence-backed decision
- `/health`, `/intel`, `/settings` — runtime health, intelligence view, settings
- `/update` — self-update to the newest release (verifies the download first)
- `/copy`, `/vim`, `ctrl+g` — clipboard, Vim keybindings, external `$EDITOR`

**Desktop** — first launch walks you through provider discovery (Axym finds installed AI CLIs automatically; no keys needed to start). Then:

1. Type a goal ("add rate limiting to the API and tests for it").
2. Watch the pipeline DAG: Planner → Designer → Coder → Review → Security → QA.
3. Approve or reject at the gates — you inspect file diffs and tool-execution requests before anything touches your working tree.
4. Every finished run records its own outcome score (accepted? useful? defects prevented?), and QA only passes when real test and build results back it up.

Configure a project by dropping [`.axym.yaml`](https://github.com/irshadpc/axym/blob/main/.axym.yaml.example) at its root (tech stack, style guide, forbidden libs, secret names), and register private agent CLIs in `~/.axym/providers.json`.

## Features

**One system, many providers.** Drive Claude Code, Codex, OpenCode, Gemini, Antigravity and more with automatic failover, unified context caching and session portability across the TUI, Desktop and web surfaces.

**Deterministic safeguards.** Tool-admission policy flags destructive operations and asks first. Vulnerability claims must be backed by captured observations — fabricated AI output is gated. Every run event is hash-chained into the local SQLite store, so run history is tamper-evident and replayable.

**A real pipeline, not a chat.** Goals flow through specialized stages — plan, design, code, review, security, QA — each with its own gates, evidence and scoring. Runs execute in isolated worktrees; parallel runs don't stomp on each other.

**Skills.** Versioned, sandboxed extensions hot-reload into both apps. Bundled today: **product-thinking** (fuzzy request → scoped decision), **engineering intelligence** (investigation/verification discipline), **reverse-engineering** (new — analyze shipped binaries, bundles, APKs and websites with plain CLI tools), **sca-fix** (remediate a Sonatype/SCA report for Java/Maven), and **break-it QA** (adversarial personas against existing features). Toggle them with `/skills` in the TUI or the Skill Dashboard in Desktop.

**Mission control.** Live pipeline DAG, run history with outcome scores, approval gates with real diffs, council decision records (full candidate sets, anonymized ballots, Borda consensus), and a self-evaluation harness that keeps comparing the system's own architecture against alternatives.

**Local-first.** Everything runs on your hardware — Rust/Tauri runtime, embedded SQLite event store, crash-journaled side effects with exactly-once recovery. No telemetry leaves the machine.

## What's new

**TUI 0.19.0** · [full notes](../../releases/tag/tui-v0.19.0)

- The tasks chip now tracks the agent's **real todo state** (live `TodoWrite` writes) instead of counting numbered lines in prose — the counter used to freeze at "0/25".
- Readable bottom bar: M-scale context numbers, separated chips, cleaner context line on narrow terminals.
- Built-in **reverse-engineering skill** ships inside the binary.
- Surfaces the new governed graph-execution mode when the runtime exposes it.

**Desktop 12.16.0** · [full notes](../../releases/tag/desktop-v12.16.0)

- **Dynamic graph execution mode:** governed task-grain parallelism — admission control (role-counted node caps, write-set conflict groups, exact cycle traces), verifier-required provider nodes, and economic-governor cost posture.
- **Parallel runs are truly isolated:** finishing one run no longer stops the agents and builds of runs still going; switching runs can't show you the previous run's data.
- **Honest scoring:** unmeasured quality dimensions are never scored as 100 anymore; every run's self-score is backed by real test and build results.
- **Security hardening:** native Electron posture checks for target apps, a fixed reverse-engineering-adapter that now parses real tool output, and injection-defensive reliability fixes.
- **Council decision records** now preserve full member candidates, anonymized ballots and Borda consensus scores; the architecture self-evaluation harness gained Arms A/B/C comparison.
- **Mobile connect:** authenticated test submissions, workspace-switcher projection, event-log resync on future cursors.
- **Priority task steering** with durable fair-share ranking, plus a serialized clock fix in the execution graph.
- UI: sidebar expands at 640 px instead of 768 px (small laptops), darker rail.

Both products now ship the same built-in skills set, and the release pipeline itself is verified by seeded-fault injection — every release gate must detect 11 deliberately planted defects before it can pass.

Full history: [all releases](../../releases).

## Requirements & updates

| | Needs | Admin rights? | How to update |
|---|---|---|---|
| TUI | One agent CLI on `PATH` | No | `/update` or `axym --update` — verifies the download, then installs |
| Desktop macOS | — | No | Download the new `.dmg` (or re-run the one-line installer) |
| Desktop Linux / Windows | System-tray lib (Linux), WebView2 (Windows) | No | Download the new archive from [Releases](../../releases) |

The first launch on any OS may show an "unknown publisher" warning you can dismiss (builds are ad-hoc signed, not notarized).

## Verify a download

Put `SHA256SUMS.txt` (from the same release) in the folder with your download and run:

```sh
shasum -a 256 -c --ignore-missing SHA256SUMS.txt
```

TUI releases also include `release-manifest.json` — version, source commit and per-file hashes.

## Help

- TUI: run `/doctor` and include its output in any bug report.
- [Open an issue](../../issues) with your OS, the file you downloaded, and what happened.
- The full engineering documentation portal (74 pages, every engine and invariant) is in the [source repo](https://github.com/irshadpc/axym).
