---
name: end
description: End a session — reconcile state, capture lessons, commit.
disable-model-invocation: true
---
I'm ending a session. Do these five steps in order, starting from 0.

## 0. Take the /end baton — on a shared clone, /end is a critical section

On a clone where other sessions commit, /end is a read-merge-write of the whole `state.md`:
two running at once redo each other's merges until luck converges them, and one interrupted
midway leaves an uncommitted half-/end that anybody's next commit sweeps in. So /end runs
**one at a time**, and git itself is the mutex:

- First gesture, before touching anything else: mark **your own** session line in
  *Sessions in flight* with `⏳ /end in corso`, then commit **that marker alone** and push.
- If the push is rejected, pull `--rebase` and look: if another session's line carries a
  `⏳` marker, **stop and wait** — tell me another /end is running, and retry when its
  marker is gone (its /end deletes its whole line). If no other marker exists, push again.
- Only with your marker visible on the remote do steps 1-4 begin. Your marker dies with
  your line in step 1.
- One verb per call throughout: pull, THEN look, THEN edit, THEN commit, THEN push — never
  chained in one command.

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

Stage **only the files this session touched** — never `git add -A`: a parallel session may have half-finished work in the same tree. Run `git status --short` first; if there are modified files you don't recognise as yours, leave them unstaged and tell me about them.

The commit message is where this session's story lives, so write it as though it were the only record — because it is. What changed, **why**, what it cost, what it taught, and what turned out to be false. Several distinct operations in one chat means several commits, each closing its own.

Every commit carries a `Claude-Session: <session-id>` trailer (read the id from your scratchpad path; ask me if you can't determine it), so the history points back at the conversation that produced it. Never commit the transcript itself.

**Push according to the project's rule, not by reflex.** Where the project says code repos are pushed by hand, commit and stop. If you do push and it is rejected because a parallel session pushed first, `git pull --rebase` and push again; if that surfaces conflicts, stop and show me instead of resolving blindly. If there's no repo or no remote, skip this and tell me the project has no git home yet.

## 4. Tell me what you deleted

Close by naming what came *out* of `state.md`, and why: closed, superseded, or disproved. It is the half of this ritual I cannot see from a diff without reading the whole thing, and it is the half that keeps the file honest.
