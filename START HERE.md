---
type: note
tags:
  - vault-admin
---
# Start here

This is a scratch vault. Nothing in it is precious, including the parts I am about to
tell you are important.

## The one rule

> **Folder is where a note sits. `type` is what it is. Bases filter on `type`, never on folder.**

A note filed in the wrong folder still turns up everywhere it should. This is the rule
that keeps the system alive, because it means there is never a wrong answer available to
you. Put the note anywhere, set `type`, carry on.

## First five minutes

1. Install **Templater** and restart Obsidian. The settings ship with this vault, so it
   comes up already pointed at `_Templates/` with the note picker bound to **cmd-shift-N**
   (ctrl-shift-N on Windows and Linux).
2. Press **cmd-shift-N**. It asks what kind of note you want, then makes it in the right
   folder, with the right name and the right frontmatter. This is the one key worth
   learning. Everything else in this vault is optional.
3. Install **Tasks**. Any `- [ ]` anywhere gathers into `TASKS.md`.
4. Open `HOME.md`. Install **Buttons** if you want the buttons to work.
5. Open any `.base` file in `_Bases/`.

If cmd-shift-N does nothing, Templater is not enabled yet, or the key is taken by
something else on your system. Settings -> Hotkeys, search for *New note*, and rebind it.

## What ships in `.obsidian/`

Enough config that the vault works on first open, and nothing else:

| File | Does |
|---|---|
| `hotkeys.json` | cmd-shift-N, and a handful of editor keys |
| `plugins/templater-obsidian/data.json` | points Templater at `_Templates/` and registers the picker |
| `daily-notes.json` | daily notes land in `00 DAILY NOTES` as `YYYY.MM.DD` |
| `appearance.json` | turns on the `paper` and `dashboard` snippets |
| `app.json` | attachments go in the nearest `attachments/` folder |

No theme is set, so you get whatever you already use. Installing a plugin will not
overwrite these, Obsidian leaves `data.json` alone and only writes the plugin code.

## The folders

Numbered so the sidebar stops reordering itself. They are a shelf, not a taxonomy.

| Folder | Holds | `type` |
|---|---|---|
| `00 DAILY NOTES/` | one note per day | `daily` |
| `01 PROJECTS/` | one folder per project, hub named `_<Project>.md` | `project` |
| `02 MEETINGS/` | weekly, journal club, group | `meeting` |
| `03 RESEARCH PAPERS/` | let Zotero own this one | `literature` |
| `04 OBSERVATIONS/` | one note per observing session | `observation` |
| `05 PROCESSING/` | one note per target, the narrative | `processing` |
| `06 TESTS/` | parameter sweeps and method tests | `test` |
| `07 CONCEPTS/` `08 METHODS/` | theory; command cookbooks | `concept` `method` |
| `09 WRITING/` | thesis, papers, proposals | `writing` |
| `19 ISSUES/` | what broke and the fix, findable by symptom | `issue` |
| `20 CANDIDATES/` | one note per candidate worth remembering | `candidate` |
| `21 RUNS/` | one note per cluster job, written by `runlog` | `run` |
| `_Agents/` | where AI agents are allowed to write | |
| `_Bases/` `_Templates/` `_Scripts/` | infrastructure | |

Gaps in the numbering are deliberate. My own vault has personal folders at 10 to 18 that
are nobody's business, and leaving the numbers free means you can add your own without
renaming anything.

## Properties

Thin on purpose. Two rules that will save you an afternoon:

- **Dates need hyphens.** `date: 2026-09-22`. Write `2026.09.22` and Obsidian reads it as
  a string, and every sort built on it silently stops working. Filenames use dots,
  properties use hyphens.
- **`created` and `date` are different questions.** `created` is when you wrote the note,
  `date` is when the thing happened. An observation on the 27th written up on the 30th
  needs both.

Leave a field blank rather than guess. A field you do not maintain is worse than no field,
because it makes every view built on it look authoritative when it is lying.

## Callouts

The six that carry the weight:

```markdown
> [!method]       how it was done
> [!result]       what came out
> [!limitation]   the caveat you will need at paper time
> [!definition]   a term, pinned down
> [!hypothesis]   what you think is going on
> [!source]       where it came from
```

`[!limitation]` is the one to adopt first. It is where the caveat goes that a referee
will find in eighteen months.

A question is not a task:

```markdown
> [!question] #q
> Why does this model prefer EQUAD over ECORR?
```

A task gets done and deleted. A question gets answered, and the answer belongs next to
the question. Delete the `#q` when you work it out and leave the callout. Search `tag:#q`
for everything still open.

## Reports

Put `cssclasses: [paper]` in the frontmatter and the note renders as a paper: narrow
column, numbered sections, full-width figure cards. `Example report.md` shows the shape.

The one device worth stealing: under every figure, an italic `*Plot generation method:*`
line giving the exact recipe. It costs thirty seconds and it answers the "wait, what is
actually plotted here" that arrives four months later.

Write section headings as questions. *Is the red noise a power law?* is a link target
that lands on the plot answering it.

## Logging cluster runs

`_Scripts/runlog` writes a run note from the cluster. You type:

```bash
sbatch run_search.sh
runlog
```

It lists your live jobs, you pick one, and it writes the note: command from your shell
history, job id and state from one call to the scheduler, cluster from the hostname.
`runlog --end` closes it with the exit code. Nothing polls and nothing runs in the
background.

It needs [atuin](https://atuin.sh) for the command history, and either slurm or HTCondor.
Set `RUNLOG_OUTBOX` to a folder on the cluster, make that folder a small private git repo,
and run `vault-collect` on your own machine to drain it into `21 RUNS/`.

The reason it is a separate repo: cluster scratch disks are often readable by every user
on the machine. Do not clone your notes there.

## Agents

`AGENTS.md` at the root is what an AI agent reads before it writes anything, plus the one
in whatever folder it is writing to. That is its whole context budget. Pointing a model at
your entire vault is expensive and makes it worse.

`_Agents/inbox/` is a drafts folder. Agents write there, you promote by hand.

Read `AGENTS.md` and change it to suit. The parts I would keep: append rather than
rewrite, never close a question, never invent a number or a path, and leave a field blank
rather than fill it in with a guess.
