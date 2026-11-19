---
type: agents
---
# AGENTS.md -- vault root

Read this file, then the `AGENTS.md` of the folder you are writing into, then the project
hub if you are working on a project. **That is your whole context budget.** Do not read
the vault, do not grep it broadly, and do not load a folder to get oriented.

## The one rule

**Folder is where a note sits. `type` is what it is. Bases filter on `type`, never on
folder.** A note in the wrong folder still appears everywhere it should, so file by
convention and do not reorganise.

## Where things are

| Folder | Holds | `type` |
|---|---|---|
| `00 DAILY NOTES/` | one note per day, the owner's planning | `daily` |
| `01 PROJECTS/` | one folder per project, hub named `_<Project>.md` | `project` |
| `02 MEETINGS/` | **minutes are frozen records** | `meeting` |
| `03 RESEARCH PAPERS/` | **a reference manager owns this. Never write here.** | `literature` |
| `04 OBSERVATIONS/` | one note per observing session | `observation` |
| `05 PROCESSING/` | one note per target | `processing` |
| `06 TESTS/` | parameter sweeps and method tests | `test` |
| `07 CONCEPTS/` `08 METHODS/` | theory notes; command cookbooks | `concept` `method` |
| `19 ISSUES/` | what broke and the fix, findable by symptom | `issue` |
| `20 CANDIDATES/` | one note per kept candidate | `candidate` |
| `21 RUNS/` | **shared.** One note per cluster job | `run` |
| `_Agents/` | **your zone.** Drafts for the owner to promote | |
| `_Templates/` `_Bases/` `_Archive/` | infrastructure. **Do not write here.** | |

## Routing

| You are doing | Read | Write to |
|---|---|---|
| launching or finishing a cluster job | `21 RUNS/AGENTS.md` | `21 RUNS/`, via `runlog` |
| drafting a write-up | `_Agents/AGENTS.md` | `_Agents/inbox/` |
| recording a processing run | `05 PROCESSING/AGENTS.md` | append to the existing note |
| a candidate worth keeping | `20 CANDIDATES/AGENTS.md` | a new candidate note |
| a bug you diagnosed | `19 ISSUES/AGENTS.md` | a new issue note |
| a reusable command | `08 METHODS/AGENTS.md` | append to the right cookbook |

## Write zones

| Zone | Written by |
|---|---|
| `_Agents/**` | agents, freely, no schema |
| `21 RUNS/**` | `runlog` |
| everywhere else | the owner, and agents only under the rules below |

## How you write

- **Append, never rewrite.** You may create new notes and append timestamped entries. You
  may not rewrite existing prose, reorder sections, or tidy anything. An entry is
  `**[YYYY-MM-DD @HH:MM:SS]**` from the real clock, newest at the top.
- **Say who you are.** Every note or entry you create carries `author: <your name>` in
  frontmatter, or names you in the entry.
- **Never invent** a timestamp, a path, a flag, a number, or a method description. If you
  do not know it, leave it out.
- **Typography.** No em dash, no en dash in ranges, no unicode minus, no superscript
  powers (`1e-15`, not the glyph), no space-separated thousands. The aside connector is
  ` -- `. These are the tells that mark text as machine-written at a glance.
- **Commands go in whole**, with the `singularity exec` or `sbatch` wrapper and every
  flag. Never abridge with `...`. Paste tool output rather than summarising it.
- **Record failures.** A note containing only successes is wrong.
- **No hedging.** "efac_dm brings the chisq down to 1.45, which is not alright" beats
  "this may suggest the model is sub-optimal".

## What you must never do

- Write into `03 RESEARCH PAPERS/`, `_Templates/`, `_Bases/`, `.obsidian/`.
- Rewrite meeting minutes. They are a record of what was said.
- Resolve a `> [!question] #q` on your own. Surface it, leave it open.
- Delete anything. Move it to `_Archive/` and say so.
- Change frontmatter you were not asked to change, especially `created` and `type`.
- Write a summary of the owner's own notes back into the vault.
- Commit credentials.
