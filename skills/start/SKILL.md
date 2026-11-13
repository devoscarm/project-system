---
name: start
description: Start a session on a project that already exists.
disable-model-invocation: true
---

I'm starting a project, or a new session on one. Do these steps in order.

**My words in this conversation beat every line below** (rule 319 where the project numbers it):
if a step tells you to do something I have just told you not to, do what I said and name the step.

## 1. Sync, then read the always-read set — and only that

If the context directory is a git repo with a remote, `git pull --ff-only` first; if it fails,
tell me and read what's on disk. Then read, before asking me anything:

- `CLAUDE.md` (auto-loaded) — the map: which files exist and when each is read. Trust it over this list.
- The project's `<name>.md` — what it is and why. If you can't find it, ask me.
- `rules.md` — the law, whole. Its `rule-cases.md` is opened only when a rule is about to bind.
- `state.md` — what is true now. Written there = true; not written = not true.
- The file the project keeps for ME to act on (`oscar.md` or equivalent), if any.

**Where a file opens with an index (`## Contenuto`, `## Indice`, `## Contents`), read the index
and stop:** open a section's body only when the work reaches it. Everything else is on demand,
often with `grep`.

## 2. Take your own working tree, then register

- **Your session id comes from your scratchpad path and nowhere else** — the UUID directory in it;
  the short id is its first segment. Never take one from `sessions/`, a commit, or another
  session's file. If you can't determine it, ask me.
- **If the project gives each session a working tree, open it first** (its `CLAUDE.md` names the
  command) and do all your work there — unless the brief waits on a predecessor (§3): then the lock
  comes first and the tree only once the wait frees you, from a main that has its code. If not, work in the shared clone under its rules.
- **The lock claims ownership** and must be readable by a chat that arrives later: repos and file
  trees you own, worktree and branch if any, and how others can reach you (the project says how).
  Prefer one file per session where the project has it. **The filename must be NEW**: if
  `sessions/<your-id>.md` exists, it is not yours — stop and tell me.
- **Other locks: tell me first**, and say plainly whether their perimeter collides with the
  mission. A lock is alive until its **work** says otherwise — commits, worktree, the files it
  claims — never the clock. A lock that declares itself guided or administrative moves nothing
  measurable while alive; say so in yours if that is what you are.
- **Commit the lock before touching any file of the work.** Reading and measuring touch nothing:
  with no mission named yet, write the lock the moment one is picked. This is the only thing
  `/start` writes; the rest of the state belongs to `/end`.

## 3. What shape is this session?

- **It EXECUTES a brief** (the project may say *mandate*): a file holding a decided contract.
- **It DESIGNS** — it produces a brief for someone else to execute.
- **It ADMINISTERS** — only if I ask for it: it owns no code, keeps the queue honest, and writes
  briefs instead of commits.

Then:

- **No mission named, or «which brief?»** — enumerate them (the root `CLAUDE.md` gives the
  command, e.g. `ls */mandate-*.md`), read each owning project's `state.md` for queue line and live
  locks, and give me ONE table: what each does, its size, and whether it can run NOW beside the
  live sessions. Only two things stop it: a file on a SHARED disk that a live session owns (the
  context clone, a repo without per-session trees — name the collision), and a predecessor in its
  order line whose code is not merged yet (name it, and who executes it). Sharing code files in
  per-session trees is NOT a collision: both run, the second to close resolves at merge. Rank by
  parallel-safety first, value second. If a design session is already live, assume this one executes.
- **A brief is named — read it in full, now.** Its perimeter replaces your guess: re-commit the
  lock to match. Read less than a planner would: open only the on-demand files it names.
- **Its order line decides whether to wait — not me.** If the brief names predecessors (here:
  `**Seriale dopo:**`), run the project's wait (here: `scripts/session.sh wait <brief>`). Exit 0:
  free. Exit 3: nobody executes a predecessor — say so in the proposal, don't wait. Otherwise it is
  waiting: write what on into your lock (here: an `Aspetta:` line), commit it, and re-run the wait
  **in the background** — you are woken when it exits. Meanwhile do everything that does not need
  the predecessor's code (check the claims, write the proposal), but open no working tree: one
  opened before the merge starts without that code. Say once what you wait on; never ask me to
  relaunch you.
- **Or start on the predecessor's branch — only if I choose it.** When the predecessor's code is
  done and its gate green, offer it in the proposal: open your tree, `git merge --no-ff
  session/<pred>` into it, and start now; the project's close refuses until those commits are on
  main (here: `session.sh close`). The price: a re-merge if the predecessor changes during its close.
