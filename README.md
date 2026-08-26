# Walter's Project System

One system for every project you run with AI. It gets smarter each time.

## The files

Four Markdown files sit in your project root:

- **CLAUDE.md** — the map: what the system is, which file holds what, the order to read them. Named exactly `CLAUDE.md` so your AI tool auto-loads it every session. You never edit it.
- **rules.md** — *Global*: the rules you work by, shared by every project. *From this project*: lessons born here that might generalize. Only `/end` adds to it.
- **state.md** — the live handoff, the one file that changes every session. Sections: **Now**, **Short term**, **Long term**, **Rules & lessons**, and **Continue**.
- **`<project-name>.md`** — the project's context: what it is, why it exists, the shared vocabulary. Written from the `project-name.md` template before you start — by `/new-project`, or rebuilt from existing material by `/adopt`.

## The skills

Invoked by hand — you call them, the model doesn't reach for them on its own:

- **/new-project** — create a project that doesn't exist yet. Decides where it belongs and what to call it, creates the folder with the four files above and its own git repo, then grills you until the context file is complete.
- **/adopt** — bring a project you've *already started* into the system. Reads what's already there, then builds the four files above, grilling you until the context is complete.
- **/start** — open a session on a project that exists. Reads the files above, then interviews you until you both know what you're building and why.
- **/end** — close a session. Rewrites `state.md`, promotes any generalizable lesson into `rules.md`, then commits and pushes if the project has a git remote.
- **/handoff-continue** — running out of context mid-task: write what a fresh chat needs into `state.md`'s Continue section.
- **/continue** — in the fresh chat: read the Continue handoff and pick the work up.

`/new-project`, `/adopt`, `/start` and `/continue` lean on **/grill** to interview you.

## The loop

1. **/new-project** to create one from nothing, or **/adopt** one already underway — get grilled until you're aligned. Then **/start** each session.
2. Build in vertical slices. Each change ships something that works.
3. **/end** — save state, capture the lesson, commit.
4. Next session, **/start** reads `state.md` and picks up where you left off.

Run out of context mid-task? `/handoff-continue` in the dying chat, `/continue` in a fresh one — same work, new context.

## Set it up

Keep the templates in a `project-system-docs/` folder inside the folder you open with your AI tool, and give each project its own folder next to it:

```
your-workspace/
├── project-system-docs/     ← CLAUDE.md, rules.md, state.md, project-name.md (the templates)
├── project-a/
├── project-b/
└── ...
```

Then:

- **New project** — run `/new-project`. It finds `project-system-docs/`, creates the project folder, copies the four templates in, renames `project-name.md` after the project, and grills you until the context file is written.
- **Project already underway** — run `/adopt` from inside it. It finds `project-system-docs/`, copies the templates, and builds the context and state from what's already there.

Put the `skills/` folder wherever your AI tool loads skills from — they're global, installed once, and work on every project.

In this repo the templates live under `project-system-docs/` to keep the root clean — but every reference inside them is a bare, root-relative name (`CLAUDE.md`, `rules.md`, `state.md`). That's on purpose: when they land in a real project's root, the links just work. `project-name.md` is the one that gets renamed on the way in, to the project's own name, lowercased.
