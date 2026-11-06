---
name: prior-art
description: Before designing or proposing something others have likely solved, research current practice online (docs, papers, repos, practitioners' write-ups) and turn it into sourced options. Use before a plan, when choosing a method or tool, or when the user asks "how do others do this" or "is there something better".
---

# Prior art first

Proposals from memory are dated and narrow. This skill finds what others already do, then proposes from that.

## Scope the question

Write the question in one or two sentences, with the constraints that matter (scale, budget, tools, what already exists). Split it into 2-4 sub-questions if it has distinct parts (e.g. "how are personas generated", "how is prompt diversity measured", "what leaks hints").

## Search

- Narrow question: search yourself. Broad question: launch one `researcher` agent per sub-question, in parallel, each writing `notes/prior-art/<topic>.md`. For a full literature review, use a dedicated deep-research skill if one is installed.
- Sources, best first: official docs and changelogs (for tools and APIs, check the version and date); papers (arXiv, venues) and their code; widely used repositories; engineering blogs and write-ups by practitioners. Open what you cite; never cite from a search snippet alone.
- Note for each source: what it does, the evidence it gives, its date, and how it applies here (or why it does not).

## Propose

Bring back:
1. **What others do:** 3-6 approaches, each in two or three lines with its source link.
2. **What fits us:** which parts apply, adapted to the constraints, and which do not and why.
3. **Options and a recommendation**, marking which ideas come from which source and which are your own judgement.

Record the result on the project's message board (one line plus the notes path), and cite it in the plan where it shapes a decision.