- **A brief is a claim, not a task.** Check cheaply what it assumes — routes, files, migrations —
  and if the world moved, propose the pruned version instead of executing last week's plan.
- **Then propose and stop**: what it does, **which model and why**, how much work. Wait for my go —
  unless my launch line already carries it (`/start <brief> vai`): then write the proposal and go on.
  One line decides the model: *the more a mistake would propagate instead of failing, the stronger
  the model.*

## 3-bis. While executing: every slice closes itself

A finding at /end costs a whole gate; the same finding after a slice costs a commit. So when a
brief has slices (or the work has natural pieces), each one ends with, in this order:

0. **On today's main** (here: `scripts/session.sh sync <id>`): every session in parallel lands
   small and often, so main moves under you; a conflict met now is yours to solve in your tree,
   met at the gate it throws you off a train and costs everybody a run (12/11, train 66: two
   sessions grew the same file, the second was refused). After the sync, a ceiling or a baseline
   both sides raised is RE-MEASURED on the merged tree, never the higher of the two.
1. **A fresh-eyes review** of that slice's diff — /end's step 2-ter agent, same four questions —
   on the commits `scripts/review-stamp.sh base <id>` (claude-ctx) names; its fixes committed, step
   2's rehearse green on that commit, then `review-stamp.sh mark <id>` (it refuses without the
   green rehearse): /land and /end then skip the review instead of redoing it. Never by
   you: you read your intent instead of the text. **«Ready» / «pronta» to Oscar** only when
   `review-stamp.sh check <id>` exits 0 AND the rehearsals of the tip are green: said before, it
   is not true (12/11: two sessions said «pronta» and their /end found the review still to do).
2. **The project's whole unit check** (here: the repo's `scripts/ci/rehearse.sh` in the worktree),
   never a folder at a time: the suite-wide fences (catalogues, cycles) live outside your folder.
3. **The slice's e2e, in the background** (here: `scripts/session.sh e2e <id>`): its stamp is
   reused, so the final gate counts only the changes after it (plus main's, if main moved: those
   meet your code for the first time). It yields
   to a train departing or closing, so it never delays somebody's merge.
   A slice that adds a migration runs it in that same slice, never «at the gate»: a migration runs
   every suite, and the schema's own fences (masking, RLS, grants) go first and answer in minutes.
4. **Green → land it now** (skill `land`: the gate, then `close --stay`), never «all at /end»
   (Oscar 12/11: «voglio parallelizzare le cose, che non si mettano i bastoni tra le ruote»). A
   slice on main is a slice every other session builds on and checks against; ten slices held
   until /end are ten slices nobody saw, met at once by two trains. /end then has only the prose
   and the last slice. A slice that cannot land alone (half a contract, a migration whose readers
   come later) says so in the lock and rides with the next one.

Read every verdict by its exit code, never through a pipe (the project's hook refuses it).

## 4. Grill me

Interview me until we both can state what we're building and why: one question at a time, each
with your recommended answer; explore instead of asking when the files can answer. Match the depth
to the situation — a fresh project gets the full interview, a resumed session only where
`state.md` leaves the plan unclear, an executed brief only where it is silent or the world moved.

<details>
<summary>Why each step is shaped this way</summary>

- **Tiered reading.** A hundred thousand tokens read before knowing the task does not make you know
  it: the one line that matters competes with the thousand that don't, and that is how a false
  line survives ten sessions of being read. An index costs a few hundred tokens and keeps every
  section findable.
- **A tree of your own.** Two chats editing one file mix their text physically on disk; no git
  command separates it afterwards — not `commit --only`, not `add -p`. With a tree of its own a
  session cannot suffer that, and the verbs a shared tree forbids become safe again.
- **Your own id.** A lock filed under someone else's id makes their `/end` delete yours and points
  your commits at their conversation; neither failure announces itself.
- **A file per lock.** A shared section is a shared write point: two `/end`s rewriting one
  paragraph is how a live lock gets deleted by somebody acting in good faith.
- **Alive by work, not clock.** An orphan reads exactly like a live lock; a chat thinking for five
  minutes has the same mtime as a dead one. A guided session (the human's hands on a machine you
  must not touch) leaves zero traces in hours of real work.
- **A brief is a claim.** It was written when the world looked a certain way; another session may
  have built half of it since. A stale note misleads a reader; a stale brief gets *obeyed*.
- **Propose and wait.** The human may switch model first, and switching mid-session keeps the
  conversation intact.

</details>
