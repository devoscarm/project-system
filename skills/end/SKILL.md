---
name: end
description: End a session — save state, capture lessons, commit.
disable-model-invocation: true
---
I'm ending a session. Do these three steps in order.

## 1. Update state.md

**Re-read `state.md` from disk first — never trust the copy in your context.** If it changed since /start read it (a parallel session ended before this one), merge: keep what you don't recognize, weave this session's outcome into it, and tell me what you merged. Overwriting the unknown is how parallel sessions erase each other.

Then rewrite only what actually changed. On each section's first fill (a fresh project), replace its placeholder description with real content, and leave the header comment untouched.

- **Now / Short term / Long term** — compress and rewrite to match where we are. Never append; they're a snapshot, not a log.
- **Rules & lessons** — add or prune deliberately, don't churn. Only rules specific to THIS project that never generalize. A lesson that would help other projects goes to `rules.md` instead (step 2).
- **Sessions in flight** — remove this session's line. Leave any other lines alone: their sessions will clear them.

## 2. Add a lesson to rules.md — only if one emerged

If this session produced a rule or lesson that could help other projects, add it to `rules.md` under **From this project**. If nothing generalizable came up, leave the file alone — don't pad it.

## 3. Commit and push — only if this is a git repo

If the project has a git repo with a remote, stage **only the files this session touched** — never `git add -A`: a parallel session may have half-finished work in the same working tree. Run `git status` first; if there are modified files you don't recognize as yours, leave them unstaged and tell me about them. Then commit with a message that captures the *why* of what changed, and push. If the push is rejected because a parallel session pushed first, `git pull --rebase` and push again; if that surfaces conflicts, stop and show me instead of resolving blindly. If there's no repo or no remote, skip this and tell me the project has no git home yet.
