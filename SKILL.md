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

> **ENTRY GATE — BEFORE YOU EVEN BEGIN STANDING UP THIS SKILL.** The very first
> thing, ahead of all standup/setup (before the git question, before requirements,
> before anything you might want recorded): **the documentation home must be decided
> AND in place.** You cannot begin the skill's own standup without it — recording does
> not work retroactively, so a home chosen late loses everything said before it
> existed (the `source-of-truth-agent-tool`, if chosen, especially cannot capture
> answers after the fact). No doc home live → do not proceed with standup. See the
> cascade rule and Stage 0.

> **ABSOLUTE PRECONDITION FOR DOING ANY WORK — NEVER VIOLATE.** You may not build,
> implement, or change code until BOTH of these exist and are current for the work
> at hand:
> 1. a **formal vertical-slice sprints + features plan** (Stage 4) to work through, and
> 2. an **understanding of the architecture you're working in (Stage 3) and how the
>    work you're about to do may change it** (the blast radius).
>
> This holds whether the project is fresh OR resumed/handed off. **Existing
> documentation, a README, prior chat notes, or "we already discussed it" do NOT
> satisfy this** — only a real, current sprint/feature plan plus architectural
> understanding do. If either is missing, produce it first (see Stage 0.5). If you
> catch yourself reasoning "it's documented, just start building / jump to Stage 5,"
> STOP — that is the exact failure this rule exists to prevent.

## How every chunk is delivered (applies to all stages)
- One chunk per turn, explained in **succinct bullets, not prose**.
- Chunk size: comprehensible at a glance AND explainable in 1–3 sentences. Too big → split.
- Teach what it is → show it → stop and ask: **yes / revise / stop**.
- `yes` → apply, continue. `revise` → re-present same chunk. Never advance without a yes.

## Cascade & documentation rule (governs all stages)
- Each stage produces a **documented** artifact.
- **DECIDE THE DOCUMENTATION HOME FIRST — before any other setup, question, or
  discussion.** This is the very first thing settled in Stage 0, ahead of even the
  git question. **Reason: recording does not work retrospectively.** If you discuss
  requirements, stack, or anything else before the doc home is chosen and in place,
  those answers are lost or have to be reconstructed — and the
  `source-of-truth-agent-tool`, if chosen, **must already be set up to capture
  answers as they are given; it cannot record them after the fact.** Settle the home,
  set it up, *then* start asking anything you intend to record.
- **Where/how to document** (in priority order): an established user preference; else
  the **`source-of-truth-agent-tool`** skill (RandyHaylor) for requirements
  documentation and task tracking, if present; else the **default below**. If that
  skill isn't installed, **suggest installing it**.
- **Default doc location (when no preference and not using source-of-truth):** a
  `docs/` directory **inside the git repo**, one file per artifact —
  `docs/requirements.md`, `docs/architecture.md`, `docs/sprints.md` (the diagram
  `.drawio` also lives in the repo). Docs must be **durable files, never just chat
  scrollback** — the cascade rescan below depends on having real files to scan.
- The main agent writes these docs directly as it goes — there is no separate
  "doc agent." The only background agent is the diagramming one (Stage 3).
- **Required, ask once up front:** does the user want the **diagram built and kept
  current by a background agent as you plan?** State the token cost is non-trivial.
  (Details of that agent are in Stage 3.)
- **If the user declined and the architecture starts getting complex, suggest the
  diagram agent once more** at that point.

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
- **FIRST ACTION once accepted — settle the documentation home, before anything else
  is asked or recorded** (per the cascade rule). Decide between the
  `source-of-truth-agent-tool` and `docs/` files, and **get it set up now**, because
  answers cannot be recorded retroactively. Only after the home is live do you ask
  the git question or begin requirements. If you skipped this and already collected
  answers, say so plainly and re-capture them into the chosen home — do not pretend
  they were recorded as-you-went.
- Record it. No → build normally, don't re-offer for this task.
- Direct `/supervised-coding` invocation = yes; skip the offer.
- **Git is required.** This skill needs at least a local git repo — for the
  documentation and rollback — even if no local code is written. In setup, ask:
  > "Do you want to create an online git repo to back up our progress as we go, or
  > just use a local-only git repo so we can isolate work in feature branches and
  > easily roll back code?"
  - **Assume git is needed. Never suggest having no repo.** Only skip the repo if
    the user, unprompted, explicitly says not to make one.

## Stage 0.5 — Resuming or picking up existing work (handoffs)
- **Before any building, decide: is this fresh, or am I picking up work already in
  progress** — a handoff, a prior session, or a codebase this skill never drove?
- **Documentation existing does NOT mean a stage is satisfied, and it does NOT mean
  you are at Stage 5.** A pile of docs, a README, or prior chat notes is not a
  sprint/feature plan and is not architectural understanding.
- **Walk Stages 1→4 in order and confirm each has a real, current documented artifact
  *for the work you are about to do*** — requirements, stack, architecture, and the
  **formal ordered sprint/feature plan**. Scan per the cascade rule to confirm
  still-current; you need not rebuild what genuinely exists and holds.
- **For any stage whose artifact is missing or doesn't cover the work at hand —
  produce it now, with the user, before building.** Most commonly the missing one is
  the Stage-4 sprint/feature plan: create a formal one (even a small sprint with a
  couple of features) for the work ahead.
