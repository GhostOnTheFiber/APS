+++
title = "Using Claude Code as a task planner with git"
date = 2026-02-24
description = "How I orginized my life with markdown files, git, and an AI agent."
[taxonomies]
tags = ["claude-code", "productivity", "git"]
+++

I have a problem, I have 3 companies / projects I am working on and I need to also organize my personal life.

After a suggestion from a colleague, I’ve been experimenting with using [Claude Code](https://docs.anthropic.com/en/docs/claude-code) as an operational task planner across the companies and my personal life. The result is a repo where the file-system is the database and an AI agent is the interface.

## The idea

Most project management tools are more than what I need. At the core, I just want organized lists of tasks and what I need to do today.

I have tried many tools, software, etc. but they either kept getting in my way or where too bulky to quickly use. I am in-favor of simplicity, plain markdown files that follow a template in git. Then Claude Code reads, creates, updates, and moves them. Git takes care of the history and storage.

## How it works

Each company is its own git sub-repo. The root repo holds shared templates and agent instructions.

```
Repo/
├── CLAUDE.md                    # Agent operating instructions
├── AGENTS.md                    # Agent role definitions
├── README.md                    # This file
├── templates/
│   ├── task.md                  # Canonical task template
│   └── wiki.md                  # Wiki page template
├── AttackPointSecurity/
│   ├── legal/         + _archive/
│   ├── finance/       + _archive/
│   ├── engineering/   + _archive/
│   ├── sales-marketing/ + _archive/
│   └── wiki/                    # Reference material
├── Unit-6/
│   ├── legal/         + _archive/
│   ├── finance/       + _archive/
│   ├── engineering/   + _archive/
│   ├── sales-marketing/ + _archive/
│   └── wiki/                    # Reference material
├── Cynaps/
│   ├── legal/         + _archive/
│   ├── finance/       + _archive/
│   ├── engineering/   + _archive/
│   ├── sales-marketing/ + _archive/
│   └── wiki/                    # Reference material
└── Personal/
    ├── tax/           + _archive/
    ├── appointments/  + _archive/
    ├── reminders/     + _archive/
    └── wiki/                    # Reference material
```


### Tasks as files

Tasks are named `{NN}-{slug}.md`, where the number encodes priority:

| Range | Meaning             |
| ----- | ------------------- |
| 00–09 | Critical    |
| 10–29 | High     |
| 30–59 | Medium  |
| 60–89 | Low   |
| 90–99 | Backlog             |

Example: `05-review-client-msa.md` in `AttackPointSecurity/legal/` immediately tells you priority, focus, and purpose. No need to go into a UI and press / configure lots of variables. 

## How I use Claude Code

The `CLAUDE.md` defines the system rules: naming conventions, status values, and workflows. Once loaded, the agent understands the structure.

I can ask things like:

* “What’s critical across all companies?”
* “Create a medium-priority engineering task for Unit-6.”
* “Archive the completed MSA review.”
* “Summarize what’s in progress for AttackPointSecurity.”

The agent reads and edits files directly, and every change is tracked in git. Claude-code is great with working in unix directories and plain files. There is something to being trained on all of the unix code out their that makes it really understand how to work with tree biased file structures.

## Tradeoffs

Lots, its not a full PM tool, there’s no kanban board, burndown chart, or Gantt view. If you need those, use a traditional PM tool.

But for a solo founder or small team, this setup is fast, transparent, and fully under your control.

