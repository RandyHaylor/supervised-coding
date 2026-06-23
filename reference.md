# supervised-coding — templates

Copy these shapes when running the flow. Keep every block scannable.

## Stage 0 — Offer block

```
This looks like a good fit for supervised-coding mode: <one-line reason>.
That means I'll plan it top-down, get your sign-off, offer naming options,
then build it up one small chunk at a time — pausing for your yes/no on each.

Use supervised-coding mode for this? (yes / no)
```

Record the answer. On "no", build normally and don't re-offer for this task.

## Stage 1 — Plan block

Interfaces and classes are required and come first.

```
## Plan: <feature name>

### Interfaces / contracts   (required, first)
- functionName(arg: Type) -> ReturnType   # what it promises
- type ShapeName = { field: Type }

### Classes / components   (required)
- ClassName — <responsibility>; relates to <other> by <relationship>

### User-story flow
1. <user does X> → <system does Y> → <result Z>

### Files & roles
- path/to/fileOne — <single responsibility>
- path/to/fileTwo — <single responsibility>

### Build order (chunks I'll produce, bottom-up)
1. <lowest-level unit>
2. <next layer>
3. <wiring / entrypoint>

Approve this plan, or tell me what to change.
```

## Stage 1b — Diagram delegation

Spawn once, then reuse the session for all updates.

```
Agent: name=drawio-diagrammer, run_in_background=true
Prompt:
  Read the bundled draw-io skill at <this-skill-dir>/draw-io/SKILL.md (and the
  section files it links). Create a .drawio diagram of this design so
  the user and I can use it as a shared whiteboard in the VS Code draw-io
  plugin. Diagram these interfaces/types/classes and their relationships:
  <paste the Stage 1 interfaces + classes>.
  Naming conventions to honor: verbose, purpose-driven names clear out of
  context; verbs for actions, nouns for things; name for purpose not
  implementation. Return the .drawio file path and a one-line summary.
```

- Save the returned agent name/session id. For later changes, **SendMessage to
  the same `drawio-diagrammer`** — don't spawn a new one.
- Review the returned diagram's labels against the naming conventions before
  accepting.
- Skip only if the user opts out or the change has no new types/relationships.

## Stage 2 — Naming options block

One block per name. Offer 2–4 candidates; the user picks or writes their own.

```
Name for <the thing it names>:
  A) candidateNameOne   — <why>
  B) candidateNameTwo   — <why>
  C) candidateNameThree — <why>
Pick A/B/C or type your own.
```

Naming conventions to honor:
- Verbose, purpose-driven, clear out of context.
- Verbs for actions, nouns for things.
- Name for purpose, not implementation.
- Flag any name that isn't a real convention for this codebase/platform.

## Stage 3 — Chunk block

```
### Chunk N of ~M: <chunk name>  (from build-order step N)

What it does:
- <succinct bullet>
- <succinct bullet>      # 1–3 sentences total, bullets not prose

<code for this chunk only>

Approve this chunk? (yes / revise / stop)
```

Rules of thumb:
- Size test: comprehensible at a glance AND explainable in 1–3 sentences. If not, split it.
- Explain in succinct bullets, not prose.
- Never advance to chunk N+1 without an explicit yes on chunk N.
- On "revise", re-present the same chunk; don't move on.
