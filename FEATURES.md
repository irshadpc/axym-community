# Axym feature catalog

Every user-facing capability in Axym, grouped by the job you are trying to do. The TUI is the fast terminal agent; the Desktop app is the full engineering pipeline. Both share the same provider conventions, project configuration and safety model.

**Last updated:** 2026-10-30 · **Axym TUI:** 0.21.0 · **Axym Desktop:** 12.16.0

> This catalog is generated from one source so the TUI and Desktop always describe the same capabilities.

## Axym TUI — terminal agent

One standalone executable: a session UI, permission gate and headless mode built on top of the AI coding CLI you already use.

### Sessions, providers & models

Bring any installed agent CLI, switch models mid-session, and recover when a provider fails.

- **One session, any agent CLI** — Drive Claude Code, OpenCode, Codex, Gemini CLI, Cursor Agent, GitHub Copilot CLI, Command Code and Antigravity through one consistent terminal interface.
  _Where:_ axym --provider claude · /provider · /model
- **Provider discovery and defaults** — Finds installed CLIs on PATH and in common install directories, accepts ids, aliases and display names, and suggests the closest match on a typo.
  _Where:_ ~/.axym/config.yaml (default-coder) · ~/.axym/provider_paths.json
- **Mid-session provider, model and effort switching** — Change the agent, model or reasoning effort without losing the conversation or restarting the session.
  _Where:_ /provider · /model · /models · /effort
- **Automatic failover and provider cooldowns** — Classifies failures as immediate, after-reset or never; benches quota walls until their reset time and policy walls for 24h; a circuit breaker stops repeated sub-minute failures.
  _Where:_ Automatic on a failed turn; /priority shows the resolution order
- **Persistent Claude connection** — Claude runs as a long-lived stream-JSON process, so repeat turns skip CLI re-initialisation and permission requests flow over live stdin.
  _Where:_ Automatic for the Claude provider
- **Sessions, resume and git reconciliation** — Saves every finished turn as JSONL, restores the provider's own session id, and tells you what changed in the workspace since the session was last active.
  _Where:_ /sessions · /resume · /continue · axym -c

### Chat, transcript & results

Read the agent's work as it happens and see a structured result when the turn ends.

- **Streaming chat and tool activity** — Token-level deltas, lightweight markdown and animated tool calls with real commands, paths and outputs instead of raw protocol dumps.
  _Where:_ Automatic in every interactive session
- **Result Ledger** — Each turn ends with an outcome banner (OK, FAIL, VER, UNV), modified files, verification checks, blockers or risks, a concise summary and contextual next actions.
  _Where:_ Automatic at turn completion; Ctrl+O expands detail
- **Markdown and syntax highlighting** — Fenced code is highlighted for JavaScript/TypeScript, Python, Rust, Go, shell, YAML/TOML and C-family languages with no external dependency.
  _Where:_ Automatic in responses
- **Message queueing** — Text submitted while a turn is running is queued and drains automatically when the turn ends, with a visible queued count.
  _Where:_ Type while the agent is working
- **Transcript search and turn jump** — Search the current session, open a scrollable hit preview, or jump directly to an earlier conversational turn.
  _Where:_ Ctrl+F · /find · /turns
- **Copy and export** — Copy the latest or an earlier response to the system clipboard, or save the full transcript as markdown.
  _Where:_ /copy · /export

### Input & editor

A terminal composer that behaves like a real editor, with fast paths for files, history and images.

- **Multi-line composer with readline keys** — Cursor motion, word jump, kill/yank, undo, history navigation and a hinted footer work across single-line and multi-line input.
  _Where:_ Shift+Enter or Ctrl+J for a newline; Ctrl+A/E/U/K/W for editing
- **Vim mode and external editor** — Persistent Vim keybindings for the prompt, or open the current prompt in your own $VISUAL/$EDITOR.
  _Where:_ /vim · Ctrl+G
