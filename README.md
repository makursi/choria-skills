# My Agent Skills

A personal collection of [Agent Skills](https://agentskills.io/home).

## Installation

```bash
pnpx skills add makursi/my-agent-skills --skill='*'
```

## Skills

| Skill | Description |
|-------|-------------|
| [notes-refiner](skills/notes-refiner) | Refine scattered Computer Science notes into structured, academically-deep knowledge base documents |
| [document-style](skills/document-style) | Proofread Chinese technical documentation against a writing style guide: audit / revise / update |

## Repository Structure

```
.
├── AGENTS.md               # Skill generation & maintenance workflow
├── README.md
└── skills/
    └── {skill-name}/       # One directory per skill (kebab-case)
        ├── SKILL.md        # Skill entry (frontmatter + overview + triggers)
        └── references/     # On-demand detailed instructions
```

## License

MIT
