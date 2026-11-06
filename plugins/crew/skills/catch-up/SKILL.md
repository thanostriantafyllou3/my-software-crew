---
name: catch-up
description: Bring the user up to date on what changed since they last looked, in plain language sized to a few minutes of reading, so the work stays auditable and never becomes a black box. Use when the user returns after a break, asks "what happened", "what did you do" or "catch me up", or before asking them to decide on work they have not seen.
---

# Catch up

The user's attention is limited and the agents' output is not. This digest spends that attention on what decides correctness, not on every diff.

1. **Find the marker:** the `Last caught up:` line near the top of `MESSAGEBOARD.md`, or a date the user gives. Collect everything newer: message-board entries, review files, and `git log` / `git diff --stat` since then if the project is a git repository.
2. **Write the digest** (one screen, about five minutes of reading). Define every term, label and ID the first time it appears.
   - **In one paragraph:** where the project stands and what changed, in plain words.
   - **Changes:** grouped by purpose, not by file; each says why.
   - **Decided without you:** each decision, its reason, and whether it is reversible.
   - **How we know it works:** what was measured or tested, against what; what is still assumed.
   - **Surprises:** results that contradict the plan or an earlier belief.
   - **Needs you:** waiting decisions, each with a one-line recommendation.
   - **Worth reading:** two or three `file:line` places that decide correctness, with what to look for in each.
3. **Offer a walkthrough** of any part the user picks: explain it with the code open, one step at a time, with an analogy from their field where it helps. Answer "why" questions from the record (message board, plan, reviews) and link the entry.
4. **When the user says they are caught up,** update the `Last caught up:` line (date and newest entry title). Add the line if it is missing.
