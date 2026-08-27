---
name: end
description: End a session — reconcile state, capture lessons, commit.
disable-model-invocation: true
---
I'm ending a session. Do these five steps in order, starting from 0.

## 0. Take the /end baton — where the clone is still shared

/end is a read-merge-write of the whole state file. Where that file lives in a clone other
sessions also commit to, two /end runs at once redo each other's merges until luck converges
them, and one interrupted midway leaves an uncommitted half-/end that anybody's next commit
sweeps in. There, /end runs **one at a time**, and git itself is the mutex:

- First gesture: mark **your own** lock with `⏳ /end in corso`, commit **that alone**, push.
- If the push is rejected, pull `--rebase` and look: another lock carrying `⏳` means **stop and
  wait** — tell me another /end is running, and retry when its marker is gone. No other marker:
  push again.
- Only with your marker visible on the remote do steps 1-4 begin. It dies with your lock in step 1.
- One verb per call throughout: pull, THEN look, THEN edit, THEN commit, THEN push — never chained.

⚠️ **This is about the file, not about the ritual.** Where the project gives each session its own
working tree, the CODE needs none of this — your commits are yours by construction. Where locks
are one file per session, the flight list needs none of it either: you delete your own file and
nobody else's. What still needs the baton is the shared prose that every /end rewrites.

## 1. Reconcile state.md — which means deleting, not only writing

**Re-read `state.md` from disk first — never trust the copy in your context.** If it changed since `/start` read it (a parallel session ended before this one), merge: keep what you don't recognise, weave this session's outcome into it, and tell me what you merged. Overwriting the unknown is how parallel sessions erase each other.

`state.md` is a **state**, not a log: if something is written there it is true, and if it is not written there it is not true. History lives in `git log` — which is why step 3's commit message is where this session's story goes, and why nothing here needs to record that the session happened at all.

So the work is three moves, and the first is the one that gets skipped:

- **Delete what is no longer true.** Walk the file entry by entry. An item this session closed is **removed**, not ticked — `✅ DONE`, `~~struck through~~` and "original text:" all belong to a log, and keeping them is how a file grows forever while nobody dares prune it. A fact that turned out to be false is **replaced**, not annotated.
- **Verify before you keep.** An entry you neither closed nor touched is a *claim*, not furniture: it carries the authority of having been checked once, by somebody who may have been wrong. Where re-checking is cheap, re-check it, and be ready to delete it as **false** rather than carry it another session. Where it is not cheap, keep it — but make sure it says how it *could* be re-checked (a command, a `file:line`, "measured on <date>", "asked on <date>"). An entry nobody can disprove without redoing the work that produced it is an opinion with a date on it.
- **Then write what changed** — the present tense of the world, with no session numbers in it.

**Respect the file's budget.** If `state.md` states a line budget in its header comment, measure it (`wc -l`) and prune until it fits. What will not fit is almost always history, and history is already in git. A budget nobody measures is an exhortation, and exhortations do not survive fifty sessions: this file once grew past a thousand lines while its own header said to prune freely.

**Sessions in flight**: remove this session's line, and leave any other lines alone — their sessions will clear them. Do not write a post-mortem of your own session there. If it taught something, that is step 2's business; if it did not, the commit message is enough.

**Briefs** (what the project may call *mandates* or *work orders* — a decided contract handed to an executing session): they are a **queue**, not a record, and they obey the same law as the rest of this file.

- **A brief this session executed is deleted**, file and queue line together, in this commit. It is not ticked and not archived: what it produced is in the code, and why it existed is in the commit message. A brief left behind after its work is done will be executed twice.
- **A brief this session did NOT execute is re-verified before it is left in the queue** — and this is the move that gets skipped. Other sessions have been building all along, so parts of it may already exist, or its premise may be gone. Where checking is cheap, check; then prune it to what is still true, or delete it as **done by other means**. A brief that quietly rots is worse than a stale note, because someone will hand it to a fresh session and it will be *obeyed*.
- **If this session wrote briefs for others**, make sure each one carries what its executor needs before it can start: the file perimeter it owns, its sequencing constraints against the other open briefs, and its recommended model — so the executing session can propose and wait rather than guess.

