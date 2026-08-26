---
name: adopt
description: Bring an already-started project into the system.
disable-model-invocation: true
---

I'm bringing a project I've already started — one that already holds notes, files, and decisions — into this system. It has no system docs yet, or only ad-hoc ones. Build them from what's there. Do these in order.

## 1. Inventory

Scan the project folder. Flag anything that already fills a system slot: a `CLAUDE.md` / `claude.md`, a `<name>.md` context file, `rules.md`, `state.md`, or an ad-hoc status doc. If the four system docs already exist and are current, there's nothing to adopt — say so and stop.

## 2. Learn the project

Read the flagged docs, then enough of the content to know: what this is, why it exists, its vocabulary, where it stands, where it's heading. Large folder → push the reading into a sub-agent and keep only the conclusions.

## 3. Get the templates

You need `CLAUDE.md` (the map) and `rules.md` (the Global rules) to copy in. Look, in order:

1. A genuine system copy already in the project → use it.
2. A `project-system-docs` folder anywhere in the folder you can access → copy both from there.
3. Neither → stop and tell me to fetch them from https://github.com/vukappa4/project-system, then re-run.

## 4. Grill me until the context is complete

Draft `<name>.md` and `state.md` from what you learned. Then interview me, one question at a time with your recommended answer, until both are complete.

Walk the branches, resolve dependencies one by one. Explore the files instead of asking wherever you can; confirm what you inferred and pull the "why" the files can't give.

## 5. Write into the project root

Only once the context is complete, write the four files side by side in the project root — never a subfolder: `CLAUDE.md` (keep that exact name so it auto-loads), `rules.md`, `<name>.md` (named after the project folder, lowercased), `state.md`.

Before writing each, check nothing real is already in that slot. If something is (a `<name>.md`, or a `CLAUDE.md` that isn't ours), don't overwrite it: rename the old one to `old_<name>.md`, write ours, and once done ask me whether to delete the `old_*` files or keep them to review.

## 6. Close

Check the project-system skills are installed and reachable here — that's what makes `/start`, `/end` and the rest work. Report what you created, renamed, and left for me to decide. Don't commit.
