# supervised-coding

> **🤖 Agents & 👤 humans:** this README is the high-level overview. **The operative
> procedure — every rule, every step — lives in [`SKILL.md`](SKILL.md).** Read that.

A Claude Code [skill](https://code.claude.com/docs/en/skills) that stops an agent
from dumping a whole implementation. It builds in documented, approval-gated stages so you:

- control the shape
- confirm as it goes
- learn the code instead of receiving a wall of it

---

### ▸ Slide 1 — The idea

Plan **breadth-first, top-down**. Build **depth-first, in vertical slices**.
Everything moves one small, digestible chunk at a time — explicit **yes / no** on each.
Required procedure, even for small tasks (skipping needs your permission).

---

### ▸ Slide 2 — The stages (top-down)

```
0. Offer & record        →  agent asks before engaging; never silent
1. Requirements          →  reqs, constraints, primary-path user story
2. Stack / platform      →  research & offer options; surface early arch choices
3. Architecture          →  deep modules behind interfaces; breadth-first, diagrammed
4. Sprint planning        →  vertical slices, sized like a one-week dev sprint
5. Build & deliver        →  slices, proven by REAL operation (not just unit tests)
```

---

### ▸ Slide 3 — The loops

```
Cascade:   change an early stage  →  re-walk every later stage, in order
Chunk:     each step  →  teach → show → (yes / revise / stop) → next
Slice:     build feature → prove it runs → commit → next feature
```

---

### ▸ Slide 4 — Packaged draw.io skill

The [`draw-io/`](draw-io/) subfolder is a **bundled, self-contained diagramming skill**.

- used by a background agent during architecture (Stage 3)
- builds one growing `.drawio` diagram of the design — a shared whiteboard
- open it in a draw.io editor (VS Code extension or local app)
- no separate plugin install required
- no editor? the agent exports PNGs for you to view

---

## Install

```bash
git clone https://github.com/RandyHaylor/supervised-coding.git ~/.claude/skills/supervised-coding
```

(Or clone into a project's `.claude/skills/` for that project only.)

## Use

- **Manually:** `/supervised-coding`
- **Automatically:** the agent offers the mode when you ask it to build something
  non-trivial — it asks before engaging.

## License

MIT