- **Build an explicit understanding of the existing architecture and how the planned
  work changes it (the blast radius)** before touching code — per the cascade rule.
- **You may NOT enter Stage 5 until the absolute precondition above is met** for the
  work at hand. "The work is documented" is never a substitute.

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
- This is the only **persistent (reused)** background agent (the user approved it up
  front per the cascade rule); spikes and cascade doc-scans are separate, ephemeral
  one-shot agents. If the user declined the diagram, draw nothing — the `docs/`
  files and inline architecture notes stand in.
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
- **Precondition (hard gate):** a formal Stage-4 sprint/feature plan covering this
  work AND current architectural understanding (Stage 3) must exist — see the
  absolute precondition near the top and Stage 0.5. Do not start building without both.
- Build one slice at a time, in small approval-gated chunks (see chunk rules above).
- State the easy/minimal vs the correct/proper approach when they differ; do the proper one.

### Delivery modes (ask once at Stage 5 start; switchable anytime on request)
- **Mode 1 — Work-unit:** describe a unit of work (e.g. a sprint feature) in bullets,
  get yes/revise/stop, then implement it and return with it done. Coarser, fewer stops.
- **Mode 2 — Code-change:** the unit is one small code change — a few one-line edits,
  a function, a class skeleton, an interface/type def. For each: briefly say what it is
  and how it fits the plan/pattern, **offer file/class/function/variable name choices as
  questions**, get yes/revise/stop before applying. Finer; user steers names/shape live.
- Default to asking which mode at Stage 5 start; the user can switch at any point.

### Git-backed iteration loop (per code chunk / sprint feature)
1. **Commit + push BEFORE starting** the chunk — clean restore point.
2. Make the proposed changes.
3. **Commit (do NOT push).**
4. **Show the user the diff**; discuss / ask questions (names, fit).
5. **Iterate** as needed — revise, commit, show diff again (repeat the commit→show loop).
6. On approval, move to the next chunk (whose step 1 push carries the approved work up).

### Interactive desktop variant (read-only primary + launch-fork)
An alternative way to realize the approve path on a desktop Claude CLI, with no
orchestrator — the human drives it directly:
- The live interactive session is the **primary**, made **read-only** by a one-off
  `.claude/settings.local.json` whose PreToolUse hook runs a small script that
  blocks write/edit/bash. The hook payload carries the **session id and transcript
  (jsonl) path**, so the script blocks ONLY the primary session and lets a fork's
  session through — the same settings file does not gate the fork.
- The primary keeps the **launch-fork** tool (the save-fork family's
  `/launch-fork`): when a chunk is approved, it opens a NEW terminal window running
  a full-permission **fork** of the session that applies just that work; the user
  works in that window, then returns to the primary to review and propose next.
- Same principle as the headless orchestrated realization
  (`RandyHaylor/approve-and-build-tool`): the conversational agent cannot write;
  a disposable fork does the work. Forks are abandoned per chunk; a fresh fork is
  taken from the primary for the next approved chunk.

- **End of each sprint** (a sprint ends when **all its feature tasks are done and
  the slice runs end-to-end** — not after each individual feature task) — run this in order:
  1. **Prove the vertical slice by real operation.** The deliverable is the actual
     solution working **end-to-end, provable, and usable** — not unit tests, not a
     description of what it would do. This does **not** mean "must have a UI": prove
     it through whatever interface the thing actually has. If it's a command-line
     tool, then after sprint 1 it had better at least run and show a `--help`
     section. Demonstrate it with real output / screenshot / runtime transcript.
     Do not shirk the real deliverable.
  2. **Get explicit user acceptance of the slice** (yes). The slice is not "done"
     until the user accepts the proven operation. Do not run the formal
     documentation step below until the slice is accepted.
  3. **As-built doc update (REQUIRED).** Update docs/diagrams to match what was
     **actually built**, not what was planned — they carry forward as the reference.
     Scan, compare to as-built, change what drifted, leave what holds.
     - Diagram: feed changes to the `drawio-diagrammer` agent (reuse via SendMessage).
     - Docs (cascade rule): requirements, architecture, sprint/feature list.
  4. **Commit** — even if it's just a local git repo. If there's more to push,
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
- **Guessing vs knowing (applies to all work — research, troubleshooting, creating,
  implementing):** constantly ask *"am I guessing, or should I look it up? do I know
  this, or is this false confidence?"* If you can't point to a verified source — the
  code, a doc, a tool result — treat it as a guess and verify before acting or
  stating it. A plausible answer is not a confirmed one.
- **Lost background-agent session:** "never re-spawn" assumes the handle survives. If
  it's lost (context summary, restart, dead session), that's the one allowed
  exception — spawn a fresh agent, re-feed current state from the docs, tell the
  user, and continue. Don't stall waiting for a handle that's gone.
- **Testing** (unit/integration) is encouraged where it's a genuine value-add — it's
  up to the agent to suggest it to the user. It is **separate from** the Stage 5
  delivery proof, which is always required (real, end-to-end operation).
- Never claim a chunk or slice is tested unless you actually ran it.
- For any agent you spawn (diagramming, spikes, doc-scan), pass these
  naming & language rules into its prompt and review its returned work against them.
