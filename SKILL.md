---
name: supervised-coding
description: >-
  Build code top-down in small, approval-gated chunks instead of emitting a
  whole implementation at once. Plan interfaces and classes first, diagram them,
  get sign-off, offer named options, then build up one digestible chunk at a
  time — teaching each and waiting for explicit yes/no. Use when asked to write
  a feature, module, class, or any non-trivial code and the user wants to stay
  in control, review as it goes, and learn the code rather than receive a dump.
when_to_use: >-
  Trigger when asked to implement, build, write, or scaffold non-trivial code
  (feature, module, service, class, multi-file change). Phrases: "build",
  "implement", "write the code for", "scaffold", "let's pair on". Skip for
  one-line edits, typos, pure questions, or when the user asks for it all at once.
---

# Supervised coding

Plan top-down, build bottom-up, one small reviewable chunk at a time. Each chunk
teaches then stops for a yes/no. Never dump a whole implementation.
Templates: load `reference.md` before the first plan.

## Stage 0 — Offer & record (always first)
- Don't enter silently. State that the mode fits + one-line reason, ask yes/no.
- Record it. No → build normally, don't re-offer for this task.
- Direct `/supervised-coding` invocation = yes; skip the offer.

## Stage 1 — Plan (one approval, no code yet)
- **Interfaces/contracts — required, FIRST**: signatures, types, data shapes. No bodies.
- **Classes/components — required**: structures + relationships.
- User-story flow: user does X → system does Y → result Z.
- Files & roles: each file + its single responsibility.
- Build order: bottom-up chunk sequence.
- Keep under a minute to read. Revise until approved; no coding until signed off.

## Stage 1b — Diagram (default: delegate)
- Default ON when design has >trivial structure (even a few functions/types/classes).
- VS Code draw-io plugin = shared whiteboard; both user and agent edit the live `.drawio`.
- Spawn agent `drawio-diagrammer`, `run_in_background=true`; in its prompt, point
  it to the bundled draw-io skill at this skill's own subfolder
  `<this-skill-dir>/draw-io/SKILL.md` (read that file + its sections), then have
  it diagram the planned interfaces/types/classes + relationships.
- **Keep its session id/name.** Reuse via SendMessage for all later diagram
  updates — never re-spawn. Saves tokens, protects primary context, frees
  primary agent to keep working/talking.
- Pass naming conventions in its prompt; review returned labels against them.
- **While the diagram builds in the background, run the Stage 2 naming pass with
  the user in parallel** — don't idle. Feed agreed names to the diagrammer via
  SendMessage so the diagram reflects the final names.
- Skip only if user opts out or change has no new types/relationships.

## Stage 2 — Naming pass
- Surface every name the user may care about (types, classes, key functions, modules, public fields).
- Per name: array of 2–4 candidates, one-line rationale each; user picks or types own.
- Lock chosen names; reuse verbatim.
- Conventions: verbose, purpose-driven, clear out of context; verbs=actions,
  nouns=things; name for purpose not implementation. Flag invented non-conventions.

## Stage 3 — Build up, one chunk per turn
Chunk size rule: each chunk must encapsulate exactly what the user can **see and
comprehend at a glance** and that you can explain in **1–3 sentences**. If it
needs more, it's too big — split it. (This, not "smaller," is the test.)
1. Say what it is — succinct bullets, 1–3 sentences max; teach it.
2. Show only this chunk's code.
3. Stop, ask: yes / revise / stop.
- yes → apply, next chunk. revise → re-present same chunk. Never advance without yes.

## Rules
- One chunk per turn; size by comprehensibility (see Stage 3 rule), not line count.
- **Explain in succinct bullets, NOT full prose, wherever possible** — every stage.
- Teach before showing.
- State easy/minimal vs correct/proper approach when they differ; do the proper one.
- Scope change mid-build → return to Stage 1 for quick re-approval.
- Never claim a chunk is tested unless you ran a test.
