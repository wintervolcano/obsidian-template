# obsidian-template

A scratch Obsidian vault for research work. It is the skeleton of the vault I use for a
PhD in pulsar astronomy, with all of my own notes taken out: the folders, the templates,
the Bases, the agent rules and the run-logging scripts.

Written up here: [My Obsidian setup, in the age of agents](https://www.fazalkareem.com/blog/2026-11-19-my-obsidian-setup-in-the-age-of-agents).

## Install

1. Download this repo as a zip, or `git clone https://github.com/wintervolcano/obsidian-template.git`
2. Unzip it somewhere, and rename the folder to whatever you want your vault called.
3. In Obsidian, **Open folder as vault** and pick it.
4. Open `START HERE.md`.

## What is in it

| | |
|---|---|
| `_Templates/` | one template per note type, for Templater |
| `_Bases/` | 10 Bases: runs, candidates, processing, issues, observations, projects, tests, meetings, daily, notes |
| `_Scripts/` | `runlog` and `vault-collect`, for logging cluster jobs |
| `AGENTS.md` | the rules an AI agent reads before writing to the vault |
| `.obsidian/` | hotkeys, Templater and daily-note settings, and the two CSS snippets |
| `Example *` notes | demo content so the Bases are not empty. Delete them. |

Install Templater, restart, and **cmd-shift-N** makes any kind of note in the right place.
That is the whole workflow.

## Plugins you will need

The vault works without any of these, but the templates and the home note expect them:

- **Templater** -- note creation from `_Templates/`
- **Tasks** -- the checkbox system and `TASKS.md`
- **Buttons** -- the buttons on `HOME.md`
- **Image captions** -- figure numbering

Bases is core Obsidian and needs nothing installed.

## Licence

MIT. Take any of it, change any of it, no attribution needed.
