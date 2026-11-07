---
name: second-opinion
description: Ask a fresh model with no project context (a new Claude agent, Codex, or both) a neutral, self-contained version of a design question, then compare its answer with yours. Use before committing to a non-trivial choice, when you and the user disagree, when you are unsure, or when the user asks "what would a fresh model say".
---

# Second opinion

You are anchored on your own context: the plan, the code, what you already decided. A fresh model is not. This skill gets an unanchored answer to a general version of the question, then maps it back.

## 1. Write the question neutrally

- Make it general and self-contained: the problem in a few sentences, the constraints that matter, nothing else. No project jargon, no file names, no history.
- Do not hint at your preferred answer. List the options symmetrically, or ask open-ended ("what is better for X, and why"). Ask for reasons, trade-offs and when each option fails.
- Save it to `notes/second-opinion/<date>-<topic>.md` with the date, and your own current answer under it (written before you read theirs).

## 2. Route it

| Situation | Ask |
|---|---|
| Ordinary design question, quick check | a fresh Claude agent (`general-purpose`, given only the question; it may search the web) |
| Claude-family blind spots suspected, or you and Claude reviewers already agree too easily | Codex |
| Costly or hard to reverse, or you and the user disagree | both, in parallel |

Codex, with only the question and no repository: `codex --search exec -m <strongest available model> -s read-only --skip-git-repo-check -C <empty temp dir> -o <answer file> - < <question file>` (`--search` gives it web search).

## 3. Compare and report

- Where they agree with each other and with you; where they differ, and whose reasons are better.
- What does not apply to this project, and why (they answered the general question).
- What you change because of it, if anything. If nothing, say why.

Tell the user in plain words: the question as asked, the short answers, and what changes. Record one line on the message board with the notes path.
