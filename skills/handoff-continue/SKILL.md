---
name: handoff-continue
description: Hand this session off mid-task.
disable-model-invocation: true
---

This session is nearing its smart zone context window, but the work continues in a fresh chat. Capture what that chat needs into `state.md`'s `Continue` section, then stop.

## 1. Ignore the durable stuff

Anything that outlives this session doesn't belong in Continue - /continue clears it:

- A rule or gotcha that binds this project → `Rules & lessons` in `state.md`.
- One that would help other projects → `rules.md`, under *From this project*.

Only session-only notes stay for step 2.

## 2. Fill the Continue section

Write `Continue` with exactly this structure. Keep it short and high-signal; reference files and commits by path, don't duplicate them. Continue holds one handoff — if it isn't empty overwrite it.

### Task
- **In flight:** the stretch of work being handed over, and why.
- **Done since the last clean state:** progress not yet in Now (often uncommitted).

### To do
Ordered checklist of the concrete remaining steps. Top unchecked box = the next action.

### Open threads
Unresolved questions or decisions still in the air: what the fresh chat must settle to move on.

### Carry-over context
Settled things the fresh chat needs and can't rebuild from the files:
- **respect:** dos and don'ts that hold only for this session, and dead ends already ruled out.
- **load:** grill decisions, the working prompt, or sub-agent briefings — only if needed to continue.

## 3. Rules

Don't rewrite Now / Short term / Long term, no commit is needed: the working tree carries over to the new chat as-is.
