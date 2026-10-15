---
name: end
description: End a session — reconcile state, capture lessons, commit.
disable-model-invocation: true
---
I'm ending a session. Do these five steps in order, starting from 0.

**My words in this conversation beat every line below** (rule 319 where the project numbers it):
if a step says to do something I have just told you not to, do what I said and name the step
you skipped.

## 0. The order: the code first, then the shared prose — one /end at a time

/end does two kinds of work, and only one of them has to queue.

- **Closing the CODE** — step 2-ter's review, merging main INTO your branch, the project's gate on
  that tip, step 3-bis's close — touches your own working tree and, for seconds, the repo's main.
  Parallel /ends do it side by side; the project's close command queues the merges by itself
  (here: `scripts/session.sh close`, a lock per repo) and refuses while main is not inside your
  branch, so a conflict is always resolved in YOUR tree, never in the shared main. If it refuses
  (main moved, a migration number, another session's commits inside yours), fix what it names in
  your tree — re-running the gate if what main brought touches your work — and re-run.
- **Writing the SHARED PROSE** — steps 1, 2, 2-bis, 3's commit of the context repo, 4 — is a
  read-merge-write of files every /end rewrites. Where that clone is shared, two at once redo each
  other's merges, and one interrupted midway leaves a half-/end anybody's next commit sweeps in.

So:

1. **The code first, holding nothing**: 2-ter, the gate, 3-bis.
2. **Then take the baton, in the background** (here: `scripts/session.sh baton take <id>`): it
   returns when it is your turn, saying once «in coda dietro <id>» if it is not. You are woken when
   it exits; never ask me to retype the /end. If it reports a TAKEOVER from a dead /end, read what
   that one left uncommitted before you write anything.
3. **Steps 1 → 2 → 2-bis → 3 (the context repo) → 4, then drop it** (here: `baton drop <id>`)
   right after the context push. If you must stop halfway (a red you cannot fix, a question for
   me), KEEP it and tell me where you stopped: the next taker is let in after a lease (here: 30
   min) and is shown your uncommitted files — whoever takes the baton reads those before writing.

Where the project has no baton command, the fallback is git as the mutex: mark your own lock
`⏳ /end in corso`, commit that alone, push, and look for another `⏳` before step 1. One verb per
call throughout: pull, THEN look, THEN edit, THEN commit, THEN push — never chained.

## 1. Reconcile state.md — which means deleting, not only writing

**Re-read `state.md` from disk first — never trust the copy in your context.** If it changed since `/start` read it (a parallel session ended before this one), merge: keep what you don't recognise, weave this session's outcome into it, and tell me what you merged. Overwriting the unknown is how parallel sessions erase each other.

`state.md` is a **state**, not a log: if something is written there it is true, and if it is not written there it is not true. History lives in `git log` — which is why step 3's commit message is where this session's story goes, and why nothing here needs to record that the session happened at all.

So the work is three moves, and the first is the one that gets skipped:

- **Delete what is no longer true.** Walk the file entry by entry. An item this session closed is **removed**, not ticked — `✅ DONE`, `~~struck through~~` and "original text:" all belong to a log, and keeping them is how a file grows forever while nobody dares prune it. A fact that turned out to be false is **replaced**, not annotated.
- **Verify before you keep.** An entry you neither closed nor touched is a *claim*, not furniture: it carries the authority of having been checked once, by somebody who may have been wrong. Where re-checking is cheap, re-check it, and be ready to delete it as **false** rather than carry it another session. Where it is not cheap, keep it — but make sure it says how it *could* be re-checked (a command, a `file:line`, "measured on <date>", "asked on <date>"). An entry nobody can disprove without redoing the work that produced it is an opinion with a date on it.
- **Then write what changed** — the present tense of the world, with no session numbers in it.

**Respect the budget — it is measured, not hoped for.** Where the project keeps a context meter
(here: `python3 scripts/context-budget.py`), every file has thresholds that can only go down, and
step 2-bis's check fails if this session pushed one up. What will not fit is almost always history
(already in git) or stable knowledge (it belongs in the on-demand files). Every surviving line has
one of three forms: an imperative with its check, a present fact with the command that re-measures
it, or a pointer. A count that changes with every merge is not written — its command and invariant are.

**Sessions in flight**: remove this session's line, and leave any other lines alone — their sessions will clear them. Do not write a post-mortem of your own session there. If it taught something, that is step 2's business; if it did not, the commit message is enough.

**Briefs** (what the project may call *mandates* or *work orders* — a decided contract handed to an executing session): they are a **queue**, not a record, and they obey the same law as the rest of this file.

- **A brief this session executed is deleted**, file and queue line together, in this commit. It is not ticked and not archived: what it produced is in the code, and why it existed is in the commit message. A brief left behind after its work is done will be executed twice.
- **And its name leaves every other brief's order line**, in the same commit (here:
  `scripts/session.sh release <brief>`, then commit the files it prints) — also when the brief
  stays open for my live check: what frees a dependent is your code on main, not my eye. A brief
  it SKIPS has its owner's edits in flight: message that owner (its lock's address) instead.
