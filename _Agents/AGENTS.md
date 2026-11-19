---
type: agents
---
# AGENTS.md -- _Agents

**Your zone. Inside this folder you may do anything.** Logs, scratch output,
command dumps, intermediate files, per-agent subfolders, whatever the work needs.
Nothing here has to follow a schema and nothing here is reviewed.

That freedom is the trade for the rest of the vault staying clean. **Outside
`_Agents/` and `21 RUNS/`, every note you write must follow the `vault-notes`
skill**, in the vault owner's registers and typography. If you cannot load that skill,
write it here instead and say so.

Because agents write only here, and run notes are new files with unique names,
two writers almost never touch the same file. That is the whole concurrency story:
there is no locking, there is just a rule about who writes where.

## `inbox/`

Drafts, write-ups, analysis summaries, anything free-form. Name the file
`YYYY.MM.DD <what it is>.md` and put `author: <your name>` in the frontmatter.

Write it in the register it will end up in, so promoting it is a move and not a
rewrite. The `vault-notes` skill has the registers.

## You may

- Create anything here.
- Link out to any note in the vault.

## You must not

- Promote your own work. Leave it here and say what you left.
- Write into `01 PROJECTS/`, `00 DAILY NOTES/` or `02 MEETINGS/`. Those are the owner's.
- Write anywhere outside this folder without following the `vault-notes` skill.
- Commit a credential, from any note, ever.
- Assume anyone has read it. Say what you left, in your reply.

## Where to go next

Run notes do not belong here. They go to `21 RUNS/`, through `runlog`.
