# supervised-coding — templates

Copy these shapes when running the flow. Keep every block scannable. Stage
numbers match `SKILL.md`.

## Stage 0 — Offer block

```
This looks like a good fit for supervised-coding mode: <one-line reason>.
That means we work in documented, approval-gated stages — requirements, stack,
architecture, sprint planning, then build & deliver slices — one small chunk at
a time with your yes/no on each.

Use supervised-coding mode for this? (yes / no)
```

Record the answer. On "no", build normally and don't re-offer for this task.

## Setup — git ask (required; git is assumed needed)

```
Do you want to create an online git repo to back up our progress as we go, or
just use a local-only git repo so we can isolate work in feature branches and
easily roll back code?
```

Never offer "no repo" as an option. Only skip if the user, unprompted, explicitly
refuses a repo. At minimum a local repo is required (for docs + rollback).

## Up-front — planning-doc agent ask (required)

```
Want a background agent to maintain a running planning doc as we go? It captures
each stage's output. Token cost is non-trivial. (yes / no)
```

If yes, spawn once and reuse its session (like the diagrammer). If the user
declined and the architecture later gets complex, ask once more.

## Stage 1 — Requirements block

```
## Requirements
- Functional requirements: <...>
- Technical constraints: <...>
- Primary-path user story: <user does X> → <system does Y> → <result Z>

Confirm / correct before we pick a stack.
```

## Stage 2 — Stack / platform block

```
## Stack options (driven by: <devices, features, extensibility needed>)
  A) <stack/lang/platform> — <tradeoffs; any early architecture choice it forces>
  B) <stack/lang/platform> — <tradeoffs>
Pick one or propose your own. (I'll document the choice + rationale.)
```

## Stage 3 — Architecture (breadth-first, top-down) + diagram

Map larger components, break toward deep modules, define each module's role,
interface, and I/O. Present one digestible piece at a time; name as you go.

```
## Architecture — level <N>
- Component / deep module: <name>
    role: <what it is / does>
    interface: <signatures it exposes>
    I/O: <in → out>
    isolation: <how it's testable on its own>
Confirm this level, then we go one level deeper.
```

Tech spikes: resolve feasibility/compatibility/architecture doubts here in
planning — assign to a subagent while you keep planning. Never inside a sprint.

Diagram delegation — no special agent type exists; spawn an ordinary background
agent, name it `drawio-diagrammer`, spawn once, reuse the session for all updates
(separate from the optional planning-doc agent — don't cross their sessions):

```
Agent: ordinary background agent, name=drawio-diagrammer, run_in_background=true
Prompt:
  Read the bundled draw-io skill at <this-skill-dir>/draw-io/SKILL.md (and the
  section files it links). Diagram these components / deep modules / interfaces
  and their relationships: <paste current architecture>. I'll send more as the
  plan grows — keep one growing diagram. Naming rules to honor: understandable
  out of context; verbs for actions, nouns for things; name for role, not the
  implementation it builds. Return the .drawio file path and a one-line summary.
```

- Feed new details to the **same** agent via SendMessage as planning progresses.
- Suggest the user open the `.drawio` in a draw-io editor (VS Code extension or
  local draw.io app) as a shared live whiteboard.
- If no editor: agent provides PNG exports for the user; the agent references the
  `.drawio` source (better than the image).
- Review returned labels against the naming rules.

## Naming options block (use wherever a name is introduced)

One block per name. Offer 2–4 candidates; the user picks or writes their own.

```
Name for <the thing it names>:
  A) candidateNameOne   — <why>
  B) candidateNameTwo   — <why>
  C) candidateNameThree — <why>
Pick A/B/C or type your own.
```

Naming rules to honor (full set in SKILL.md → "Naming & language rules"):
- Understandable out of context; promote readable code (SOLID).
- Verbs for actions, nouns for things.
- Name for role, not the implementation it builds — unless a hard framework
  convention makes the conventional name less confusing.
- Flag any name that isn't a real convention for this codebase/platform.

### Example — names that follow the rules

Encapsulate clever expressions behind role names; keep `main` reading like intent.

```python
# BAD — app-in-a-line; reduce/filter dumped as logic; nothing names the role
def main():
    print(reduce(lambda running, tx: running + tx["amount"],
                 filter(lambda tx: tx["status"] == "settled", load()), 0))

# GOOD — main states intent; the clever bits are encapsulated and named for role
def main():
    transactions = load_transactions_from_ledger()
    settled_revenue = total_settled_revenue(transactions)
    write_revenue_report(settled_revenue)

def total_settled_revenue(transactions):
    settled_transactions = [tx for tx in transactions if tx["status"] == "settled"]
    settled_amounts = [tx["amount"] for tx in settled_transactions]
    return sum(settled_amounts)
```

Why it passes:
- `main` is a sequence of well-named calls — a statement of intent, no plumbing.
- `total_settled_revenue` names the **role/result**, not "reduce" or "the loop".
- No implementation leaks through the name; reads correctly out of context.
- Not narrated English (`NowSumTheList`) and not a hieroglyphic one-liner.

## Stage 4 — Vertical-slice (sprint) plan block

```
## Sprints (vertical slices)
Sprint 1 — <feature name>: <the end-to-end thing a user can do after this>
  feature tasks (Stage-3 components built out just enough for this slice):
    - <component> → <built only as far as this slice needs>
  runs end-to-end & deployable: <how>
Sprint 2 — <feature name>: ...
```

Sizing: a slice is **NOT the absolute minimum** — it's a comfortable short sprint,
as if one dev did a one-week sprint. Phrase work as sprints/features. A sprint IS
the vertical slice; a feature is a task inside it, not a slice on its own.

## Stage 5 — Chunk block (build & deliver)

Work through the sprint's numbered feature tasks. A chunk is just how one feature
task is shown when it's too big for a single look — no separate chunk numbering.

```
### Sprint <N> · feature task <N>: <name>

What it does:
- <succinct bullet>
- <succinct bullet>      # 1–3 sentences total, bullets not prose

<code for this piece only>

Approve? (yes / revise / stop)
```

Rules of thumb:
- Size test: comprehensible at a glance AND explainable in 1–3 sentences. If not, split it.
- Explain in succinct bullets, not prose.
- Never advance without an explicit yes.
- End-of-sprint sequence (sprint ends when its feature ends): (1) prove the slice by
  **real operation** — the actual solution working end-to-end, provable and usable
  (not unit tests, not a description); prove it through whatever interface it has
  (a CLI should at least run and show `--help` after sprint 1); (2) update docs
  (cascade); (3) commit — even a local-only repo; push before the next sprint if
  there's more to push.
