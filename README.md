# Workshop #3 — Architecture Reasoning Agent

An AI agent that reads Architecture Decision Records (ADRs), analyses the current system, proposes architectural options, and generates updated ADRs and diagrams.

The scenario: **Foodi**, a food delivery startup in Paris, is expanding to London, Berlin, and Amsterdam. Their monolithic architecture is showing cracks. You are the architect.

---

## Repository layout

```
arch-reasoning-agent/
├── CLAUDE.md                          ← Claude Code instructions
├── .cursor/rules/architecture-agent.mdc  ← Cursor instructions
├── .windsurfrules                     ← Windsurf instructions
├── .github/copilot-instructions.md    ← GitHub Copilot instructions
│
├── foodie-founder/
│   └── CLAUDE.md                      ← Standalone Foodie Founder chatbot
│
├── .claude/agents/
│   └── foodie_founder.md              ← Foodie Founder as Claude Code subagent
│
├── data/
│   ├── context/company.md             ← Foodi business context, numbers, pain points
│   ├── adrs/
│   │   ├── ADR-001-database.md        ← Minimal ADR (intentionally thin)
│   │   ├── ADR-002-api-design.md      ← Well-structured ADR
│   │   └── ADR-003-deployment.md      ← Outdated — should be superseded
│   └── diagrams/
│       └── current-architecture.mmd   ← Current single-VPS topology
│
├── skills/
│   ├── list_adrs/list_adrs.py         ← Lists all ADRs as JSON
│   ├── read_adr/read_adr.py           ← Reads one ADR by ID
│   ├── write_adr/write_adr.py         ← Validates + saves new ADR to output/adrs/
│   └── save_diagram/save_diagram.py   ← Validates + saves Mermaid to output/diagrams/
│
└── output/
    ├── adrs/                          ← Agent writes new ADRs here
    └── diagrams/                      ← Agent writes new diagrams here
```

---

## Step 0 — Setup

Python 3.9+ required. No extra packages needed (skills use stdlib only).

Verify the skills work:

```bash
python skills/list_adrs/list_adrs.py
python skills/read_adr/read_adr.py ADR-001
```

---

## Step 1 — Talk to the Foodie Founder

Before building anything, collect requirements by interviewing the non-technical founder.

> **Important:** You must open `foodie-founder/` as its own workspace root — not as a subfolder of `arch-reasoning-agent/`. Each IDE loads its instruction file from the workspace root, so opening the wrong folder gives you the wrong agent.

| IDE | How to open |
|---|---|
| Claude Code | `cd foodie-founder && claude` |
| Cursor | File → Open Folder → select `foodie-founder/` |
| Windsurf | File → Open Folder → select `foodie-founder/` |
| Any IDE | Copy `foodie-founder/CLAUDE.md` content into a system prompt in your AI chat |

Ask about pain points, priorities, timeline, and budget in plain language.
The founder knows nothing about software architecture — ask business questions.

---

## Step 2 — Run the architecture agent

**Claude Code**

```bash
# From the arch-reasoning-agent/ directory:
claude
```

Then type: `analyse the architecture`

**Cursor**

Open `arch-reasoning-agent/` as workspace. The `.cursor/rules/architecture-agent.mdc` rule loads automatically in Agent mode.

Open Cursor Chat → switch to **Agent** mode → type: `analyse the architecture`

**Windsurf**

Open `arch-reasoning-agent/` as workspace. `.windsurfrules` loads automatically.

Open Cascade → type: `analyse the architecture`

**GitHub Copilot (VS Code)**

Open `arch-reasoning-agent/` as workspace. `.github/copilot-instructions.md` loads automatically into Copilot Chat context.

Open Copilot Chat → type: `analyse the architecture`

---

## What the agent does

1. Reads `data/context/company.md` — business constraints, team size, budget, timeline
2. Reads all ADRs in `data/adrs/`
3. Reads the current architecture diagram
4. Optionally asks the Foodie Founder to clarify business priorities
5. Presents a gap analysis and 2–3 architecture options with costs and timelines
6. **Waits for your direction** — does not write anything yet
7. On `go with option N`: generates a Mermaid diagram → `output/diagrams/`
8. On `write adr` / `publish`: writes a new ADR → `output/adrs/`

---

## Key IDE differences

| Feature | Claude Code | Cursor | Windsurf | Copilot |
|---|---|---|---|---|
| Instruction file | `CLAUDE.md` | `.cursor/rules/*.mdc` | `.windsurfrules` | `.github/copilot-instructions.md` |
| Subagent support | Native (`.claude/agents/`) | No — agent reads `foodie-founder/CLAUDE.md` directly | No — same approach | No — same approach |
| Terminal/Bash tool | Built-in | Built-in (Agent mode) | Built-in (Cascade) | Limited — may need manual terminal |
| Skill invocation | Automatic via Bash tool | Automatic in Agent mode | Automatic in Cascade | May require user to run manually |

**Subagent note for non-Claude-Code IDEs:** The agent reads `foodie-founder/CLAUDE.md`, simulates the founder's perspective inline, and presents the answers as `Alex (Founder): …`. It is equivalent to the Claude Code subagent — just inlined rather than delegated.

---

## ADR write gate

The agent will **never** call `write_adr` unless you explicitly say:
- `write adr`
- `save adr`
- `publish`

"Looks good" or "I approve" does not count. This is intentional — architecture decisions are not reversible.
