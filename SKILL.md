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

> **THE SPRINT / FEATURE / VERTICAL SLICE AND OTHER VOCABULARY ARE CRITICAL TO AI
> AGENTS 'UNDERSTANDING' HOW TO BREAK UP CODING WORK AND MUST BE KEPT AND ADHERED
> TO. YOU MAY NOT WORK WITHOUT PLANNING WORK AS A SPRINT, EVEN IF IT'S A SMALL
> SPRINT WITH A COUPLE FEATURES.**

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
- **Where/how to document** (in priority order): an established user preference; else
  the **`source-of-truth-agent-tool`** skill (RandyHaylor) for requirements
  documentation and task tracking, if present; else the **default below**. If that
  skill isn't installed, **suggest installing it**.
- **Default doc location (when no preference and not using source-of-truth):** a
  `docs/` directory **inside the git repo**, one file per artifact —
  `docs/requirements.md`, `docs/architecture.md`, `docs/sprints.md` (the diagram
  `.drawio` also lives in the repo). Docs must be **durable files, never just chat
  scrollback** — the cascade rescan below depends on having real files to scan.
- **Required, ask once up front:** does the user want a **background agent to build
  out a running planning doc as you go?** State the token cost is non-trivial.
  - Yes → spawn one background agent for it; reuse that one session for every
    update (never re-spawn); feed it each stage's output as you go.
  - No → still write the durable docs above yourself; "lightweight" means terse,
    not absent.
- **If the user declined and the architecture starts getting complex, suggest it
  once more** at that point.

### When you change an earlier stage — MANDATORY, this is how documentation dies
- **Think out loud about the blast radius** before touching anything: e.g. "we're
  changing a requirement, but this does not change the architecture or the
  diagram — only the sprint list." Re-walk to the degree actually necessary.
- **But you STILL go through and SCAN every downstream document EVERY TIME to
  confirm it's still current** — requirements, architecture, the diagram, the
  sprint list, and the feature docs. Confirming "still accurate" is required work,
  not optional. Stale docs are how this whole process rots; do not let it.
- **Fan out: spawn multiple background agents in parallel** to scan/refresh the
  docs quickly so this stays cheap in time and primary context.
- Requirements and architecture should not change often. A stack/platform change
  (Stage 2) that doesn't touch requirements still **requires** revisiting
  architecture (Stage 3) before resuming the build.

## Stage 0 — Offer & record (always first)
- Don't enter silently. State the mode fits + one-line reason, ask yes/no.
- Record it. No → build normally, don't re-offer for this task.
- Direct `/supervised-coding` invocation = yes; skip the offer.
- **Git is required.** This skill needs at least a local git repo — for the
  documentation and rollback — even if no local code is written. In setup, ask:
  > "Do you want to create an online git repo to back up our progress as we go, or
  > just use a local-only git repo so we can isolate work in feature branches and
  > easily roll back code?"
  - **Assume git is needed. Never suggest having no repo.** Only skip the repo if
    the user, unprompted, explicitly says not to make one.

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
- Diagram via the diagramming agent (below); the diagram grows as the breakdown deepens.
- **Tech spikes belong here, in planning — not in a sprint.** A spike is a throwaway
  test to find out if something works, is compatible, or whether a theory holds. If
  there's any doubt about the architecture or stack up front, **dispel it with a
  spike**: assign it to a subagent while you keep planning. (A spike done inside a
  sprint isn't a spike — that's just developing.) Spikes may also be run between
  sprints if a new doubt appears.
- Documented output: the component map + module interfaces.

### Diagram delegation (default ON above the trivial bar)
- **The bar:** draw it whenever the design has more than a couple of pieces
  (roughly: 3+ functions/types/classes, or any relationship worth seeing). Below
  that bar, skip it.
- There is **no special agent type or agent file** for this — spawn an ordinary
  background agent (`run_in_background=true`) and **name it `drawio-diagrammer`** so
  it's addressable. In its prompt, point it to the bundled skill at
  `<this-skill-dir>/draw-io/SKILL.md` (read that file + its sections), then have it
  diagram the components/modules/interfaces + relationships.
- **As planning progresses, the main agent feeds new details to that same agent**
  so there's a single growing diagram to reference.
- **Keep its session id/name; reuse via SendMessage for all updates — never re-spawn.**
  Saves tokens, protects primary context, frees the primary agent to keep working.
- This is a **separate** background agent from the optional planning-doc agent;
  track the two session ids separately and never cross them.
- **Suggest the user open it in a draw-io editor** (VS Code extension or local
  draw.io app) so the `.drawio` is a shared, live whiteboard both can edit.
- **If no editor is available**, have the agent provide **PNG exports** for the
  user to look at. The agent itself should reference the **`.drawio` source**, not
  the PNG — the source is the better reference.
- Pass the naming rules in its prompt; review returned labels against them.
- Skip only if the user opts out or the design is below the bar above.

## Stage 4 — Vertical-slice planning (as real sprints)
- **A sprint is a vertical slice. A feature is not a vertical slice** — features are
  the tasks inside a sprint; the sprint as a whole is the slice that runs end-to-end.
- Define the slices as **actual sprints**, and the feature tasks as the Stage-3
  components **built out just enough to make that slice work**.
- A slice is **NOT "the absolute minimum you can do"** — it is **a comfortable
  short sprint of work, as if one dev were doing a one-week sprint.**
- Each slice runs **end-to-end and is deployable** ("anything built works").
- Phrase the work as sprints/features (required — it markedly improves how the
  work gets broken up). Document the ordered sprint/feature list.
- **No tech spikes in sprints.** Feasibility/compatibility doubts are resolved in
  planning (Stage 3) or between sprints — never folded into a sprint's work.

## Stage 5 — Build & deliver slices (iterate)
- Build one slice at a time, in small approval-gated chunks (see chunk rules above).
- State the easy/minimal vs the correct/proper approach when they differ; do the proper one.
- **End of each sprint** (a sprint ends when **all its feature tasks are done and
  the slice runs end-to-end** — not after each individual feature task) — run this in order:
  1. **Prove the vertical slice by real operation.** The deliverable is the actual
     solution working **end-to-end, provable, and usable** — not unit tests, not a
     description of what it would do. This does **not** mean "must have a UI": prove
     it through whatever interface the thing actually has. If it's a command-line
     tool, then after sprint 1 it had better at least run and show a `--help`
     section. Demonstrate it with real output / screenshot / runtime transcript.
     Do not shirk the real deliverable.
  2. **Update the docs** (cascade rule) so requirements/architecture/diagram/
     sprints/features stay current.
  3. **Commit** — even if it's just a local git repo. If there's more to push,
     **push before starting the next sprint.**
- Then confirm delivered and start the next sprint.

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
- **Lost background-agent session:** "never re-spawn" assumes the handle survives. If
  it's lost (context summary, restart, dead session), that's the one allowed
  exception — spawn a fresh agent, re-feed current state from the docs, tell the
  user, and continue. Don't stall waiting for a handle that's gone.
- **Testing** (unit/integration) is encouraged where it's a genuine value-add — it's
  up to the agent to suggest it to the user. It is **separate from** the Stage 5
  delivery proof, which is always required (real, end-to-end operation).
- Never claim a chunk or slice is tested unless you actually ran it.
- For any agent you spawn (diagramming, planning-doc, spikes, doc-scan), pass these
  naming & language rules into its prompt and review its returned work against them.
