> **🤖 Agents: read [`SKILL.md`](SKILL.md), not this file.** `SKILL.md` is the operative instruction set. This README is for humans evaluating or installing the skill.

# supervised-coding

A Claude Code [skill](https://code.claude.com/docs/en/skills) that makes an AI agent build code the way a careful senior pair would: **plan top-down, then build bottom-up in small, approval-gated chunks** — instead of emitting a whole implementation in one dump.

## Why

Large one-shot code generations are hard to review, hard to learn from, and easy to get subtly wrong. This skill forces the agent to slow down:

- You see the **design first** (interfaces and classes), and sign off before any code is written.
- You **name things** — the agent offers options, you pick or write your own.
- Code arrives **one digestible chunk at a time**, each explained in a sentence or two, each gated on your explicit yes/no.
- You end up understanding the code, not just receiving it — and the code tends to be better for having been planned.

## Flow

| Stage | What happens |
| :-- | :-- |
| **0 — Offer & record** | Agent states the mode fits and asks; never enters silently. |
| **1 — Plan** | Interfaces & classes **first** (required), then user-story flow, files/roles, build order. One approval, no code yet. |
| **1b — Diagram** | By default, a backgrounded `drawio-diagrammer` agent renders the design as a `.drawio` file (a shared whiteboard in the VS Code draw.io plugin). The naming pass runs in parallel while it builds. |
| **2 — Naming** | For each name, an array of 2–4 candidates with rationale; you pick or type your own. |
| **3 — Build** | One chunk per turn, sized to be comprehensible at a glance and explainable in 1–3 sentences. Teach → show → `yes / revise / stop`. |

## What's in here

```
supervised-coding/
├── SKILL.md       # operative instructions (the agent reads this)
├── reference.md   # copy-paste templates: offer, plan, diagram, naming, chunk
├── draw-io/       # bundled draw.io diagramming skill (self-contained, no plugin dependency)
└── README.md      # this file
```

The `draw-io/` subfolder is bundled so the diagramming agent can be pointed at a local path rather than depending on a separately installed plugin.

## Install

Clone into your personal skills directory so it's available across all your projects:

```bash
git clone https://github.com/RandyHaylor/supervised-coding.git ~/.claude/skills/supervised-coding
```

Or per-project: clone into a project's `.claude/skills/` instead.

## Use

- **Manually:** type `/supervised-coding` in Claude Code.
- **Automatically:** the agent may offer the mode when you ask it to build, implement, or scaffold non-trivial code — it will ask before engaging.

## License

MIT
