---
name: supervised-coding
description: >-
  Build software in methodical, documented, approval-gated stages — never a big
  plan or a big code dump. Define requirements, then stack, then plan
  architecture breadth-first top-down (deep modules behind interfaces defined up
  front), then plan vertical slices as real sprints, then build and deliver each
  slice proven by real operation. Everything moves one small digestible chunk at
  a time with explicit yes/no. Use when asked to build, implement, or scaffold
  non-trivial code and the user wants to control the shape, confirm as it goes,
  and learn the code rather than receive a dump.
when_to_use: >-
  Trigger when asked to implement, build, write, or scaffold non-trivial code
  (feature, module, service, class, app, multi-file change). Phrases: "build",
  "implement", "write the code for", "scaffold", "let's pair on". Skip for
  one-line edits, typos, pure questions, or when the user asks for it all at once.
---

# Supervised coding

> **THIS DOC IS REQUIRED PROCEDURE, EVEN FOR SMALL TASKS. YOU ARE NOT AUTHORIZED
> TO SKIP THESE STEPS WITHOUT EXPLICIT USER PERMISSION. IF SKIPPING STEPS IS
> REQUIRED OR OBVIOUSLY BENEFICIAL, YOU MUST GET EXPLICIT PERMISSION.**

Plan breadth-first top-down; build depth-first in vertical slices. Everything
happens in small, approval-gated chunks — never dump a big plan or big code.
Load `reference.md` for templates before starting.

## How every chunk is delivered (applies to all stages)
- One chunk per turn, explained in **succinct bullets, not prose**.
- Chunk size: comprehensible at a glance AND explainable in 1–3 sentences. Too big → split.
- Teach what it is → show it → stop and ask: **yes / revise / stop**.
- `yes` → apply, continue. `revise` → re-present same chunk. Never advance without a yes.

## Cascade & documentation rule (governs all stages)
- Each stage produces a **documented** artifact.
- **Required, ask once up front:** does the user want a **background agent to build
  out a running planning doc as you go?** State the token cost is non-trivial.
  - Yes → spawn one (reuse its session like the diagrammer; never re-spawn); feed
    it each stage's output as you go.
  - No → keep documentation lightweight and inline.
- **If the user declined and the architecture starts getting complex, suggest it
  once more** at that point.
- Change anything in an earlier stage → back up and re-walk every later stage, in order.
- Change stack/platform (Stage 2) but not requirements → must revisit architecture
  (Stage 3) before resuming the build.

## Stage 0 — Offer & record (always first)
- Don't enter silently. State the mode fits + one-line reason, ask yes/no.
- Record it. No → build normally, don't re-offer for this task.
- Direct `/supervised-coding` invocation = yes; skip the offer.

## Stage 1 — Requirements
- Methodically define and document: functional requirements, technical
  constraints, and at least the primary-path (happy-path) user story.
- Drive these out with the user; ask where unknown — don't assume.
- Documented output before moving on.

## Stage 2 — Stack / platform
- From the requirements (target devices, features, extensibility to support),
  research and offer the best stack / language / platform / tooling options,
  with tradeoffs.
- If a choice forces an early architecture decision, say so explicitly.
- User picks; document the decision and rationale.

## Stage 3 — Architecture (breadth-first, top-down planning)
- Identify the larger logical components first.
- Work to break them down into **deep modules** (simple interface, substantial
  hidden implementation).
- For each module, define its **role, interface, and I/O up front**.
- Design so modules are **testable in isolation** wherever possible.
- Name modules/interfaces here per the Naming & language rules; offer options.
- Diagram via the draw-io agent (below); the diagram grows as the breakdown deepens.
- Documented output: the component map + module interfaces.

### Diagram delegation (default ON when structure is more than trivial)
- Spawn agent `drawio-diagrammer`, `run_in_background=true`; in its prompt point it
  to the bundled skill at `<this-skill-dir>/draw-io/SKILL.md` (read that file + its
  sections), then have it diagram the components/modules/interfaces + relationships.
- **As planning progresses, the main agent feeds new details to the same spawned
  agent** so there's a single growing diagram to reference.
- **Keep its session id/name; reuse via SendMessage for all updates — never re-spawn.**
  Saves tokens, protects primary context, frees the primary agent to keep working.
- **Suggest the user open it in a draw-io editor** (VS Code extension or local
  draw.io app) so the `.drawio` is a shared, live whiteboard both can edit.
- **If no editor is available**, have the agent provide **PNG exports** for the
  user to look at. The agent itself should reference the **`.drawio` source**, not
  the PNG — the source is the better reference.
- Pass the naming rules in its prompt; review returned labels against them.
- Skip only if the user opts out or there's no meaningful structure to draw.

## Stage 4 — Vertical-slice planning (as real sprints)
- Define the slices as **actual sprints**, and the feature tasks as the Stage-3
  components **built out just enough to make that slice work**.
- A slice is **NOT "the absolute minimum you can do"** — it is **a comfortable
  short sprint of work, as if one dev were doing a one-week sprint.**
- Each slice runs **end-to-end and is deployable** ("anything built works").
- Phrase the work as sprints/features (required — it markedly improves how the
  work gets broken up). Document the ordered sprint/feature list.

## Stage 5 — Build & deliver slices (iterate)
- Build one slice at a time, in small approval-gated chunks (see chunk rules above).
- A slice is done only when **delivery is proven by real operation** — output,
  screenshot, runtime demo, etc. **NOT just unit tests.**
- State the easy/minimal vs the correct/proper approach when they differ; do the proper one.
- **Commit after every feature is complete** — even if it's just a local git repo.
- After each slice: confirm delivered, then start the next.

## Naming & language rules

Language:
- Speak plainly. Normal, comprehensible English over jargon.
- Do **not** invent project-specific jargon, coined terms, or a private vocabulary.
- Don't reach for insider words (e.g. "dogfooding") when plain words say it.

Naming:
- Names follow SOLID: understandable out of context, promote readable code.
- Name for **role / what a thing is or does — NEVER for the implementation it's
  used to build**. Exception: a hard framework convention where the conventional
  name causes *less* confusion than a "correct" one (e.g. web route patterns; the
  traditional `map`/`filter`/`reduce`, which don't actually mean what they say).
- Where syntactic sugar or a bad-but-standard name adds confusion, **encapsulate
  it**: wrap the clever expression in a function named for its role; don't leak
  internals an interface shouldn't expose.
- No "app-in-a-line": never dump chained map/filter/reduce (or similar) as logic
  in place. Give it a named home. (Genuinely standard one-liners are fine — don't
  apply this rule where it harms readability.)
- Match level of detail to the level of the code. Top-level / `main` /
  orchestration reads like a statement of intent — a sequence of well-named
  calls, never low-level functional plumbing.
- But not literal English narration: name the thing or role, not the step
  (`load_user_records`, not `NowLoadTheDict`).
- Don't be edgy where it doesn't belong and then sloppy where naming matters.
  Aim: readability + abstraction level appropriate to the current script.

## Other rules
- Never claim a chunk or slice is tested unless you actually ran it.
- When you spawn the draw-io agent (or any agent), pass these naming & language
  rules into its prompt and review its returned work against them.
