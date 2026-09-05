---
name: start
description: Start a session on a project that already exists.
disable-model-invocation: true
---

I'm starting a project, or a new session on one. Do these steps in order.

## 1. Sync, then get your bearings

If the context directory lives in a git repo with a remote, run `git pull --ff-only` first — a parallel session or another machine may have pushed newer state since the last time this clone was touched. If the pull fails, tell me and read what's on disk.

Then, before asking me anything, read **the always-read set, and only that**:

- `CLAUDE.md` — the map of how this system works and what each file holds (it auto-loads, so it's already in context). It also says which files exist and when each one is read; trust it over this list.
- The `<name>.md` context file, named after the project folder — what the project is and why it exists. If you can't find it, ask me where it is.
- `rules.md` — the rules I work by, kept short and imperative. If this project also has a `rules-cases.md`, that is the archive of *why* each rule exists, and it is **not** read here: open it only when a rule is about to bind and you need the reasoning.
- `state.md` — what is true right now. Not a log: if something is written there it is true, and if it is not written there it is not true.
- Any file the project keeps for *me* to act on (`oscar.md` or its equivalent), if one exists — so you can answer "what's left?" without going digging.

**Everything else is read on demand, not now.** The architecture document, the generated system map, the scoreboard, the field notes, the rule cases, any guide written for me rather than for you: those get opened when the work needs them, and often with `grep` rather than whole. Reading a hundred thousand tokens of context before knowing the task does not make you know it — it makes the one line that matters compete with the thousand that don't, and that is how a false line survives ten sessions of being read.

## 2. Take your own working tree, then register the session

**If the project gives each session its own working tree, open it FIRST and do all your work
there.** A shared working directory is not a discipline problem that better rules can fix: when
two chats edit the same file, their text is physically mixed in one file on disk, and no git
command can separate it afterwards — not `commit --only`, not `add -p`, because all of them read
the file as it is. A session with its own tree cannot suffer that, and the verbs a shared tree
forbids (`git add -A`, `git commit -a`, `git stash`, `--amend`) become safe again.

The project's `CLAUDE.md` says whether it has this and what the command is. If it does not, work
in the shared clone and stay under the project's rules for it.

**Your session id comes from your scratchpad path and from NOWHERE else** — it is the UUID
directory in that path, and the short id is its first segment. Read it before you write
anything, because the worktree, the branch, the lock filename and every commit subject are all
named after it. ⚠️ **Never take an id from `sessions/`, from a recent commit message, or from
anything another session wrote**: those are *their* ids. A lock filed under somebody else's
name makes their `/end` delete yours, and makes your commits point at their conversation — and
neither failure announces itself. If you cannot determine your id, ask me; do not guess one.

Then register. **The lock is a claim of OWNERSHIP, and its whole job is to be readable by a chat
that arrives after you**: which repos and which file trees are yours, plus your worktree and
branch if you have one.

- **Where the lock goes is the project's business** — one file per session in a directory the
  project names, or a line in a shared section if that is what it still keeps. Prefer a **file
  per session** where it exists: a shared section is a shared write point, and two `/end`s
  rewriting the same paragraph is how a live lock gets deleted by somebody acting in good faith.
- If other sessions are registered, tell me before anything else, and say plainly whether their
  perimeter collides with the mission — name the collision instead of averaging it away.
- **A lock is not a lock until you have checked that it is alive.** An orphan left by a chat that
  died reads exactly like a live one. Measure the **work** it announces — its commits, its
  worktree, the files it claims — never the clock on its transcript. ⚠️ And a **guided** session
  (the work is the human's hands on a machine you must not touch) moves nothing measurable: hours
  of real work leave zero traces. If a lock declares itself guided, an immobile perimeter means
  nothing. Say so in your own lock if yours is guided — and the same for an
  **administrative** session (step 3), which moves prose and no code: measured on code
  commits it looks dead while it is doing the only work nobody else is doing.
- **The filename must be NEW.** If `sessions/<your-id>.md` already exists, it is not yours —
  stop and tell me instead of writing into it. Two chats behind one name destroy the very
  handle the orphan-lock rule depends on: neither can be measured separately, and the first
  `/end` deletes the other's lock.
- **Commit the lock before you touch any file of the WORK.** Reading, enumerating and
  measuring touch nothing, so when I have not yet named the mission the lock is written the
  moment I pick one — not before, when its perimeter would be a guess. What must never happen
  is work landing under no lock at all. A lock nobody can see is not a lock, and an uncommitted
  one is invisible to precisely the session it was written for.

This lock is the only thing `/start` ever writes; the rest of the state belongs to `/end`.

## 3. What shape is this session? There are THREE, not two

A session takes one of three shapes, and picking the wrong one costs a whole session:

- **It EXECUTES a written brief.** When a session designs, its output is often a **brief** — a file holding a contract already decided, so that the session executing it implements instead of re-deciding. If this project keeps briefs, `state.md` lists the open ones (the project names them: *mandate*, *brief*, *work order*), and each carries its own file perimeter for step 2's ownership line.
- **It DESIGNS** — it produces a brief for somebody else to execute.
- **It ADMINISTERS.** It owns **no code at all**. It keeps the queue honest and re-measures a brief before anyone executes a stale one; it prunes, writes and re-orders the briefs; it translates outside doctrine into this project's own rites; it decides the questions the documents explicitly leave to it; it answers "what is left / which brief / what must *I* do myself"; and it argues with me — including about shortcuts I have already approved.

### Check for the administrator FIRST, before enumerating anything

Read the ledger of live sessions. **If no administrative session is registered, propose becoming one** — say it in the same message as the brief list, not instead of it, and say plainly what it would own (the context prose: the briefs, the `state.md` files, the rules, the file written for me) and what it would not (**any code repository** — when the answer to a question is code, an administrative session writes a *brief*, not a commit). An administrative session owns no code, so it **collides with nothing** and never has to wait its turn behind a live executor.

**Why this check is first.** Everything else in this system is built for executors — working trees, locks, file perimeters, a closing rite that merges. Nobody is built to keep the queue honest, so a brief written when the world looked different gets executed as written, and the pruning falls on me by hand or on whichever chat happens to pass. And without this check the default is actively wrong: a chat that starts with no mission gets funnelled toward "execute one", which is the worst move precisely when the queue itself is what needs looking at.

### Then, whichever shape it is

- **If I launched `/start` with no mission at all AND a planning/design session is already registered in *Sessions in flight*, assume this session EXECUTES.** Don't become a second planner beside the one that exists: enumerate the queued briefs (next bullet) and propose them **optimized for parallelism** — what can start NOW without touching any live session's files or repos, in a ranked short list. Only if I then say this session is for something else does the assumption drop.

- **If I say "execute a brief" without naming one — or ask what's available — don't ask me where they are: enumerate them.** Briefs live each in its own project folder; from the context root one command finds them all (the root `CLAUDE.md` carries it; the convention is `ls */mandate-*.md`). For each hit, read the owning project's `state.md` — its queue line and its *Sessions in flight* — and give me ONE small table: what each brief does, its size, and whether it can run NOW beside the live sessions. A brief whose repo no live session owns runs safely in parallel; one sharing a repo with a live session does not — name the collision instead of averaging it away. Recommend one, ranked by parallel-safety first, value second: I pick by how much session budget I have left. Then registration (step 2) happens in the **owning** project's ledger, and the rest of this step continues there.

- **If I told you to execute one, read it now, in full, before anything else.** Its perimeter replaces the one you would have guessed; widen or narrow your session line accordingly and re-commit it. And read LESS than a planning session would: the brief is your memory — open only the on-demand files and sections it names (a `knowledge.md` section, a runbook), with `grep`, when the work reaches them. An executor that loads the whole context re-creates the problem tiered reading exists to solve.
- **A brief is a claim, not a task.** It was written when the world looked a certain way, and the world moved: another session may have already built part of it, or made it pointless. Before proposing anything, check the parts you can check cheaply — the routes, the files, the migrations it assumes. If it is stale, say so and propose the pruned version rather than executing a plan that was true last week.
- **Then propose, and stop.** Tell me, in one short message: what this brief does, **which model you recommend and why**, and how much work it looks like. Then **wait for my go** — I may switch model first, and switching mid-session keeps this conversation intact. Do not touch a file of the work before I answer — step 2's lock is not work, it is what makes this session visible, and it is written as soon as the brief is picked.

How to recommend a model, in one line: **the more a mistake would propagate instead of failing, the stronger the model.** Irreversible effects, security boundaries, schema shapes with no test that can catch a wrong one, work with no oracle to check it against — those want the strongest available. Work whose contract is already written and whose net is a test suite does not.

If this session has no brief, skip the brief bullets — but **not** the administrative check
above: that one runs on every `/start`.

## 4. Grill me

Interview me relentlessly until we reach a shared understanding — until we can both state what we're building and why. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, give your recommended answer.

Ask one question at a time and wait for my answer before the next. Asking several at once is bewildering.

If a question can be answered by exploring the files or the code, explore instead of asking.

Match the depth to the situation: a fresh project needs the full interview; resuming a session means reading `state.md` first and grilling only where the plan is unclear; and a session executing a brief has already had its interview — the brief *is* the interview's output, so grill me only where the brief is silent or where the world has moved under it.

Don't write `state.md` here beyond the session line — `/end` owns it at the session's close.
