# Axym — downloads for the TUI and desktop app

Axym is an autonomous engineering assistant that takes a plain-English goal and produces reviewed, tested, and committed code. This repo is the public distribution gateway: grab the latest builds below, no account or build tools needed.

[![Releases](https://img.shields.io/github/v/release/irshadpc/axym-community?label=latest%20release&color=blue)](https://github.com/irshadpc/axym-community/releases)
[![Source Code](https://img.shields.io/badge/source-irshadpc%2Faxym-black.svg)](https://github.com/irshadpc/axym)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://github.com/irshadpc/axym/blob/main/LICENSE)

- **Axym TUI** — a standalone terminal agent. Single-file executables, no
  Node.js or runtime to install. It drives the AI coding CLIs you already
  use and gives them a shared session UI, permissions gate, and headless
  mode for scripts and CI.
- **Axym Desktop** — the full desktop app (pipeline execution, mission
  control, security review, and an embedded terminal) with automatic
  in-app updates.

## Download

| Product | Current version | Get it |
|---|---|---|
| Axym TUI | `0.18.0` | [tui-v0.18.0](../../releases/tag/tui-v0.18.0) · [all TUI releases](../../releases?q=tui-v) |
| Axym Desktop | `12.15.0` | [desktop-v12.15.0](../../releases/tag/desktop-v12.15.0) · [all desktop releases](../../releases?q=desktop-v) |

## What's new

**TUI 0.18.0 / Desktop 12.15.0 — Concurrent-run safety, self-evaluating runs & run inspection.**
- **Concurrent runs no longer kill each other:** previously, when one desktop run finished, the provider CLIs and build/test processes of every other run still in flight were terminated. Shutdown cleanup now happens once, when the app actually exits, and only for its own runs.
- **Every run evaluates itself:** each finished run, accepted or rejected, records six outcome scores (accepted, useful, effort reduced, quality, defects prevented, maintainability) in the engineering knowledge graph automatically, and they survive graph re-ingest.
- **Stricter QA acceptance:** a test or build check that claims to pass must be backed by the runner's real exit code (and, for tests, real output), or the run is not accepted. Projects without a test runner are judged exactly as before.
- **Execution protocol:** `axym task …` manifests with harness-run verification, evidence pinned to a git revision, and a fail-closed completion gate; durable execution state and evidence-gated handover.
- **Smarter runtime:** outcome-driven ambition planning, provider retries with backoff + jitter that fail closed on non-retryable errors, SQLite WAL auto-compaction with archival retention, and runtime log rotation.
- **TUI:** `/pipeline` to inspect a run's stages, `/health` `/intel` `/settings` `/handover` overlays, `/po` (built-in Product-Owner Intelligence), and Result Ledger task summaries. Fixes: no flicker or screen clear when scrolling up mid-task, PgUp reaches a streaming turn, the slash-completion popup scrolls with the selection, and stall notices name the actual command.
- **Desktop:** drag-and-drop into the app works again.

**TUI 0.17.3 / Desktop 12.14.3 — Security Audit Remediation & Unified Dialog Chrome.**
- **Security Audit Remediation (F-01..F-13):** All 13 findings resolved (3 Critical, 5 High, 3 Medium, 2 Low) across the desktop Rust engine and Python workers. Closes pre-admission unsandboxed code execution, Zip-Slip, path-traversal escapes, auth bypasses, and aligns autonomous CLI coders under strict admission and staging controls. Full report: `SECURITY_AUDIT.md`.
- **Security Workstreams & REA Investigation Adapter:** Runtime-discovered, opt-in REA investigation adapter (`AXYM_REA_BIN`) with scope-filtered untrusted evidence, persisted execution budgets (`max_steps` / `wall_clock_ms` / `max_remediation_attempts`), and operator-controlled cancellation.
- **Read-Only CLI Commands:** Safe `axym status [run-id] [--json]` and `axym diff [run-id] [--json]` commands opening SQLite in read-only mode without taking runtime locks, running safely alongside live daemons.
- **TUI Dialog Refactor & Determinism:** Unified dialog chrome (`ui/dialogs.tsx`) enforcing width budgets, consistent footers, and uniform selection across all overlays without Ink cursor corruption. Deterministic reaper test suite (693 passed / 0 failed).
- **Active Project Path Indicator:** Both the TUI status bar and Desktop GoalForm now prominently display the active project directory under the input textbox.
- **Fleet Observability & Environment Comparison:** Direct fleet monitoring via `axym fleet status` / `/fleet` and run-environment comparison via `/compare`.

**TUI 0.16.1 — reliability release, no new features.**
- Fixed a rendering bug where the Sessions, Resume session, Find-in-chat, and
  Jump-to-turn screens could garble the terminal display (wrapped borders,
  overwritten rows) when a session title or search result was long.
- Agent processes, the runtime connection, and public tunnels no longer get
  left running in the background after the TUI closes on Windows (already
  fixed on macOS/Linux) — verified via an automated test; a live Windows
  machine confirmation is still pending.
- Verified the "keep awake while working" behavior for real on Linux: the
  system no longer stays awake after Axym exits or crashes.

**TUI 0.16.0 / Desktop 12.13.0 — Universal Feature Excellence & Role-Adaptive Workspaces.**
- **Universal Feature Excellence:** All core system capabilities (execution, governance, role workspaces, mission control, continuity handoff) verified with executable measurements, regression suites, and 0 contract drift.
- **Role-Adaptive Activities:** Designer (`interface-design`) and QA (`qa-validation`) native activities added alongside Developer, Product Manager, Architect, and Lead workflows with role-specific intent lenses, live verification gates, and artifact generation.
- **WCAG 2.2 AA Contrast Compliance:** Contrast ratios across all interactive elements, status badges, and typography hardened to meet and exceed 4.5:1 / 3:1 WCAG AA standards in both dark and light modes.
- **TUI Command Segregation:** Dedicated `/help` categories and command segregation for faster, zero-confusion terminal navigation.
- **External Git Drift & Continuity:** Full bi-directional drift reconciliation preventing stale state overwrites when resuming work across IDEs, external CLI agents, or restarts.

**TUI 0.15.0 / Desktop 12.12.0 — fail-closed reliability.** No new features; Axym now reports the truth about every turn and repairs provider integrations that upstream CLI changes had broken:

- A provider that crashes, is killed, or stops mid‑stream is reported as an error, never as a successful (empty) answer. A success followed by an abnormal exit keeps the answer and shows a warning.
- No more lost or hung turns: the first Claude turn after a model change, runtime events that race a submit, and a crashed ACP agent all recover.
- `codex` auto mode works again (codex removed `--full-auto`), and resumed codex turns work in the desktop app. `copilot` failure reasons such as an exhausted quota are shown and trigger failover.
- Headless `axym -p` prints the provider's failure reason instead of exiting silently (`--format json` adds `outcome.error` / `outcome.warning`).
- Desktop runtime: a run that was supposed to change code but wrote nothing is no longer accepted as complete.

**TUI 0.14.4 / Desktop 12.11.2 — Antigravity integration.** Google's Antigravity CLI (`agy`) joins the provider roster in both products:

- `axym --provider antigravity` in the TUI, native pipeline dispatch in the desktop. `agy models` inventory, `--conversation` resume, and stream‑json token usage all work.
- Desktop 12.11.2 also folds in adaptive‑router hardening (atomic exploration‑slot reservation, daily‑cap enforcement).
- Binaries are named `axym-*` since 0.14.2.
- **Skill Integration:** New Antigravity skill system added (see the **Skill Integration (Antigravity)** section above).

Full changelogs for every prior version ship with [each release](../../releases) — this page only covers the current one.

### Skill Integration (Antigravity)

- **Antigravity Customization System** – automatic discovery of skills, rules, plugins, and MCP servers from the `agy/` directory.
- **YAML‑based skill manifests** (`skill.yaml`) with versioning, dependencies, and capability flags.
- **Live hot‑reload** of skills via the Effect Bus, no restart required.
- **Isolated sandbox v2** for security‑critical skills with per‑skill resource limits.
- **Skill Messaging Bus** enabling asynchronous communication between skills.
- **Skill Dashboard** in the desktop UI to list, enable/disable, view version and health.
- **Versioned skill packages** (`skill‑vX.Y.Z.tar.gz`) with auto‑upgrade checks and rollback.
- **Security Skill** offering policy‑check, secret‑scan, and compliance enforcement.
- **Skill Test Harness** (`axym skill test <skill>`) for isolated end‑to‑end testing.
- **CLI commands**: `axym skill list`, `axym skill enable`, `axym skill disable`, `axym skill upgrade`.

---

## 🧭 How to Explore Features & Details

Axym uses a **progressive disclosure** model so you can discover capabilities directly inside your workflow:

### 1. In-Product Discovery (Terminal TUI)
- **Built-in Manual (`axym --man`)**: View the complete user manual, command reference, and keybindings directly in your terminal pager.
- **Environment Doctor (`axym --doctor`)**: Run diagnostics on detected AI providers, binary paths, permissions, and tool admission policies.
- **Interactive Slash Commands (`/`)**: Type `/` to access workflows with instant tab-completion:
  - `/review` and `/qa` — run automated code quality and test-coverage passes.
  - `/security` (alias `/audit`) — run threat modeling and evidence-grounded vulnerability scanning.
  - `/priority` — inspect active AI provider resolution order and failover precedence.
  - `/workflow run <name>` — execute structured templates (`engineer-flow`, `product-discovery`, `security-review`).
  - `/skills` — list and toggle Antigravity skills.
  - `/vim` and `ctrl+g` — toggle persistent Vim keybindings or open prompt in your external `$EDITOR`.
  - `/copy` — copy the last response or turn to system clipboard over OSC 52.
  - `/pipeline` — inspect a desktop-runtime run stage by stage; `/health`, `/intel`, `/settings`, `/handover` open their overlays.
  - `/po` — Product-Owner Intelligence: turn a fuzzy request into an evidence-backed, scoped decision.
- **`axym web`**: Scan the terminal QR code from your phone to monitor runs, approve actions, or review diffs remotely.

### 2. In-Product Discovery (Desktop App)
- **Role-Adaptive Workspaces**: Switch intent lenses between Developer, Product Manager, Architect, Designer, and QA.
- **Interactive Pipeline DAG**: Watch goals progress through Planner → Designer → Coder → Review → Security → QA in real-time.
- **Audit & Approval Gates**: Inspect file diffs and tool execution requests before permitting changes to your working tree.
- **Offline Documentation**: Open `PROJECT.html` in your browser for the full 74-page offline engineering reference portal.

---

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
curl -fL -o axym.tar.gz https://github.com/irshadpc/axym-community/releases/download/tui-v0.18.0/axym-0.18.0-macos.tar.gz
tar -xzf axym.tar.gz && ./axym-macos-arm64 --version
```

> TUI links are pinned to the `tui-v0.18.0` tag rather than
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

> **12.15.0 Linux packaging:** this release was built on macOS, so it ships the
> macOS universal `.dmg`, the Linux **portable tarball**, and the Windows zip.
> The Linux `.deb` / `.AppImage` / `.rpm` installers are produced on a Linux
> build host — the newest of those on this page are `12.8.0`. Grab the tarball
> below if you want 12.15.0 on Linux today; it needs no installer.

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
curl -fL -o axym-linux.tar.gz https://github.com/irshadpc/axym-community/releases/download/desktop-v12.15.0/axym-12.15.0-linux-x86_64.tar.gz
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

- **Desktop (macOS):** the app checks for updates itself and offers
  one-click install + relaunch.
- **Desktop (Linux / Windows portable):** download the newer tarball or zip
  from [Releases](../../releases) and replace the old one. `.deb` / `.rpm`
  users can install the newer package over the old one when one is published.
- **TUI:** `/update` inside the TUI (or `axym --update`) downloads the
  latest release, verifies its SHA-256, and installs in place.

## Requirements at a glance

| | Needs | No admin? | Auto-updates? |
|---|---|---|---|
| TUI | One agent CLI on `PATH` (see above) | Yes | Yes — `/update` |
| Desktop macOS | — (quarantine clear on first run) | Yes | Yes, in-app |
| Desktop Linux AppImage | FUSE (`libfuse2` on Ubuntu 22.04+) | Yes | Via re-download |
| Desktop Linux deb/rpm | root for install | No | Via re-install |
| Desktop Windows | WebView2 (built into Win 10/11) | Yes (portable) | Via re-download |

## Help & feedback

- [Open an issue](../../issues) — bugs, crashes, installer problems.
  Include your OS, the exact file you downloaded, and (for the TUI) the
  output of `/doctor`.
- [Browse releases](../../releases) — changelogs ship with every version.
- Verify any download with the `SHA256SUMS.txt` published next to it:
  `shasum -a 256 -c SHA256SUMS.txt` (macOS/Linux).
