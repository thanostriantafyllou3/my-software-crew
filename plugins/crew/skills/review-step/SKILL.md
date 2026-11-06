---
name: review-step
description: Run three parallel code reviews (quality, correctness and robustness, plan conformance) after a build step or before shipping, then triage and answer every finding. Use when a module and its tests are done, before a release or upload, or when asked to review code.
---

# Review a build step

Three fresh agents review the code in parallel, one job each. They see the code and the plan, never your reasoning, so their findings are not anchored on yours. They write reports and never edit code.

## Inputs

- `ROOT`: the project or sub-project folder (holds `PLAN.md` if there is one).
- `N`: the next review number (one more than the highest in `ROOT/reviews/`).
- `STEP`: one line naming what was just built.
- Private or sensitive paths the reviewers must not open, if any.

## Run

Launch in one message, in the background:
- `quality-reviewer` → `ROOT/reviews/QUALITY-REVIEW-N.md`
- `robustness-reviewer` → `ROOT/reviews/ROBUSTNESS-REVIEW-N.md`
- `conformance-reviewer` → `ROOT/reviews/CONFORMANCE-REVIEW-N.md` (skip if there is no plan or spec)

Give each the absolute paths, `STEP`, the test command, and the paths it must not open.

## After

1. Read the reports. Triage each finding: **fix**, **decline** (with a reason), or **decide** (the user's call).
2. Fix the "fix" items yourself if they are few; otherwise commit (with the user's go-ahead) so the last commit is the snapshot, and launch one `refactorer` agent with the findings, your decision on each, the snapshot path, the test command, the bugs it may change behaviour for, and a response-file path. When it returns, read its diff yourself and confirm tests pass with unchanged tolerances.
3. Write (or complete the refactorer's) `ROOT/reviews/REVIEW-N-RESPONSE.md`: one line per finding with its outcome.
4. Log one entry on the message board: one plain sentence first, then step reviewed, counts, what changed, what is open.
5. Tell the user in plain terms what the review found and what changed (the two-minute report in `user/CLAUDE.md`), then bring "decide" items in the decision-brief form of `user/CLAUDE.md` (context, problem as a consequence, options, recommendation, what you need), each understandable without having read the reports. Technical items the user would have to take on trust are yours to decide: decide, log, and tell them in one line.
