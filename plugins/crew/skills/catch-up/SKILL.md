---
name: catch-up
description: Bring the user up to date on what changed since they last looked, in plain language sized to a few minutes of reading, so the work stays auditable and never becomes a black box. Use when the user returns after a break, asks "what happened", "what did you do" or "catch me up", or before asking them to decide on work they have not seen.
---

# Catch up

The user's attention is limited and the agents' output is not. This digest spends that attention on what decides correctness, not on every diff.

1. **Find the marker:** the `Last caught up:` line near the top of `MESSAGEBOARD.md`, or a date the user gives. Collect everything newer: message-board entries, agent-log rows, review files, and `git log` / `git diff --stat` since then if the project is a git repository.
2. **Write the digest** at the level of the goal, not the code: short enough to read in two minutes, and useful if the user stops after the first sentence. Use only terms the user already uses.
   - **One sentence:** where the project stands on the way to done.
   - **What works now that didn't, and what is broken or uncertain.**
   - **Decided without you:** only decisions that change what the project does or delivers, each with its reason and whether it can be undone.
   - **Where you could catch a mistake:** choices and results that shape what the project produces (a range picked, a check relaxed, a number that moved, an assumption relied on, a surprise), one plain line each with the reason.
   - **Needs you:** waiting decisions, each with a one-line recommendation.
   Internal changes (refactors, fixes inside modules, new tests) stay in the files; mention them only as "code cleanup, checked, nothing changes for you".
3. **Offer, don't push, details:** a walkthrough of any part the user picks: explain it with the code open, one step at a time, with an analogy from their field where it helps. Answer "why" questions from the record (message board, plan, reviews) and link the entry.
4. **When the user says they are caught up,** update the `Last caught up:` line (date and newest entry title). Add the line if it is missing.
