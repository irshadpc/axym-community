# Axym

An autonomous engineering assistant: give it a goal in plain English and it plans, writes, reviews, and tests the code. Download a build below. You don't need an account or any build tools.

[![Latest release](https://img.shields.io/github/v/release/irshadpc/axym-community?label=latest%20release&color=blue)](../../releases)

| | **Axym TUI** — terminal agent | **Axym Desktop** — full app |
|---|---|---|
| Version | [`0.18.0`](../../releases/tag/tui-v0.18.0) | [`12.15.0`](../../releases/tag/desktop-v12.15.0) |
| macOS (Apple Silicon + Intel) | [axym-0.18.0-macos.tar.gz](https://github.com/irshadpc/axym-community/releases/download/tui-v0.18.0/axym-0.18.0-macos.tar.gz) | [Axym_12.15.0_universal.dmg](https://github.com/irshadpc/axym-community/releases/download/desktop-v12.15.0/Axym_12.15.0_universal.dmg) |
| Linux x86_64 | [axym-0.18.0-linux-x86_64.tar.gz](https://github.com/irshadpc/axym-community/releases/download/tui-v0.18.0/axym-0.18.0-linux-x86_64.tar.gz) | [axym-12.15.0-linux-x86_64.tar.gz](https://github.com/irshadpc/axym-community/releases/download/desktop-v12.15.0/axym-12.15.0-linux-x86_64.tar.gz) |
| Windows x86_64 | [axym-0.18.0-windows-x86_64.zip](https://github.com/irshadpc/axym-community/releases/download/tui-v0.18.0/axym-0.18.0-windows-x86_64.zip) | [axym-12.15.0-windows-x86_64.zip](https://github.com/irshadpc/axym-community/releases/download/desktop-v12.15.0/axym-12.15.0-windows-x86_64.zip) |

**Which one?** The **TUI** is one standalone executable: a session UI, permission gate and headless mode built on top of the AI coding CLI you already use. The **Desktop** app runs the whole pipeline (plan → design → code → review → security → QA), with mission control, approval gates and in-app updates.

## Quick start

**TUI** needs at least one agent CLI on your `PATH`: `claude`, `opencode`, `codex`, `gemini`, `cursor-agent`, `copilot`, or `agy`.

```sh
tar -xzf axym-0.18.0-macos.tar.gz
./axym-macos-arm64 --install     # Intel Mac: ./axym-macos-x64 --install
axym                             # start a session in the current project
```

**Desktop (macOS)** — this one command downloads and installs the app and clears the Gatekeeper warning:

```sh
curl -fsSL https://github.com/irshadpc/axym-community/releases/latest/download/install-macos.sh | bash
```

Linux, Windows and troubleshooting are covered in [Install details](#install-details).

## New in this release

- **Parallel runs are safe:** when one desktop run finished, it used to stop the agent processes and builds of every other run still going. That no longer happens.
- **Every run scores itself:** each finished run records how it went (accepted, useful, quality, defects prevented and more), and QA only passes when the real test and build results back it up.
- **TUI:** `/pipeline` shows a run stage by stage, `/po` turns a fuzzy request into a scoped decision, and there are new `/health`, `/intel` and `/settings` views.

Full notes: [TUI 0.18.0](../../releases/tag/tui-v0.18.0) · [Desktop 12.15.0](../../releases/tag/desktop-v12.15.0) · [all releases](../../releases)

## Install details

<details>
<summary><b>TUI — macOS / Linux</b></summary>

The macOS archive contains both `axym-macos-arm64` (Apple Silicon) and `axym-macos-x64` (Intel). The Linux archive contains `axym-linux-x86_64`.

```sh
tar -xzf axym-<version>-macos.tar.gz     # or the linux tarball
./axym-macos-arm64 --version
./axym-macos-arm64 --install             # copies it to ~/.local/bin and installs the man page
```

If macOS blocks the first launch, run this once: `xattr -d com.apple.quarantine axym-macos-arm64`.

</details>

<details>
<summary><b>TUI — Windows</b></summary>

Extract the zip and run `axym-windows-x86_64.exe` from PowerShell or `cmd`. Running `axym-windows-x86_64.exe --install` copies it to `%LOCALAPPDATA%\Axym\bin\axym.exe` and tells you if that folder isn't on your `PATH` yet.

</details>

<details>
<summary><b>TUI — first commands</b></summary>

```sh
axym                                      # interactive session
axym --provider claude --mode plan        # pick a provider; read-only mode
axym -p "summarize uncommitted changes"   # headless, for scripts and CI
axym web --host 0.0.0.0 --pin 4821        # control it from your phone: scan the QR code
```

To explore inside the TUI: type `/` to see every command, `/doctor` to check your providers and setup, and run `axym --man` for the full manual.

</details>

<details>
<summary><b>Desktop — macOS</b></summary>

Open the `.dmg` and drag Axym to Applications, or use the one-line installer in [Quick start](#quick-start). The builds are signed without an Apple developer certificate. If macOS says the app "is damaged", run this once: `sudo xattr -dr com.apple.quarantine /Applications/Axym.app`.

</details>

<details>
<summary><b>Desktop — Linux</b></summary>

```sh
tar -xzf axym-12.15.0-linux-x86_64.tar.gz
./axym-linux/axym                 # or ./axym-linux/install.sh
```

- The app needs the system-tray library. Desktop Ubuntu already has it; on minimal or server images run `sudo apt install libayatana-appindicator3-1` (Fedora: `sudo dnf install libayatana-appindicator-gtk3`).
- This release has only the portable tarball for Linux. The newest `.AppImage`, `.deb` and `.rpm` installers are from [12.8.0](../../releases/tag/desktop-v12.8.0). The AppImage needs FUSE: `sudo apt install libfuse2`.

</details>

<details>
<summary><b>Desktop — Windows</b></summary>

Extract the zip and run `axym.exe`. It needs the WebView2 runtime, which Windows 10 (1803+) and Windows 11 already include.

</details>

<details>
<summary><b>Verify a download</b></summary>

Every release includes a `SHA256SUMS.txt` file. Put it in the same folder as your download and run `shasum -a 256 -c --ignore-missing SHA256SUMS.txt`. TUI releases also include `release-manifest.json`, which lists the version, the source commit and each file's hash.

</details>

## Requirements and updates

| | Needs | Admin rights? | Updates |
|---|---|---|---|
| TUI | One agent CLI on `PATH` | No | `/update` (or `axym --update`) — verifies the download, then installs it |
| Desktop macOS | — | No | Built into the app: one click |
| Desktop Linux / Windows (portable) | System-tray library (Linux), WebView2 (Windows) | No | Download the new archive from [Releases](../../releases) |

The first launch on any OS may show an "unknown publisher" warning that you can dismiss.

## Help

[Open an issue](../../issues) for bugs, crashes or installer problems. Please include your OS, the file you downloaded, and, for the TUI, the output of `/doctor`.
