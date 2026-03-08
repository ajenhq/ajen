<div align="center">

<img src="./assets/logo.png" alt="Ajen Logo" width="120" />

# Ajen

**Ajna + Genesis**

Describe a startup. Watch AI employees build it. Ship in minutes, not months.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Rust](https://img.shields.io/badge/Rust-1.85+-orange?logo=rust&logoColor=white)](https://www.rust-lang.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[Website](https://www.ajen.dev) · [Dashboard](https://www.ajen.dev) · [Contributing](CONTRIBUTING.md)

</div>

---

## What is Ajen?

Ajen is an open-source engine that spins up an entire AI-powered company from a single prompt. You describe the idea — Ajen creates a CEO, CTO, developers, designers, and content writers that plan, build, and deploy your product autonomously.

You sit on the board. The AI team does the rest.

> **Ajen** = **Ajna** (the third-eye chakra — vision, intuition, insight) + **Genesis** (origin, creation). See the idea. Bring it to life.

---

## How It Works

**Idea → CEO Plan → Board Approval → Team Execution → Product**

1. **Describe your startup idea**
   Tell Ajen what you want to build.

2. **The AI CEO creates a plan**
   A structured roadmap with milestones, architecture, and tasks.

3. **You approve as the board**
   Nothing starts until you approve the plan.

4. **AI employees execute**
   Developers, designers, marketers, and operators begin working.

5. **Watch it happen live**
   Every decision, tool call, and update streams to the dashboard in real time.

<p align="center">
  <a href="https://www.ajen.dev">
  <img src="./assets/dashboard.png" alt="Ajen Dashboard showing AI employees building a startup" width="900"/>
  </a>
</p>

<p align="center">
  <a href="https://www.ajen.dev"><strong>Open Dashboard →</strong></a>
</p>


---

## Quick Start

```bash
# Clone & setup
git clone https://github.com/ajenhq/ajen.git && cd ajen
cp .env.example .env
# Configure your API keys in .env

# Run
cargo run --release
```

The CLI auto-installs `cloudflared` if needed, opens a tunnel, and prints everything you need:

```
  ┌─────────────────────────────────────────────────┐
  │  Ajen CLI v0.1.0                                │
  │                                                 │
  │  Secret:  ak_7f3a...b2c1                        │
  │  Local:   http://localhost:3000                  │
  │  Tunnel:  https://abc123.trycloudflare.com      │
  │                                                 │
  │  Connect: https://www.ajen.dev/cli_auth?url=... │
  │                                                 │
  │  Ready. Waiting for commands.                   │
  └─────────────────────────────────────────────────┘
```

Open your browser and connect using the link. The [ajen.dev](https://www.ajen.dev) dashboard connects to your local CLI through the tunnel — no port forwarding, no configuration.

---

## CLI Flags

| Flag | Default | Description |
|---|---|---|
| `--port` | `3000` | Local server port |
| `--no-tunnel` | off | Disable Cloudflare tunnel |
| `--no-open` | off | Don't auto-open browser |
| `--workspace-dir` | `./workspaces` | Directory for generated projects |
| `--manifests-dir` | built-in | Custom employee manifests directory |

---

## Features

<table>
<tr>
<td width="33%" valign="top">

### Company Hierarchy
Board (you) → CEO → CTO / CMO / COO → Developers, designers, writers. The Director orchestrates the entire flow from idea to deployment.

</td>
<td width="33%" valign="top">

### Plug-and-Play Employees
Each employee is a YAML manifest + persona file. Swap roles, add custom employees, share them with the community.

</td>
<td width="33%" valign="top">

### Multi-LLM Support
Claude, GPT, Gemini, or Ollama. Each employee can run on a different model — Sonnet for strategy, Haiku for execution.

</td>
</tr>
<tr>
<td width="33%" valign="top">

### Human-as-Board
The CEO generates a plan. You approve it before any work begins. You stay in control of what gets built.

</td>
<td width="33%" valign="top">

### Real-Time Events
Watch your company being built live at [ajen.dev](https://www.ajen.dev). Every tool call, LLM response, and cost is streamed over WebSocket.

</td>
<td width="33%" valign="top">

### Budget Controls
Per-employee and per-company cost tracking. Every LLM call records token usage and cost in cents.

</td>
</tr>
</table>

---

## Architecture

**Single binary** — Director, engine, API server, tunnel, and WebSocket all run in one Rust process on Tokio. The [ajen.dev](https://www.ajen.dev) dashboard connects to your local CLI via a Cloudflare tunnel — your code and API keys never leave your machine.

---

## Employee Manifest

Every employee is defined by a `manifest.yaml` and a `PERSONA.md`:

```yaml
# employee-manifests/ceo/manifest.yaml
apiVersion: ajen.dev/v1
kind: EmployeeManifest

metadata:
  id: "ceo"
  name: "Chief Executive Officer"
  version: "1.0.0"

spec:
  role: "ceo"
  tier: "executive"
  model:
    provider: "anthropic"
    model: "claude-sonnet-4-6"
  persona: "./PERSONA.md"
  tools:
    builtin:
      - "filesystem.read_file"
      - "filesystem.write_file"
      - "filesystem.list_directory"
  capabilities:
    canDelegateWork: true
    maxConcurrentTasks: 3
```

built-in roles: `ceo`, `cto`, `cmo`, `coo`, `fullstack_dev`, `frontend_dev`, `backend_dev`, `content_writer`, `designer`, `seo_specialist`, `devops`, `qa_engineer`, `social_media`, `data_analyst`. Roles are open strings — create any role you want.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Rust |
| Async Runtime | Tokio |
| HTTP Server | Axum |
| LLM Providers | Anthropic, OpenAI, Gemini, Ollama |
| Dashboard | [ajen.dev](https://www.ajen.dev) |
| Tunnel | Cloudflare Quick Tunnel (auto-installed) |
| Serialization | serde + serde_json + serde_yaml |
| IDs | UUID v4 |

---

## Project Structure

```
ajen/
  crates/
    ajen-core/          # Domain types, traits (EventBus, CompanyStore, LLMProvider, Tool)
    ajen-provider/      # LLM clients — Anthropic, OpenAI, Gemini, Ollama
    ajen-tools/         # Tool registry + filesystem tools (read, write, list)
    ajen-engine/        # Director, employee runtime, infra stores, manifests
    ajen-server/        # Axum HTTP + WebSocket + tunnel (the CLI binary)
  employee-manifests/   # 14 built-in employee definitions
    ceo/                #   manifest.yaml + PERSONA.md per role
    cto/
    fullstack-dev/
    ...
```

---

## Roadmap

- [x] Core Engine — ReAct loop, file tools, event bus
- [x] Multi-Provider — Anthropic, OpenAI, Gemini, Ollama
- [x] CLI with tunnel — secret auth, auto-install cloudflared, browser connect
- [x] Director — CEO planning, plan approval, team spawn, milestone execution
- [ ] Persistent Storage — SQLite-backed stores, data survives restarts
- [ ] Parallel Execution — concurrent tasks within milestones
- [ ] Container Isolation — sandboxed employee environments
- [ ] Plugin System — community employee manifests + custom tools

---

## Contributing

Ajen is in active development and contributions are welcome — bug fixes, new employee manifests, feature ideas.

1. Fork the repo
2. Create your branch (`git checkout -b feat/my-feature`)
3. Commit your changes
4. Open a pull request

See [open issues](https://github.com/ajenhq/ajen/issues) for things to work on.

---

## License

MIT — see [LICENSE](LICENSE) for details.

---

<div align="center">

Built with Rust, caffeine, and a company of AI employees.

**[Star this repo](https://github.com/ajenhq/ajen)** if you think AI should build companies, not just code.

</div>
