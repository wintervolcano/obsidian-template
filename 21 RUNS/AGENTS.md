---
type: agents
---
# AGENTS.md -- 21 RUNS

The registry of long jobs on the clusters. **You and agents both write here.**
One note per job, created when it is launched and updated when it ends.

One file per job means two writers never touch the same file, which is what makes
this safe to share.

## Read before writing

Nothing in this folder. To find a run, filter `Runs.base`, or match on `job_id`.
Never read the folder to orient yourself.

## Naming

`YYYY.MM.DD <cluster> <job_id> <target>.md`

The job id in the filename means `cmd+O` and typing the id finds it.

## Frontmatter

```yaml
type: run
project: "[[_Example Project]]"  # wikilink to the project hub
target: EXAMPLE-0000
cluster: hercules             # whatever you call your machines
scheduler: slurm              # slurm | condor | nextflow | shell
job_id: "4471829"             # quoted, ids can have leading zeros
script: run_search.sh
revision: 642a062             # commit, branch or pipeline revision
started: 2026-10-24T09:12:00
ended:                        # fill on exit
status: running               # queued running done failed killed
outdir: /scratch/you/EXAMPLE-0000/search_01/
launched_by: you             # or the agent name
```

`status` is the job's state, and it is the only status field in the vault. A
processing note has no `stage` field on purpose: a status you have to remember to
advance by hand is a status that will be wrong, and a dashboard built on it will
be confidently wrong.

## Body

A `## Log` heading, then timestamped entries, newest first:

```markdown
**[2026-01-16 @14:02:11]** Finished. 136 chunks per beam, 3 beams. Candidates in `CANDS/`.

**[2026-01-16 @09:12:03]** Submitted. `sbatch run_search.sh`, 4 nodes, 12 h limit.
```

## How notes get here

Nobody writes into this folder from a cluster, and no cluster holds a clone of
the vault. Cluster scratch is often readable by every user on the machine.

On a cluster, `runlog` writes to `~/vault-outbox/` and pushes that outbox repo,
which contains run notes and nothing else. On your own machine, `vault-collect`
pulls the outbox and files the notes here. The outbox is append-only from the
clusters and your machine never writes to it, so the pull is always a
fast-forward and cannot conflict.

```
runlog                      pick from your live jobs, write the note
runlog --job 4471829        skip the picker
runlog --note "..."         append to the most recent note
runlog --end                close it, pulling exit code from sacct
runlog --day                today's commands from atuin, as a draft
```

An agent working on a cluster calls `runlog` too, with `RUNLOG_AGENT` set to its
own name. It does not write markdown here by hand.

A note you have edited in Obsidian is never overwritten by a later collect. If
the cluster also appended to it, `vault-collect` reports it as HELD and leaves
both copies alone.

## You may

- Create a run note when you launch a job.
- Append a timestamped entry at any point.
- Set `ended` and change `status` on exit.

## You must not

- Rewrite or reorder existing entries.
- Change `started`, `job_id`, `cluster` or `launched_by` after creation.
- Delete a run note. A failed run is the most useful kind.
- Create a run note for something that finishes in seconds. This is for jobs you
  would otherwise lose track of.

## Where to go next

- The run produced candidates worth keeping, go to `20 CANDIDATES/AGENTS.md`.
- The run broke in a way worth remembering, go to `19 ISSUES/AGENTS.md`.
- The data moved along the pipeline, append to the note in `05 PROCESSING/`.