- **A brief this session did NOT execute is re-verified before it is left in the queue** — and this is the move that gets skipped. Other sessions have been building all along, so parts of it may already exist, or its premise may be gone. Where checking is cheap, check; then prune it to what is still true, or delete it as **done by other means**. A brief that quietly rots is worse than a stale note, because someone will hand it to a fresh session and it will be *obeyed*.
- **If this session wrote briefs for others**, make sure each one carries what its executor needs before it can start: the file perimeter it owns, its sequencing constraints against the other open briefs, and its recommended model — so the executing session can propose and wait rather than guess.
- ⚠️ **And every brief declares three things, each in a section of its own — the check refuses
  it otherwise**: what ALREADY EXISTS (what you searched, what you found, even «nothing»); its
  PERIMETER (`## Perimetro`, the paths it owns) and ORDER (`**Seriale dopo:**` a brief or
  `nessuno`); and the ALTERNATIVES it weighed, with why the chosen road holds in ten years — a
  shortcut is written *as* a shortcut, with its price, even when the human just approved it.

⚠️ And be honest about a session that has not ended: `/end` is a ritual, not a boundary. Work often continues after it. When it does, what you wrote here is now wrong and gets updated alongside the work that contradicts it — a self-assessment written at `/end` is the one part of the file nobody re-reads, because it reads like a summing-up.

## 2. Capture the lesson — and it almost never belongs in the law

**Default: this session produced no law.** A law is what binds EVERY session whatever it is doing;
what you learned today is almost always a *trap* — narrow, biting only while somebody touches that
one thing. Writing a trap into the law is how a rules file reaches 222 entries that nobody can hold
and therefore nobody follows: measured on this system on 2026-09-02, growing ~4.5 rules a day and
never once shrinking.

So the lesson has **three possible homes**, and you pick before you write — and **most lessons
need none of them**: they stay in the commit message, or they become a mechanism (a test, a check,
a hook), which beats any sentence.

- **A trap** → the project's `traps.md` (`shared/traps.md` only if it binds every project), with
  the next free number that `rules-check` prints. It is grepped when the work reaches that area,
  never read at `/start`. **Only if** the defect cost us TWICE and cannot become a mechanism; grep
  the area first, and if it resembles an entry that exists, widen that one.
- **A case** → `rule-cases.md`, under the same number — only for a new LAW: what happened, what
  it cost, the shape of the defect, at most 25 lines. The check below refuses a law that arrives
  without one. A trap carries its own story inside its text, in a clause, never as a dated trailer.
- **A law** → `rules.md`, and ONLY if it passes two tests: it binds every session regardless of
  task, AND it comes **with the mechanism that makes it bite**. The file has a hard ceiling: to add
  one past it, one comes out. Each law declares ⚙️ (a check that goes red), 🔒 (a structure that
  refuses) or 👁️ (nothing) — and the count of 👁️ may only go DOWN. A law you can only mark 👁️ is
  a law you already know will be violated: write the check, or write a trap instead.

⚠️ **A rule that PRESCRIBES without a MECHANISM does not bite** (rule 214). The proof is in this
system's own history: the three rules never violated in a month are the three that *refuse* — the
CI on push, the grants replayed by 27 suites, the boot that will not start on a bad allowlist. The
prose ones were violated, every one, by sessions that had read them that same morning.

⚠️ **Numbers are identifiers, not positions.** They are cited in `state.md`, in commit messages, in
mandates and in git history you cannot edit. Prune freely; never renumber. The sequence is allowed
to have gaps and suffixes — leave it ugly.

### 2-bis. Run the check — it is what makes step 2 real

Where the project keeps one (here: `python3 scripts/rules-check.py`, from the context root), run
it and **do not close the session while it is red**. It names the exact element that is missing —
a reused number, a law over the ceiling, a law with no mechanism marker, a rule with no case, a
`regola N` citation that no longer resolves, a state file that grew past its budget, a brief that
does not say what already exists or which alternatives it weighed. Fix what it
names; if you believe a red is wrong, say so to me rather than editing the check to be quiet.