⚠️ And be honest about a session that has not ended: `/end` is a ritual, not a boundary. Work often continues after it. When it does, what you wrote here is now wrong and gets updated alongside the work that contradicts it — a self-assessment written at `/end` is the one part of the file nobody re-reads, because it reads like a summing-up.

## 2. Capture the lesson — only if one emerged

If this session produced a rule or lesson that generalises beyond this project, add it to `rules.md` under **From this project**. If nothing generalisable came up, leave the file alone — don't pad it.

Keep the rule itself to a few lines: the imperative, plus **the tell** — what you will be doing when it is about to bind. If the project keeps a `rules-cases.md`, the story of what happened goes *there*, under the same number. A rule that grows past a handful of lines has become a case wearing a law's clothes, and it will then be re-read in full at the start of every future session.

⚠️ **Numbers are identifiers, not positions.** They are cited in `state.md`, in commit messages, in the architecture document and in git history you cannot edit. Prune freely; never renumber. The sequence is allowed to have gaps and suffixes — leave it ugly.

If the project keeps a scoreboard of remaining work, update it **only if something actually changed state**, and re-run the command that measures it instead of deducing the number from your own work.

## 3. Commit — and push only where the project says to

**In a shared working tree, stage only the files this session touched — never `git add -A`**: a parallel session may have half-finished work in the same tree. Run `git status --short` first; if there are modified files you don't recognise as yours, leave them unstaged and tell me about them.

⚠️ And know the limit of that care, because it is not obvious and it has already cost a night:
`git commit --only <path>` protects you from the shared INDEX, never from the shared FILE. Of that
path it commits whatever is in the working tree, whoever wrote it — so another session's
half-finished work can reach HEAD *without* the files that make it compile. When a path you must
commit also holds somebody else's edits, do not commit the path: build the content you mean and
put it in the index directly (`git hash-object -w` + `git update-index --cacheinfo`), leaving
their working tree untouched.

**In your own session worktree none of this applies**: `git add -A` and `git commit -a` are correct
there, because nothing in that tree belongs to anyone else.

The commit message is where this session's story lives, so write it as though it were the only record — because it is. What changed, **why**, what it cost, what it taught, and what turned out to be false. Several distinct operations in one chat means several commits, each closing its own.

Every commit carries a `Claude-Session: <session-id>` trailer (read the id from your scratchpad path; ask me if you can't determine it), so the history points back at the conversation that produced it. Never commit the transcript itself.

**Push according to the project's rule, not by reflex.** Where the project says code repos are pushed by hand, commit and stop. If you do push and it is rejected because a parallel session pushed first, `git pull --rebase` and push again; if that surfaces conflicts, stop and show me instead of resolving blindly. If there's no repo or no remote, skip this and tell me the project has no git home yet.

## 3-bis. Hand your work back — close the worktree

If this session had its own working tree, close it the way the project says (it will have one
command). Two things that are not optional:

- **Commit everything first.** Closing a worktree with uncommitted work throws that work away.
- **The re-entry never rewrites commits.** A merge keeps every SHA, every author, every
  `Claude-Session` trailer exactly as written — and SHAs are cited as re-check handles all over
  the context files, by sessions that wrote them while they worked. A rebase would silently make
  every one of those citations false. If the merge conflicts, resolve it and say what you chose;
  do not take either side blindly.

If this session worked in a shared clone instead, there is nothing to close here.

## 4. Tell me what you deleted

Close by naming what came *out* of `state.md`, and why: closed, superseded, or disproved. It is the half of this ritual I cannot see from a diff without reading the whole thing, and it is the half that keeps the file honest.
