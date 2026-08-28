# My Agent Skills

A personal collection of [Agent Skills](https://agentskills.io/home).

## Installation

```bash
pnpx skills add makursi/choria-skills --skill='*'
```

Or install a single skill from its standalone repo:

```bash
pnpx skills add makursi/skill-notes-refiner
pnpx skills add makursi/skill-document-style
```

## Skills

| Skill | Description | Standalone Repo | Source of Truth |
|-------|-------------|-----------------|-----------------|
| [notes-refiner](skills/notes-refiner) | Refine scattered Computer Science notes into structured, academically-deep knowledge base documents | [skill-notes-refiner](https://github.com/makursi/skill-notes-refiner) | This collection (standalone repo is a publish channel) |
| [document-style](skills/document-style) | Proofread Chinese technical documentation against a writing style guide: audit / revise / update | [skill-document-style](https://github.com/makursi/skill-document-style) | Standalone repo (synced in via vendor submodule) |

## Repository Structure

```
.
├── AGENTS.md               # Skill generation & maintenance workflow
├── README.md
├── vendor/                 # Submodules of synced skills' source repos
│   └── {skill-name}/
└── skills/
    └── {skill-name}/       # One directory per skill (kebab-case)
        ├── SKILL.md        # Skill entry (frontmatter + overview + triggers)
        ├── SYNC.md         # Synced skills: source path + git SHA
        └── references/     # On-demand detailed instructions
```

Hand-written skills live in this collection, which is their source of truth; their standalone repos are one-way publish channels for `skills add`. Synced skills are developed in their standalone repos (the source of truth) and tracked here via a git submodule under `vendor/` plus a `SYNC.md` recording the synced git SHA. See AGENTS.md for the full workflow.

## License

MIT
