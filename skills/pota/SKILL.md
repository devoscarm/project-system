---
name: pota
description: Prune the context prose against its meter. Use it whenever `scripts/context-budget.py` (or `rules-check.py`) is red, when a file of the always-read set feels long, or once a week — files rot without a scheduled pass. Proposes cuts file by file and writes nothing before the human says yes.
---

Prune the project's context prose. The meter decides what is heavy; I decide what goes.

## 1. Read the meter — never your impression

Run the project's meter from the context root (here: `python3 scripts/context-budget.py`). Take
from it, in this order: the reds; the always-read sets and their tokens; the files flagged without
an index; the files with dates in a normative body; then the heaviest durable files. Name the
command you ran and paste its numbers — a pruning argued from memory is an opinion.

## 2. Propose, file by file — and write nothing yet

For each file you propose to touch, one short block:

- **What comes out** — sections or lines, by heading or by `file:line`.
- **Where it goes** — deleted (history is already in git: give the command that finds it), moved
  to the on-demand file that owns that kind of truth, or reduced to a pointer.
- **What is lost** — say it plainly, even when the answer is «nothing that git does not keep».

Apply the three forms (rule 80 where the project numbers it): every surviving line is an imperative
with its check, a present fact with the command that re-measures it, or a pointer. Story is not a
form. A count that changes with every merge is replaced by its command and the invariant it must
give. Before calling a line false, re-run its handle.

Never renumber an identifier that is cited elsewhere (rules, traps, voices): shorten it, or merge it
under the lower number and leave the line `N. → merged into M`.

Then stop, and ask me which blocks to apply.

## 3. Apply only what I approved, then ratchet

Apply exactly the approved blocks, nothing else. Run the project's gate (here:
`python3 scripts/rules-check.py`) until green, then the ratchet (here:
`python3 scripts/context-budget.py --stringi`) so the ground gained cannot be lost, and commit the
pruned files together with the thresholds file — one commit per file or per coherent block, staged
by name, following the project's commit and push rules.

Close by telling me the before/after of each touched file (lines and tokens) and of each set.
