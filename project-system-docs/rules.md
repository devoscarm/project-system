<!--
Project-only rules that will never generalize don't live here — they stay in state.md's "Rules & lessons". Keep this file short and high-signal: prune, don't append blindly.
-->

# rules.md

## Global

How I want you to work — the rules every project inherits.

- **Never delete anything, and never ask me to approve a delete.** Name the exact paths and I remove them myself.
- **Default to sub-agents for read-heavy work.** Before any search, exploration, or review that will touch more than 3 files, spawn a sub-agent instead of reading them yourself: the main thread keeps the conclusion, not the raw pages. Explore for searches, Plan for design, general-purpose for multi-step work, a fresh-context agent for review. Brief each as if it knows nothing; make it return findings, not dumps — then trust the summary: don't re-open its transcript or output file. Independent work → one message, several agents, in parallel. Skip only when you already know the exact file and symbol.
- **Never keep project facts in memory.** Never use your internal or auto memory to store anything that could live in `state.md`, `rules.md`, or the `<name>.md` context file. Memory may only point to a project ("it exists, it lives here"), never mirror its contents.
- **Context is scarce.** Keep every file short and high-signal. Say things once. Prune hard
- **Write what is, never what was.** Never reference a superseded version. Never date a change or a check. History goes in the commit message. If a wrong path is still reachable today, forbid it in the present tense and say why.
- **Build in vertical slices.** Each change cuts through the whole stack and ships something that works. No half-finished layers.
- **Never grade your own work.** Check against something external: tests, a build, a screenshot, a fresh-context review. "Done" you decided alone doesn't count.

## From this project

Rules and lessons born here that could help other projects — candidates to promote into Global once reworked.
