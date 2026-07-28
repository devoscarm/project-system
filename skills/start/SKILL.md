---
name: start
description: Start a new project, or start a new session on an existing one.
disable-model-invocation: true
---

I'm starting a project, or a new session on one. Do these three steps in order.

## 1. Sync, then get your bearings

If the context directory lives in a git repo with a remote, run `git pull --ff-only` first — a parallel session or another machine may have pushed newer state since the last time this clone was touched. If the pull fails, tell me and read what's on disk.

Then, before asking me anything, read **the always-read set, and only that**:

- `CLAUDE.md` — the map of how this system works and what each file holds (it auto-loads, so it's already in context). It also says which files exist and when each one is read; trust it over this list.
- The `<name>.md` context file, named after the project folder — what the project is and why it exists. If you can't find it, ask me where it is.
- `rules.md` — the rules I work by, kept short and imperative. If this project also has a `rules-cases.md`, that is the archive of *why* each rule exists, and it is **not** read here: open it only when a rule is about to bind and you need the reasoning.
- `state.md` — what is true right now. Not a log: if something is written there it is true, and if it is not written there it is not true.
- Any file the project keeps for *me* to act on (`oscar.md` or its equivalent), if one exists — so you can answer "what's left?" without going digging.

**Everything else is read on demand, not now.** The architecture document, the generated system map, the scoreboard, the field notes, the rule cases, any guide written for me rather than for you: those get opened when the work needs them, and often with `grep` rather than whole. Reading a hundred thousand tokens of context before knowing the task does not make you know it — it makes the one line that matters compete with the thousand that don't, and that is how a false line survives ten sessions of being read.

## 2. Register the session

`state.md` has a **Sessions in flight** section (create it if this project's file predates it). It lists **open sessions only**: a session that closed leaves no trace here, because `git log` already tells that story.

- If it lists any session, tell me before anything else: a parallel chat may be working these same files. Its `/end` and ours must each merge into `state.md`, never overwrite it.
- **A line is not a lock until you have checked that it is alive.** An orphan line — left by a chat that died without `/end`, often uncommitted — reads exactly like a live one. Check the transcript's last activity (`claude --resume <id>`), the working tree, and whether the work it announces was actually done. If it is dead, recover anything that session discovered, then delete the line.
- Add one line for this session: date, **start time**, the mission in a few words, **which repo and file tree it owns**, and this session's id. The ownership is the point — two tasks that sound independent collide the moment they touch the same files.
- **Commit that line immediately**, before touching anything else. A lock nobody can see is not a lock, and an uncommitted one is invisible to precisely the session it was written for.

This line is the only thing `/start` ever writes; the rest of `state.md` belongs to `/end`.

## 3. Grill me

Interview me relentlessly until we reach a shared understanding — until we can both state what we're building and why. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, give your recommended answer.

Ask one question at a time and wait for my answer before the next. Asking several at once is bewildering.

If a question can be answered by exploring the files or the code, explore instead of asking.

Match the depth to the situation: a fresh project needs the full interview; resuming a session means reading `state.md` first and grilling only where the plan is unclear.

Don't write `state.md` here beyond the session line — `/end` owns it at the session's close.
