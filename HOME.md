---
type: dashboard
cssclasses:
  - dashboard
---
```button
name Commit and sync
type command
action Git: Commit-and-sync
color green
```
```button
name Today
type command
action Daily notes: Open today's daily note
```

[[START HERE]] · [[TASKS]] · [[AGENTS.md|Agent rules]]

## Running now

Written by `runlog` on the clusters. `vault-collect` on your own machine brings them in.

![[Runs.base#In flight]]

## Waiting for me

Drafts an agent has left for you.

```query
path:"_Agents/inbox"
```

## Projects

![[Projects.base#Active]]

---

> [!question]- Open questions
> ```query
> tag:#q -path:"_Templates"
> ```

> [!bug]- Runs that failed
> ![[Runs.base#Failed]]

> [!success]- Runs finished
> ![[Runs.base#Finished]]

> [!list]- Observations
> ![[Observations.base#All]]

> [!info]- Meetings
> ![[Meetings.base#Meetings]]