**Then ratchet.** Once it is green, run the meter's ratchet (here:
`python3 scripts/context-budget.py --stringi`): every threshold this session brought down is
locked where it now stands. The updated thresholds file goes into this session's commit.

If the project keeps a scoreboard of remaining work, update it **only if something actually changed state**, and re-run the command that measures it instead of deducing the number from your own work.

### 2-ter. If this session changed code: have it reviewed by eyes that did not write it

The gates catch what a tool can see with ~0 false positives. Four principles the human chose to
hold as REVIEW QUESTIONS instead (here: doctrine 11, «how it is held»; the verdicts in
`osan-core/audit-qualita-2026-10.md` §4) are asked nowhere else — so they are asked here, before
the worktree merges, every time code changed. Never by you: you wrote the code, and you will read
your intent instead of the text.

Spawn ONE sub-agent with a fresh context. Give it the diff (`git diff main...HEAD` in your
worktree, per repo), doctrine 11, and these four questions, to be answered per question with
`file:line` and the concrete case, or «nothing»:

1. **Built but not asked for** — an abstraction with one implementation, a parameter nobody
   passes, a setting with one value, code for a case the business does not have. (Contemplated is
   not built: doctrine 9.)
2. **A decision away from the core** — a permission, a policy or a state transition decided inside
   a repository, a controller, a component or an I/O adapter, where a pure function tested alone
   belongs.
3. **A state the business forbids, still representable** — a string where a closed union belongs,
   two flags that cannot both be true, a field required in one state and optional in the type, an
   `else` or `default` that swallows the unknown case.
4. **A comment that claims what the code does not enforce** — an inventory («every X», «the
   ordered list of»), a promise about order or exclusivity, a name of something gone. Replace it
   with a test that enumerates, or delete it.

Then, for every finding: fix it in this session, or write in the commit message why it stands.
The answer «nothing» on all four is a valid outcome — say so in the commit message too, so the
review leaves a trace. A session that changed no code skips this step.

## 3. Commit — and push only where the project says to

**In a shared working tree, stage only the files this session touched — never `git add -A`**: a parallel session may have half-finished work in the same tree. Run `git status --short` first; if there are modified files you don't recognise as yours, leave them unstaged and tell me about them.

⚠️ `git commit --only <path>` protects you from the shared INDEX, never from the shared FILE: of
that path it commits whatever the working tree holds, whoever wrote it. When a path you must
commit also holds somebody else's edits, build your content and put it in the index directly
(`git hash-object -w` + `git update-index --cacheinfo`), leaving their working tree untouched.

**In your own session worktree none of this applies**: `git add -A` and `git commit -a` are correct
there, because nothing in that tree belongs to anyone else.

The commit message is where this session's story lives, so write it as though it were the only record — because it is. What changed, **why**, what it cost, what it taught, and what turned out to be false. Several distinct operations in one chat means several commits, each closing its own.

Every commit carries a `Claude-Session: <session-id>` trailer (read the id from your scratchpad path; ask me if you can't determine it), so the history points back at the conversation that produced it. Never commit the transcript itself.

**Before pushing the context repo, sum to zero** (rule 320, where the project has it): run
`python3 scripts/context-budget.py --somma-zero origin/main`. If this session's commits add net
lines to the always-read set, either prune as much in the same set (one more commit — never
`--amend`), or, if the growth is deliberate, add an empty commit whose message carries the line
`Deroga somma zero: <why>`, run the ratchet again, and commit the thresholds it raised. Never edit
a threshold up by hand to make the red go away.

**Push according to the project's rule, not by reflex.** Where the project says code repos are pushed by hand, commit and stop. If you do push and it is rejected because a parallel session pushed first, `git pull --rebase` and push again; if that surfaces conflicts, stop and show me instead of resolving blindly. If there's no repo or no remote, skip this and tell me the project has no git home yet.

## 3-bis. Hand your work back — close the worktree

If this session had its own working tree, close it the way the project says (it will have one
command) — in step 0's first phase, before the baton. Two things that are not optional:

- **Commit everything first.** Closing a worktree with uncommitted work throws that work away.
- **The re-entry never rewrites commits.** A merge keeps every SHA, every author, every
  `Claude-Session` trailer exactly as written — and SHAs are cited as re-check handles all over
  the context files, by sessions that wrote them while they worked. A rebase would silently make
  every one of those citations false. If the merge conflicts, resolve it and say what you chose;
  do not take either side blindly.

If this session worked in a shared clone instead, there is nothing to close here.

## 4. Tell me what you deleted

Close by naming what came *out* of `state.md`, and why: closed, superseded, or disproved. It is the half of this ritual I cannot see from a diff without reading the whole thing, and it is the half that keeps the file honest.
