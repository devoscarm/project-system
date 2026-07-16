# CLAUDE.md

I'm the map for this project: what the system is, which file holds what, and the order to read them.

## The system

A small, repeatable setup for every project where I lean on AI, each project feeds rules back up so the next one starts smarter.

## The files

- **CLAUDE.md**: this map. Named `CLAUDE.md` so it auto-loads every session. You never edit it.
- **`<project-name>.md`**, the project's context: what it is, why it exists, the shared vocabulary. Written before we start.
- **rules.md**: *Global*: the rules I work by, shared by every project. *From this project*: lessons born here that may generalize. You read it; you only add to it via /end.
- **state.md**, the live handoff and the only file that changes every session.

## Read order

CLAUDE.md (this — auto-loaded) → the `<name>.md` context file → rules.md → state.md.

## The skills

- **/adopt** — bring a project I've already started into the system: learn it from its own files, then build the four docs above.
- **/start** — begin a new project, or start a new session: pull, read the files above, register the session in state.md's Sessions in flight, then interview me until we're aligned.
- **/end** — close a session: merge-update state.md (parallel sessions may have ended first), add any generalizable lesson to rules.md, and commit.
- **/handoff-continue** — a session hits its context limit mid-task: dump what a fresh chat needs into state.md's Continue section.
- **/continue** — in the fresh chat: read the Continue handoff, pick the work up, then clear it.
