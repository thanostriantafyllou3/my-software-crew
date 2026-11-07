# my-software-crew

A small crew for Claude Code, for any project (research, production software, side projects): a planner and an adversary who argue a plan into shape, reviewers at every stage (plan, code, outputs, before sharing, real use), and the habits that keep a long project resumable and the human able to follow it. Packaged as a Claude Code plugin (`crew`) plus my user-level instructions.

## What's in it

| | What it does |
|---|---|
| `crew:plan-loop` (skill) | Planner writes `PLAN.md`; a fresh adversary attacks it; revise until no blockers; open decisions go to the user as background / options / recommendation |
| `crew:review-step` (skill) | After a build step: quality, robustness and plan-conformance reviewers in parallel; triage every finding (fix / decline / decide) and answer it |
| `crew:prior-art` (skill) | Research current practice online (docs, papers, repos, write-ups) before proposing; bring back sourced options |
| `crew:catch-up` (skill) | What changed since the user last looked, in plain words sized to a few minutes: decisions made for them, what was measured, surprises, what needs them, where to read |
| `crew:resume` (skill) | Pick up a project from its message board and plan, check they still hold, propose the next step |
| `planner`, `plan-adversary`, `researcher`, `investigator` (agents) | The plan roles; a researcher looks outside (current practice online), an investigator looks inside (existing code, data, infrastructure, logs, failures), both read-only and citing sources |
| `quality-reviewer`, `robustness-reviewer`, `conformance-reviewer`, `output-reviewer` (agents) | The review roles; the first three read the code, the output reviewer sees only what the code produces; they write reports, never edit code |
| `release-reviewer` (agent) | Before anything is shared (paper, release, deploy, demo): claims follow from evidence, nothing leaks, reproducible, limitations stated, holds up when attacked |
| `refactorer` (agent) | Applies triaged findings without changing behaviour, proven by tests and before/after comparison |
| `plan-loop/templates/` | `PLAN.md` and `MESSAGEBOARD.md` skeletons |
| `user/CLAUDE.md` | How I work: decisions, explanations, building habits. Linked by hand (plugins can't ship user instructions) |

## Roles

- **Orchestrator: the main session.** Holds the picture, talks to the user, writes briefs, launches agents, checks what they return, and keeps state in `PLAN.md` and `MESSAGEBOARD.md` so any session can take over. It never lets an agent grade its own work, and it keeps the user able to follow: plain reports sized to their attention, so the work never becomes a black box.
- **Builder: usually the main session**, which knows why the code is the way it is.
- **Planner, adversary, researcher, investigator, reviewers, refactorer: fresh agents** with a brief and a narrow job. Reviewers see the work, not the author's reasoning; the refactorer gets precise rules instead of the whole history.

Cycle: prior-art → plan-loop → build a step → look at its outputs → review-step (code reviewers, plus an output reviewer when the step produces data) → refactorer → next review checks the fix. Codex, if installed, complements the plan review, the output review and red-team passes.

Review points, the same for research, production and side projects (each by a fresh reviewer that did not do the work; Codex, if installed, adds a second opinion at the starred ones):

| Stage | Reviewer sees | Looks for | Skill or agent |
|---|---|---|---|
| Prior work and existing state | the question; the code, data, infrastructure and past runs it touches | what others already do (outside); how what we have actually works and what constrains us (inside) | `prior-art` (researcher), `investigator` |
| Plan* | the plan, notes, worked examples | wrong assumptions, what the design would produce, untestable goals, wrong baselines, gaps | `plan-loop` (plan-adversary) |
| Code, after each step | code, tests, plan | quality and structure, bugs, plan conformance | `review-step` (three code reviewers) |
| Outputs* | only what the work produces (results, data, behaviour), and the goal | degenerate, guessable, too-good-to-be-true, unrealistic or contradictory results | `review-step` (output-reviewer) |
| Before sharing* | exactly what will be shared (paper, release, deploy, demo) and its evidence | unsupported claims, leaks, not reproducible, unstated limitations, ways to break it | `review-step` (release-reviewer) |
| Real use* | logs or traces of real runs or users, and the goal | unearned success, failures that were not the user's fault, behaviour that shows the design is wrong | `review-step` (output-reviewer) |

The checks on claims draw on the [NeurIPS paper checklist](https://neurips.cc/public/guides/PaperChecklist) (claims, limitations, reproducibility, error bars) and apply equally to a README's promises or a release's benchmark.

Investigators are also used when something fails (a crashed run, a wrong result, a production incident): they reconstruct what happened from logs and find the cause before anyone fixes it. And when a decision needs a fact nobody has checked.

Loops run until they converge: no blockers and no new majors. A round cap is only a safety net; hitting it means the loop is not converging (reviewers always find something, or two fixes undo each other), and the orchestrator escalates to the user instead of stopping silently.

### Decision rights

| Tier | Examples | The orchestrator… |
|---|---|---|
| Never decides | Uploads, launches, publishing, spending money, production writes; changing goal or scope; anything irreversible or touching other people | prepares it, then stops and asks |
| Decides, then reports | Reversible choices inside the plan's bounds: triage, code structure, retries, an extra review round | acts and logs the decision with its reason |
| Escalates when unsure | Options close in value, evidence conflicts, the plan is silent, or the choice would change what the plan states | stops and brings background, options and a recommendation |

Abstaining is a correct answer when the orchestrator does not know.

## Install

```
claude plugin marketplace add ~/code/my-software-crew      # or <github-user>/my-software-crew
claude plugin install crew@my-software-crew                # user scope: every project
ln -s ~/code/my-software-crew/user/CLAUDE.md ~/.claude/CLAUDE.md
```

A local-path marketplace loads in place: edits apply in the next session or after `/reload-plugins`. From GitHub: push, then `claude plugin marketplace update my-software-crew` and `claude plugin update crew@my-software-crew` (bump `version` in `plugins/crew/.claude-plugin/plugin.json`). Check with `claude plugin validate ./`.

## Use

- `/crew:prior-art` when a question has likely been solved elsewhere.
- `/crew:plan-loop` before building anything non-trivial.
- `/crew:review-step` after each build step and before shipping.
- `/crew:catch-up` after a break, or before deciding on work you haven't seen.
- `/crew:resume` at the start of a session on an existing project.

## Principles

- State lives in files (`PLAN.md`, `MESSAGEBOARD.md`), not in the conversation, and is committed at every checkpoint so any step can be undone.
- Reviewers are fresh agents that see the work, not the author's reasoning.
- The human's attention is the bottleneck, not the agents' output. Comprehension is a deliverable: if the user can't follow the work, they can't check it ("comprehension debt").
- The user owns every decision and every outward-facing action, and gets honest pushback when they are wrong.
- Look up current practice before proposing; cite it.
- Measure before trusting a model; keep the measurement as a test.
- Grow this crew from what proved useful in real projects; don't invent roles in advance.
