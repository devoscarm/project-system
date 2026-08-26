---
name: new-project
description: Create a new project.
disable-model-invocation: true
---

I'm creating a new project. Do these in order.

## 1. Place it and name it

Decide whether it's personal or work. Work means I build it for the job I hold now; everything else is personal. Personal projects go in the **personal projects folder**, work projects go in the **work projects folder** inside the current job folder. Both are named in the index `.md` at the root of the workspace.

Then come back to me with both decisions at once: which of the two it is and why, and a folder name drawn from what I told you. I confirm or override before you create anything.

If a folder with that name already exists, stop and say so: the project is already here, and /adopt is the skill for it.

## 2. Build the folder

Create the folder and copy in `CLAUDE.md`, `rules.md`, `state.md` and `project-name.md`. Get them from:

1. A `project-system-docs` folder anywhere you can reach → copy all four from there.
2. Not reachable → stop and tell me to fetch them from https://github.com/vukappa4/project-system, then re-run.

Rename `project-name.md` to the folder name, lowercased.

Then run `git init` in the new folder: every project is its own repository, personal or work. No commit, no remote — those are mine.

## 3. Grill me

Interview me until you can write the context file end to end, every section that earns its place carrying something real. That is the stopping condition, not a question count.

Ask one question at a time and wait for my answer before the next. For each, give your recommended answer. Walk down each branch of the design tree, resolving dependencies between decisions one by one.

Start from whatever I gave you when I called you: mine it for answers, confirm what you inferred instead of asking it fresh, and only ask what's actually missing. If I gave you nothing, open with what this is and why it exists, and build out from there. Wherever a file or the code can answer, explore instead of asking.

## 4. Write the context file

Only once the context is complete. Write it in my voice: direct, short, no filler. Drop a section rather than pad it. Add one the template doesn't list when the project genuinely needs it.

## 5. Fill state.md — only where I've already decided

Fill **Now**, **Short term** and **Long term** from what I actually told you, and ask about the ones you can't fill. Offer "I'll decide later" as an answer every time: a section I don't answer keeps its placeholder.

## 6. Index it

Add the project to the index `.md` of the folder it landed in — one line, in the format the entries around it use.
