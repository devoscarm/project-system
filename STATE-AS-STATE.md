# State as state — what this branch changes, and why

This branch reworks `/start` and `/end` around one idea: **a state file is a state, not a
log** — and parallel sessions must be safe **by construction**, not by discipline. Everything
below was learned running the system for months on one real project, with up to five chats
working the same context repo at once. Each piece exists because its absence was paid for;
the incidents are named, because a rule without its incident reads as taste.

## The two incidents that shaped it

**A false line survived ten sessions of being read.** Not for lack of space: the always-read
set had grown to ~99k tokens, of which roughly 15% was still live. Past a certain density,
adding a true sentence does not add knowledge — it adds the noise a false sentence hides in.
That is why `/end` now *deletes*, why reading happens in tiers, and why the state file gets a
hard line budget measured with `wc -l`.

**Three `/end`s ran in parallel and converged only by luck.** Each `/end` is a
read-merge-write of the whole `state.md`; three at once kept redoing each other's merges at
quadratic cost, and a fourth died midway, leaving an uncommitted half-merge in the shared
worktree that anybody's next commit would have silently swept in. That is why `/end` is now a
critical section with a baton.

## The pieces, and how they fit

**1. Sessions in flight — a ledger of file ownership.** Every session registers one line in
`state.md` at `/start`: when it started, its mission, **which files and repos it owns**, its
session id. Ownership by FILE is the point: two tasks that sound independent collide the
moment they touch the same file. The line is committed *immediately* — a lock nobody can see
is not a lock. A line found without a live session behind it is an orphan: you measure the
WORK it announces (its files, its mandates, `git log`) before declaring it dead, never the
clock — a chat thinking for five minutes looks exactly like a dead one.

**2. `/end` deletes.** The state file answers "what is true now", never "what happened" —
history lives in `git log`, which keeps it better and makes nobody re-read it. So a closed
item is removed, not ticked; a fact that turned out false is replaced, not annotated; and
every surviving entry carries how to re-verify it (a command, a `file:line`, a "measured on
<date>") — an entry nobody can disprove without redoing the work is an opinion with a date.

**3. Tiered reading at `/start`.** The always-read set stays deliberately small: the map, the
project context, the rules, the state. Everything else — architecture, generated maps, field
notes, rule case-law — is opened on demand, often with `grep` rather than whole. Reading
100k tokens before knowing the task doesn't make the model know the project; it makes the
one line that matters compete with the thousand that don't.

**4. Pilot / executive, and briefs as claims.** Some sessions design; others execute. A
design session's output is a **brief** (we call them *mandates*): a file holding a decided
contract — perimeter, contracts, oracles, recommended model — so the executing session
implements instead of re-deciding. But a brief is a CLAIM about a world that keeps moving
while it sits in the queue: the executor re-verifies it before starting, proposes the model,
and **waits for the go**. Briefs are a queue, not a record: executed → deleted; superseded →
deleted as "done by other means".

**5. A bare `/start` proposes work.** Launched with no mission while a planning session is
already registered, a new session doesn't become a second planner: it enumerates the queued
briefs and proposes them **optimized for parallelism** — what can start right now without
touching any live session's files.

**6. The `/end` baton.** First gesture of every `/end`: mark your own ledger line
"/end in progress", commit and push *that marker alone*. A rejected push showing somebody
else's marker means wait. Git itself is the mutex — the triple-/end incident becomes
structurally impossible, with zero coordination outside the repo. And throughout: **one verb
per call** — pull, then look, then edit, then commit, then push, never chained into one
command, because chaining hands the action to an outcome nobody looked at.

**7. Provenance on every commit.** A `Claude-Session: <id>` trailer ties each commit to the
conversation that produced it, so `git log` can answer "which chat did this, and why" months
later. The transcript itself is never committed.

## A day with three chats, concretely

One planning chat reads the full set, grills the human, and writes two mandates. Two
executor chats each `/start <mandate>`: they read the map, the ledger, and THEIR mandate
only; they register disjoint file perimeters, propose a model, wait for the go, and work in
parallel without ever touching the same file. Each `/end` takes the baton in turn, deletes
its own ledger line and everything it made false, and leaves the state file smaller than it
found it. The human launches the next session with a bare `/start` and gets told what can
run now.

## Open point

`main` recently split project creation into `/new-project`, and this branch's `/start` still
carries the older wording. The merge keeps both working; harmonizing the texts is left as a
conversation on the PR rather than decided unilaterally here.
