---
name: start
description: Start a new project, or start a new session on an existing one.
disable-model-invocation: true
---

I'm starting a project, or a new session on one. Do these three steps in order.

## 1. Sync, then get your bearings

If the context directory lives in a git repo with a remote, run `git pull --ff-only` first — a parallel session or another machine may have pushed newer state since the last time this clone was touched. If the pull fails, tell me and read what's on disk.

Then, before asking me anything, read, in this order:

- `CLAUDE.md` — the map of how this system works and what each file holds (it auto-loads, so it's already in context).
- The `<name>.md` context file, named after the project folder — what the project is and why it exists. If you can't find it, ask me where it is.
- `rules.md` — the global rules I work by, plus this project's generalizable lessons.
- `state.md` — the live handoff from the last session. If it's empty, this is a fresh project.

## 2. Register the session

`state.md` ends with a **Sessions in flight** section (create it if this project's file predates it).

- If it already lists open sessions, tell me before anything else: a parallel chat may be working these same files. Its `/end` and ours must each merge into `state.md`, never overwrite it — and if a listed session looks abandoned, ask me whether to clear its line.
- Add one line for this session: `- <YYYY-MM-DD> — <the mission, in a few words>` (ask me for the mission if step 3 hasn't surfaced it yet — a few words are enough).

This line is the only thing /start ever writes; the rest of `state.md` still belongs to /end.

## 3. Grill me

Interview me relentlessly until we reach a shared understanding — until we can both state what we're building and why. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, give your recommended answer.

Ask one question at a time and wait for my answer before the next. Asking several at once is bewildering.

If a question can be answered by exploring the files or the code, explore instead of asking.

Match the depth to the situation: a fresh project needs the full interview; resuming a session means reading `state.md` first and grilling only where the plan is unclear.

Don't write `state.md` here beyond the session line — /end owns it at the session's close.