- **@file completion** — Fuzzy-complete repository files from git ls-files, or a bounded directory walk outside git, ranked by filename, path and subsequence match.
  _Where:_ Type @ followed by part of a path
- **Prompt history and reverse search** — Keeps the last 500 prompts and offers bash-style reverse search over them.
  _Where:_ Up/Down · Ctrl+R
- **Paste formatting and image attachments** — Formats large pasted text into a collapsible prompt, and attaches screenshots or image files as [Image #n] inputs.
  _Where:_ Ctrl+Shift+F · Ctrl+V · Ctrl+I · Ctrl+Shift+S

### Modes, permissions & safety

Decide how much the agent may do, and keep the terminal recoverable when something goes wrong.

- **Plan, Ask, Edit and Auto modes** — Plan is read-only, Ask never edits, Edit auto-accepts file edits, and Auto bypasses permission prompts; each maps to the provider's native permission mode.
  _Where:_ Shift+Tab · /mode plan|ask|edit|auto
- **Inline permission gate** — Permission-requiring tool calls are relayed to the TUI and answered there: allow or deny with the real command or path shown. Gate errors and interrupts deny by default.
  _Where:_ Answer y or n when a gate appears; AXYM_TUI_NO_RELAY=1 opts out
- **Always-allow command list** — Review or clear exact commands you have chosen to always allow, so repeated safe commands stop asking.
  _Where:_ /permissions · /permissions clear
- **Explicit YOLO mode** — Skips every approval prompt and implies Auto mode, with the risk stated up front. Governed runtime approval gates are never bypassed.
  _Where:_ axym --yolo
- **Crash-safe terminal handling** — A render error or external signal unmounts cleanly, restores raw-mode terminal state, and releases the wake-lock child instead of leaving a garbled screen.
  _Where:_ Automatic

### Workspaces & engineering views

Move from a conversation to the evidence, graph, health and handoff views behind it.

- **Adaptive workspaces** — Switch the session between explore, execute, investigate, review and approval modes so the header, hints and available actions match the job.
  _Where:_ /workspace explore|execute|investigate|review|approval
- **Pipeline view** — Lists recent runtime runs, then shows a run's recorded stages, files and verification events stage by stage.
  _Where:_ /pipeline [run-id]
- **Execution graph** — Interactive stage, gate, agent and task dependency tree for a runtime run.
  _Where:_ /graph [run-id]
- **Evidence browser** — Inspect the evidence primitive behind a claim: inspected, inferred, verified and failed proof.
  _Where:_ /evidence [id]
- **Health, intelligence and observatory** — Read the daemon self-report and provider cooldowns, the AEI decision ledger with estimates and measured outcomes, and active skills with evaluation candidates.
  _Where:_ /health · /intel · /observatory
- **Handover and Work Passport** — See session state, unresolved tasks and uncommitted work, or show and export the durable passport (product brief, acceptance criteria, execution truth, recovery context).
  _Where:_ /handover · /passport
- **Settings, themes and density** — Toggle Vim editing, verbose tool output and a compact header, or switch between graphite, classic and high-contrast themes.
  _Where:_ /settings · /theme graphite|classic|contrast

### Verification, review & delivery

Turn a change into evidence: review it, test it, scan it, and keep a reversible history.

- **Code review and QA** — Runs a code-quality pass for hidden assumptions, simplicity, surgical diffs, edge cases and test completeness, with file and line references.
  _Where:_ /review [goal] · /qa [goal]
- **Security review** — Threat-models the target, reviews code and configuration with captured evidence, and returns a prioritized remediation plan.
  _Where:_ /security [goal] · /audit · /security-audit
- **Background test runner** — Runs the project's tests without blocking the conversation and reports the result back into the session.
  _Where:_ /test [command]
- **Diff, pull, undo and bisect** — Inspect uncommitted changes, pull the remote tracking branch, shelve work into a reversible stash, or binary-search checkpoints for the turn that introduced a regression.
  _Where:_ /diff · /pull · /undo · /bisect
- **Reusable workflows** — Define a named sequence of prompts once, then replay it on any provider. Four starter workflows cover engineering, product discovery, security review and QA.
  _Where:_ /workflow new|run|save|list · /workflows
- **Project setup** — Writes a starter .axym.yaml shared with the desktop app, without ever overwriting an existing file.
  _Where:_ /init

### Skills & customization

Extend the agent with the same skills and project configuration the desktop app uses.

- **Bundled skills** — Product-Owner Intelligence, Axym Engineering Intelligence, reverse engineering and break-it QA ship inside the binary and work in any project.
  _Where:_ /skills · /skill <name> [task] · /po · /aei
- **Project and user skills** — Discovers SKILL.md definitions from .agents, .claude, .codex and compatible project or user folders, with mode support.
  _Where:_ /skills · /skill <name> [task]
- **Bundled SCA remediation** — Runs the sca-fix skill to upgrade a Java/Maven pom.xml and write a plain-language justification spreadsheet for a Sonatype/SCA report.
  _Where:_ /sca-fix [report-pdf]
- **Slash-command plugins** — External modules can register narrow slash commands; built-in commands always win.
  _Where:_ See the plugin API in the TUI docs
- **Project configuration** — Reads .axym.yaml for tech stack, language, style guide, forbidden libraries, custom instructions, secrets, capabilities and budgets.
  _Where:_ .axym.yaml at the project root

### Multi-model, fleet & automation

Use more than one model deliberately, watch many runs, and automate the TUI from scripts.

- **Council mode** — Runs the same prompt across two or more providers in parallel and merges their answers into one synthesized response.
  _Where:_ /council <prompt>
- **Cross-provider verification** — Run review or security on a different provider than the coder; the run labels whether the verifier was independent of the code author or shared its provider and model.
  _Where:_ Desktop → Expert options → AI system per stage
- **Agent Hub and fleet overview** — Toggle a live roster of runtime runs, select and attach to one, or read configured capacity, active runs, oldest in-flight work and recent failures.
  _Where:_ Alt+A · /agents · /fleet
- **Run comparison and stage costs** — Compare what two runtime runs actually ran against (commit, dependencies, config), and read per-stage token, spend and call-count evidence.
  _Where:_ /compare [run-a run-b] · /pipeline · /cost [run-id]
- **Headless mode** — Run one prompt from the shell or stdin with text or JSONL output and stable exit codes for scripts and CI.
  _Where:_ axym -p "prompt" · --format json · exit 0/1/2/130
- **Live run steering and queueing** — Change course mid-turn at the next safe boundary on the governed runtime, or queue guidance for later without interrupting the run.
  _Where:_ /steer <instruction> · /queue <instruction>
- **Follow, fork and compare runtime runs** — Attach to a Desktop or Mobile run with durable replay and live updates, fork it into a child lineage, or open a second read-only viewer.
  _Where:_ /attach · /view · /fork

### Collaboration, remote & handoff

Keep working when you leave the terminal, and let other people follow or take over safely.

- **Share and read-only viewing** — Share a session or live run with collaborators; viewers follow an isolated read-only event stream and cannot steer, approve or write.
  _Where:_ /share [run-id] · /view <run-id>
- **Phone pairing** — Continue the live session from a phone over the same Wi-Fi, Tailscale or a Cloudflare tunnel, with a QR pairing card, PIN or scoped access and a revocable link.
  _Where:_ /remote · /remote tunnel · /remote status · /remote stop
- **Browser bridge** — Serve a phone-friendly page over the governed runtime with live transcript, submit, steer, cancel and approval actions, protected by a PIN or token.
  _Where:_ axym web --host 0.0.0.0 --pin 4821
- **Detach and attach** — Free the terminal while the session keeps running, then return from any terminal or SSH login with the transcript redrawn at the new size.
  _Where:_ /detach · axym attach · axym -c
- **Push notifications** — Send a push when a turn finishes, fails, or every provider is cooling, through ntfy.sh or a self-hosted topic.
  _Where:_ /notify · ~/.axym/tui/notify.json
- **Session and run handoff** — Export or import a session, or move a multi-machine run checkpoint with offer, export, accept, status and retract actions.
  _Where:_ /handoff · /run-handoff

### Runtime, recovery & operations

Connect to the governed runtime, keep long work alive, and update safely.

- **Governed runtime connection** — Use axym-runtime for durable runs, evidence, approvals, fleet views and run replay while keeping the same terminal interface.
  _Where:_ axym --provider axym-runtime · /attach · /pipeline
- **Laya decisions and vibe mode** — Classify low-risk decisions, announce and record them, or ask the runtime for the smallest honest pipeline it can justify.
  _Where:_ /laya on|off|status · /vibe on|off|status
- **Provider priority** — Show how the default provider was resolved across environment variables, config and provider manifests.
  _Where:_ /priority
- **Self-update with checksum verification** — Checks the release channel, downloads the platform asset, verifies its SHA-256 against SHA256SUMS.txt and installs atomically.
  _Where:_ /update · axym --update · --update-force
- **Keep-awake during long runs** — Holds a power assertion while a turn runs so idle sleep cannot suspend the work; opt out with a marker file.
  _Where:_ Automatic; ~/.axym/tui/no-keep-awake to disable
- **Atomic local state** — Sessions, history and allow-lists are written with a temp-file, fsync and rename sequence, so a crash cannot leave half-written state.
  _Where:_ Automatic

## Axym Desktop — full app

The full engineering app: governed plan, design, code, security, review and QA runs with mission control, approval gates and an audit trail.

### Pipeline & execution

Turn a plain-English goal into a governed engineering run with the right amount of planning and parallelism.

- **Six-stage governed pipeline** — Goals flow through plan, design, code, security, review and QA, with stage-level gates, evidence and outcome scoring.
  _Where:_ Start a Build from the home screen
- **Council mode** — Fans plan, design, review and security prompts to up to three providers in parallel, ranks the anonymous candidates and has a chairman synthesize the result.
  _Where:_ Choose Highest Confidence, or set AXYM_COUNCIL_MODE=on
- **Deep Reasoning with a pre-execution preview** — An analyser composes the pipeline and per-stage providers from evidence, then shows stages, confidence, cost, time and risk before any LLM call.
  _Where:_ Choose Deep Reasoning, then inspect the Reasoning Preview
- **Dynamic graph execution mode** — Task-grain parallelism with admission control, role-counted node caps, write-set conflict groups, cycle traces, verifier-required nodes and economic-governor cost posture.
  _Where:_ Choose graph execution for a run
- **Parallel isolated runs** — Runs execute in isolated worktrees; finishing one run no longer stops the agents or builds of other runs, and switching runs cannot show another run's data.
  _Where:_ Start multiple runs; inspect each in Active run
- **Priority task steering** — Durable fair-share ranking moves urgent work ahead without starving other active work.
  _Where:_ Run queue priority controls
- **Build-check loop and chunked code generation** — Generates large projects in parallel batches, runs the real build, and feeds compiler errors back to the provider for up to two correction passes.
  _Where:_ Automatic for large or compiled projects
- **Resume, checkpoints and replay** — Durable execution state survives crashes, token exhaustion and reboots; resume packages and replay reconstruct the run from its event history.
  _Where:_ Continue an interrupted run · History & Replay

### Mission control & navigation

Start, find and orient work without knowing the internal architecture.

- **Engineering Mission Control** — The home screen answers what you are building, what changed, what needs attention, what Axym is doing, what comes next and whether you can trust it.
  _Where:_ Open Mission Control
- **Universal command surface** — One keyboard modal to switch activities, jump to work items, launch builds and run quick actions.
  _Where:_ Cmd+K · Ctrl+K
- **Workspace Hub and projects** — Organise work by project with recent, pinned, favourite, template and archived projects.
  _Where:_ Projects
- **Activities and role workspaces** — Pick a role-shaped activity such as coding, review, incident, product thinking, requirements, interface design, QA, architecture, research, sprint or retro.
  _Where:_ Activities
- **My Work queue** — Prioritises active tasks by urgency, blockers and decisions only a human can make.
  _Where:_ My Work
- **Guided onboarding and progressive disclosure** — Provider discovery, a product tour, experience levels and plain-language help reveal deeper screens only when they become useful.
  _Where:_ First launch · Settings · FAQ

### Runs, approvals & review

See exactly what the agent did, approve it with evidence, and compare outcomes.

- **Live pipeline DAG and run detail** — Watch each stage light up in real time and inspect its files, verification events, provider, timing and cost.
  _Where:_ Active run · Run detail
- **Approval gates with real diffs** — Pause before changes touch the working tree, inspect file diffs and tool-execution requests, then approve or reject.
  _Where:_ Approval Gate card · Approvals
- **Risk-graded diff viewer** — Syntax-highlighted unified diffs with risk indicators for sensitive files and destructive changes.
  _Where:_ Open a run's diff
- **Grounded QA and honest scoring** — A claimed test or build pass must be backed by the runner's real exit code and output. Unmeasured quality dimensions are never scored as perfect.
  _Where:_ Automatic at run completion; inspect the outcome score
- **Run history with outcome scores** — Every finished run records accepted, useful, effort reduced, quality, defects prevented and maintainability signals in the engineering knowledge graph.
  _Where:_ History
- **Run comparison and environment compare** — Compare two runs or their environments by commit, dependencies and configuration before deciding which result to keep.
  _Where:_ Compare view
- **Read-only watch mode** — Follow a running run without any write authority, so reviewers and collaborators can observe safely.
  _Where:_ Watch from a running run

### Providers, models & routing

Use the best available model for each stage and fail over without losing the run.

- **Multi-provider layer** — Drives Claude Code, OpenCode, Codex, Gemini, Antigravity, Stitch, GLM, Ollama and Aider through their local CLIs with a shared transport contract.
  _Where:_ Settings · Provider Manager
- **Outcome profiles and quality presets** — Recommended, Fast, High Quality, Free and Local profiles resolve to the first installed provider; Fast, Balanced, Powerful and Custom presets map per-provider models without mutating your chosen provider.
  _Where:_ New Run · provider and quality pickers
- **Provider discovery and custom providers** — Detects Ollama, agent CLIs and cloud keys, enriches PATH, scans portable binaries and supports aliases and private providers.
  _Where:_ Automatic · ~/.axym/providers.json
- **Provider health and circuit breakers** — Tracks launch, execution, cancellation and recovery success, then fails over per provider with concurrency caps.
  _Where:_ Provider Status · Health
- **Adaptive routing** — Uses Thompson sampling, UCB1 and cost pressure to choose providers, with outcome profiles keeping the default choice simple.
  _Where:_ Automatic; inspect in AI Learning
- **Prompt transport and token budgets** — Selects stdin, CLI argument or temp-file delivery by provider capability and prompt size, and tracks per-stage token use with exhaustion risk.
  _Where:_ Automatic · run cost breakdown
- **Council decision records** — Preserves the full candidate set, anonymized ballots and Borda consensus scores for every council run.
  _Where:_ Open a council run's decision record

### Intelligence & learning

The system keeps evidence about what worked, what failed and what to try next.

- **Trust Engine** — Weighted engineering confidence across validation, regression, provider reliability, documentation freshness, accessibility, performance, security, peer review and release readiness.
  _Where:_ Reliability · run confidence badge
- **Benchmark ledger** — A domain by provider by metric matrix for planning, architecture, implementation, security, code review, testing and recovery.
  _Where:_ Benchmark
- **Engineering Knowledge Network** — A persistent evidence-weighted graph of decisions, patterns, outcomes and relationships that later runs can query.
  _Where:_ Engineering Knowledge
- **Engineering memory and LEARNINGS.md** — Records observations, failures, bugs, lessons, recommendations and plans; each run output includes LEARNINGS.md and CONVENTIONS.md.
  _Where:_ Run output folder · AI Learning
- **Digital twin, simulation farm and scoreboard** — Observes, tests and breaks the system against known scenarios, runs persona journeys without real AI calls, and scores health across ten dimensions.
  _Where:_ System Insights · Engineering Health
- **Recommendation and optimization engines** — Suggests providers, workflows and patterns from evidence, and tunes checkpoint frequency or stage ordering from observed performance.
  _Where:_ Recommendations
- **Failure learning and recovery policy** — Classifies and clusters failures, learns routing and recovery policies in shadow mode, and ingests human feedback at higher weight.
  _Where:_ AI Learning · Recovery Center

### Verification, security & governance

Deterministic checks and non-bypassable policy sit between the model and your working tree.

- **Tool admission policy** — Validates task contracts, workspace boundaries, quotas, concurrency, budget, transport safety and schema before anything runs.
  _Where:_ Automatic for every run
- **Deterministic verification and false-green detection** — Runs real test, lint and typecheck commands, verifies exit codes and test counts, and flags a runner that exits zero without running tests.
  _Where:_ Automatic before a run is accepted
- **Security engineering** — Recon, static analysis, secret scanning, dependency audits and configuration checks; vulnerability claims need captured observations.
  _Where:_ Security · /security in the TUI
- **Policy engine, standards and compliance** — Roles, policies, standards, review boards and compliance reports make governance inspectable rather than implicit.
  _Where:_ Platform · Standards · Policies · Compliance
- **Economic governor and budgets** — Throttles spend through Normal, Restricted and Blocked states, backs off expensive work and enforces a run budget.
  _Where:_ Set RUN_BUDGET_USD · Cost Breakdown
- **Credential encryption and privacy** — Credentials are encrypted with AES-256-GCM in app-scoped storage; a network kill switch blocks all non-localhost traffic.
  _Where:_ Settings · AXYM_DISABLE_NETWORK=1
- **Tamper-evident audit log** — Every run event is hash-chained in SQLite WAL storage with startup tamper detection, so history is reproducible and auditable.
  _Where:_ Automatic · History & Replay

### Skills, plugins & customization

Extend both apps with the same versioned skills, plugins and project context.

- **Skill Dashboard and hot reload** — List, enable, disable, inspect and hot-reload skills without restarting the app.
  _Where:_ Settings · Skills
- **Versioned, sandboxed skill packages** — YAML manifests carry version, dependencies and capabilities; security-critical skills run in sandbox v2 with per-skill limits, and packages can auto-upgrade or roll back.
  _Where:_ Skill Dashboard · axym skill upgrade
- **Bundled skills** — Product-Owner Intelligence, Axym Engineering Intelligence, reverse engineering, break-it QA and sca-fix ship with the app.
  _Where:_ Skill Dashboard · /skills in the TUI
- **Skill test harness** — Runs a skill end to end in isolation before you rely on it.
  _Where:_ axym skill test <skill>
- **Plugins and MCP** — Discovers prompt templates, rules, plugins and MCP servers, with capability enforcement for untrusted plugins.
  _Where:_ Ecosystem · Settings
- **Project configuration and ECS** — Reads .axym.yaml and builds a portable .axym/engineering context (manifest, architecture, modules, knowledge packs and deltas).
  _Where:_ .axym.yaml · ECS import

### Operations, observability & assurance

See the whole system's health, history and release readiness.

- **Operations Center** — Observe-only registry of ten recurring jobs covering health, validation, benchmarks, improvements, dependencies, debt, docs, strategy, workflow and architecture.
  _Where:_ Operations Center
- **EOS Kernel and System Health** — A service registry with discovery, dependency graph, unified health roll-up and resource-aware scheduling that pauses on low battery or metered networks.
  _Where:_ System Health
- **Morning report and value ledger** — Synthesizes overnight job activity into a short report of what ran, what changed and what needs a decision.
  _Where:_ Mission Control banner
- **History and Replay (Time Machine)** — A unified timeline, decision timeline, incremental replay, two-approach comparison, counterfactual what-if projections and lesson extraction.
  _Where:_ History & Replay
- **Certification** — Twelve certifiers roll up into a readiness score of Certified, Provisional or Not Certified, with read-only, fail-open checks.
  _Where:_ Certification
- **Release assurance** — Release gates, architecture fitness contracts, product readiness, release validation and executive review keep a release honest.
  _Where:_ Platform · Release Review · Release Validation
- **Enterprise and organizations** — Organization settings, enterprise mission control, approval dashboards, standards, policies and compliance for team deployments.
  _Where:_ Enterprise · Organizations

### Collaboration, mobile & continuity

Move work between people, devices and machines without losing the thread.

- **Mobile connect** — Authenticated test submissions, a workspace-switcher projection and event-log resync when a phone reconnects.
  _Where:_ Mobile connect
- **Run invites and sharing** — Invite collaborators to follow or contribute to a run with scoped access.
  _Where:_ Run detail · Share
- **Remote sessions and viewers** — Attach from another surface with an isolated read-only cursor, or let an approver decide a gate without giving them write access.
  _Where:_ Run invites · TUI /attach and /view
- **Handoff and multi-machine continuity** — Offer, export, accept, verify and retract a run handoff so work can continue on another machine.
  _Where:_ Run handoff
- **Resume banner and interrupted work** — One-click continuation of an interrupted run with a concise summary of what completed and what remains.
  _Where:_ Resume banner
- **Crash-safe recovery** — A journaled Effect Bus, SQLite WAL event store, backup and restore, and git reconciliation keep state consistent after a crash.
  _Where:_ Automatic · Recovery Center
- **In-app updates and release validation** — Desktop updates install from inside the app; release artifacts are checked with seeded-fault detection before they ship.
  _Where:_ Settings · update prompt

## Shared foundation

Both products deliberately share the same runtime conventions, so a project configured once works everywhere.

### One runtime, three interfaces

The same Rust/Tauri core backs Desktop, the TUI and the mobile bridge.

- **Canonical event and run model** — Desktop, TUI and mobile read the same SQLite run log and event contract, so a run started in one surface can be inspected from another.
  _Where:_ SQLite WAL event store
- **Shared project configuration** — .axym.yaml and ~/.axym/providers.json are read by both products.
  _Where:_ .axym.yaml · ~/.axym/config.yaml
- **Local-first by default** — The embedded runtime, event store and side-effect journal run on your machine; no telemetry leaves it.
  _Where:_ Automatic

### Safety and evidence

The same fail-closed rules apply whichever surface you use.

- **Approval before mutation** — Destructive or workspace-mutating actions are gated and can be denied without touching the working tree.
  _Where:_ TUI permission gate · Desktop approval gates
- **Evidence-backed outcomes** — A success claim must be supported by real command output, exit codes and verification records; unverified work is marked as such.
  _Where:_ Result Ledger · run outcome scores
- **Replayable history** — Hash-chained events make every run reproducible and tamper-evident.
  _Where:_ History & Replay · axym replay

### Extensibility

Skills, plugins and project context work across both products.

- **Portable skills** — SKILL.md skills and versioned skill packages are discoverable from the same folders and bundled set.
  _Where:_ /skills · Skill Dashboard
- **Provider-agnostic** — Use whichever agent CLI you already trust; Axym adapts transport, permissions and failover around it.
  _Where:_ Providers · ~/.axym/providers.json
