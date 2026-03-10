# cc-plugin-marketplace

My personal Claude Code plugin marketplace. Contains the `devtools` plugin — a collection of specialized agents, slash commands, and skills for software development.

---

## Installation

**Step 1 — Add this repo as a marketplace:**

```bash
claude plugin marketplace add https://github.com/scottmcclung/cc-plugin-marketplace.git
```

**Step 2 — Install the devtools plugin:**

```bash
claude plugin install devtools
```

That's it. Agents, commands, and skills are available immediately in any Claude Code session.

**To update later:**

```bash
claude plugin update devtools
```

---

## Dependencies

Some features require external tools. Here's what you need and when:

### beads — required by `/work`, `/plan`, `/reflection`

All three commands depend on [beads](https://github.com/steveyegge/beads), a git-backed issue tracker for Claude Code. Install it as a separate plugin:

```bash
claude plugin marketplace add https://github.com/steveyegge/beads.git
claude plugin install beads
```

The `bd` CLI must be on your PATH. Run `bd --version` to verify.

### @playwright/cli — required by the `playwright-cli` skill

The browser automation skill drives a standalone Playwright CLI (not the Node.js testing library).

```bash
npm install -g @playwright/cli
playwright-cli install-browser   # installs Chromium
```

Verify with `playwright-cli --version`.

### pandoc — required by the `pandoc-converter` skill

```bash
brew install pandoc              # macOS
sudo apt-get install pandoc      # Ubuntu/Debian
```

PDF output also requires a LaTeX engine:

```bash
brew install --cask basictex     # minimal, macOS (~100MB)
# or
brew install --cask mactex       # full LaTeX, macOS (~5GB)
```

Verify with `pandoc --version`.

### Docker — required by the `tailscale-serve-sidecar` skill and `dev-ops-engineer` agent

Docker (with Docker Compose) is needed to use the Tailscale sidecar skill and for any containerized dev work the DevOps agent handles. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) or your preferred Docker distribution.

Verify with `docker --version` and `docker compose version`.

### gh (GitHub CLI) — used by `dev-ops-engineer`

The DevOps agent uses `gh` for GitHub operations (PRs, issues, checks).

```bash
brew install gh                  # macOS
gh auth login                    # authenticate once
```

Verify with `gh --version`.

---

## What's Included

### Agents

Specialized sub-agents that Claude Code can delegate work to. They run autonomously on focused tasks.

| Agent | What it does |
|-------|-------------|
| `js-software-engineer` | Full-stack JS/TS engineer for Node.js, Deno, and Bun. Covers Vue, React, Svelte, Next.js, Express, Vitest, Playwright, and the broader modern JS ecosystem. |
| `dev-ops-engineer` | Git workflow specialist. Handles worktrees, branching, rebasing, merge conflicts, Docker/container tasks, CI/CD, and issue tracker operations. |
| `uat-specialist` | Validates features from an end-user perspective using browser automation, API calls, and CLI tools. Spins up environments, exercises user journeys, and files bugs with reproduction steps. |
| `deno-expert-agent` | Expert in Deno runtime — secure server-side code, REST APIs, Deno Deploy, Deno KV, CLI tools, and Deno-specific patterns. |
| `diagram-expert` | Generates professional diagrams (Mermaid for markdown, XML for draw.io) from code, architecture docs, or written requirements. |
| `laravel-php-expert-agent` | Laravel/PHP specialist covering Eloquent ORM, auth, REST/GraphQL APIs, queues, Livewire, Inertia.js, Sanctum, Horizon, and deployment. |
| `salesforce-platform-expert-agent` | Salesforce development — Apex, LWC, Visualforce, fflib patterns, Flows, Permission Sets, integrations, and governor limit solutions. |
| `salesforce-apex-test-engineer` | Writes and runs Apex tests (unit + integration). Targets 90% coverage, uses fflib Apex Mocks, and follows Salesforce naming conventions. |
| `crystal-lang-engineer` | Crystal language expert — type system, macros, concurrency (fibers/channels), performance optimization, C bindings, and PRs. |
| `crystal-code-reviewer` | Aggressive Crystal code reviewer focused on correctness, idioms, performance, and refactoring. |

### Slash Commands

Invokable with `/command-name` in any Claude Code session.

| Command | What it does |
|---------|-------------|
| `/work` | Project orchestrator. Takes a task and sequences it through the full dev lifecycle: branch → TDD (red/green) → implementation → code review → feedback → merge. Delegates to specialist agents. |
| `/plan` | Tech lead for planning. Decomposes a goal into scoped, ordered beads issues with milestone boundaries and parallel work identification. |
| `/reflection` | Reviews the current conversation and instructions to identify improvements. Proposes optimizations to Claude's prompts/behavior via beads issues. |

### Skills

Reusable capabilities Claude Code loads on demand when a matching task comes up.

| Skill | What it does |
|-------|-------------|
| `playwright-cli` | Browser automation via the Playwright CLI. Navigation, form interaction, screenshots, keyboard/mouse events, request mocking, storage state, tracing, and video recording. |
| `pandoc-converter` | Universal document conversion (53+ input formats, 68+ output formats). PDF generation, batch processing, bibliography handling, Lua filters, and presentations. |
| `tailscale-serve-sidecar` | Configures a Tailscale sidecar in Docker Compose to expose services over your tailnet — HTTPS, Funnel (public access), TCP/UDP proxying, and auth key management. |

---

## Other Useful Tools

Not part of this plugin, but worth having alongside it.

### Excalidraw Diagram Skill — beautiful architecture diagrams

Lets Claude Code generate Excalidraw diagrams from natural language — not just generic boxes and arrows, but layouts that mirror the concept (fan-outs for one-to-many, timelines for sequences, convergence for aggregation). Includes a Playwright-based render pipeline that visually validates diagrams before handing them back.

Install per-project (not global):

```bash
git clone https://github.com/coleam00/excalidraw-diagram-skill.git
cp -r excalidraw-diagram-skill .claude/skills/excalidraw-diagram
```

Then let the agent set itself up:

> "Set up the Excalidraw diagram skill renderer by following the instructions in SKILL.md."

Or manually:

```bash
cd .claude/skills/excalidraw-diagram/references
uv sync
uv run playwright install chromium
```

Requires `uv` and `python` on your PATH.

---

## Plugin Management Reference

```bash
# List installed plugins
claude plugin list

# List configured marketplaces
claude plugin marketplace list

# Remove the marketplace
claude plugin marketplace remove cc-plugin-marketplace

# Uninstall the plugin
claude plugin uninstall devtools
```
