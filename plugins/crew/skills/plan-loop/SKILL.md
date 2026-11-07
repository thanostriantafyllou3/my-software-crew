---
name: plan-loop
description: Plan a non-trivial piece of work with a planner and a fresh adversarial reviewer in rounds until no blockers remain, then bring the open decisions to the user. Use before building anything that takes more than a few steps, or when the user asks for a plan.
---

# Plan loop

A plan written and checked by the same mind keeps its blind spots. Here a planner writes it and a fresh adversary attacks it, in rounds, with the project's files as the only shared state.

## Setup

- `PLAN.md` and `MESSAGEBOARD.md` in the project or sub-project folder; if missing, copy them from this skill's `templates/` folder.
- Gather what the plan depends on first, from two directions, in parallel where possible:
  - **Inside (what already exists):** one `investigator` agent per part the plan touches (codebase, data, infrastructure, past runs), each writing `notes/<topic>.md` with sources; measure what can be measured. Small and familiar: read it yourself.
  - **Outside (what others do):** the `prior-art` skill, where the problem has likely been solved elsewhere.
  Log one line per notes file on the message board, and give the planner the notes. A plan built on guesses gets reviewed into the ground.

## Rounds

1. **Plan.** Launch the `planner` agent with: the goal in the user's words, constraints, the notes to read, and the files it may write (`PLAN.md`). Planning only.
2. **Attack.** Launch a fresh `plan-adversary` agent with the same inputs plus `PLAN.md`. It writes `PLAN-REVIEW-<round>.md` with blockers, majors and minors, each with a concrete fix. At least one round (usually the first) also runs the same brief through Codex if it is installed (`codex exec -m <strongest available model> -s read-only -C <repo> -o <report file> - < <brief file>`), written to `PLAN-REVIEW-<round>-CODEX.md`: another model family has different blind spots.
3. **Revise.** Planner (or you) addresses every blocker and major: fix it, or record why not. Add a changelog entry at the top of `PLAN.md` mapping each finding to its fix.
4. Repeat 2-3 until it converges: a round finds no blockers and no new majors (usually two or three rounds). If it has not converged after four rounds, stop and escalate to the user with what keeps coming back. Stop earlier if the remaining issues can only be settled by building or measuring; list them in the plan's "to settle by measurement" section instead.

Log each round on the message board (counts per severity, what changed).

## Decisions

Collect every open decision in one section of the plan. Bring the user only those their input changes (goals, priorities, risk, cost, scope); decide technical ones yourself, log them, and list them in one line each. Present each user decision so it stands alone for someone who hasn't followed the work:
- **Context:** the part of the system involved and what it is for, in one or two sentences.
- **The problem, as a consequence** for the goal, not as a mechanism.
- **Options:** two to four, each with what it means for the outcome, its cost, and whether it can be undone.
- **Recommendation:** one, with the reason; then what you need from the user (usually yes or no).
Numbers and mechanism come after, for reference. No undefined labels. If the user says they don't understand, rewrite from zero.

The user owns every decision. When they decide, or ask you to decide, record it (dated, who decided) on the message board and work it into the plan's sections, so the plan stays the true spec.
