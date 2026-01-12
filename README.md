# agentic-skills

A framework for creating AI skills using AI coding agents. Skills extend Claude's capabilities with specialized knowledge, workflows, and tool integrations.

## Overview

This repository provides base skills that help AI agents (OpenCode, Claude Code, Codex CLI, etc.) create new skills. The agents use these tools to discover methodologies, initialize skill structures, and package distributable skills.

**Best practice**: Use an AI coding agent to build skills under this framework rather than manually executing scripts.

## Repository Structure

```
agentic-skills/
├── .claude/skills/           # Base skills (tools for creating skills)
│   ├── skill-creator/        # Initialization and packaging
│   ├── skill-from-masters/   # Methodology discovery
│   └── planning-with-files/  # File-based planning
└── skills/                   # Created skills (output)
```

## Prerequisites

- Python 3.x
- PyYAML (`pip install pyyaml`)

The AI agent handles environment setup and dependency installation as needed.

## Usage

Open this repository in your AI coding agent and ask it to create a skill:

```
"Create a skill for rotating PDF pages"
"Help me build a skill for writing PRDs using Amazon's 6-pager format"
"I need a skill that helps with code reviews"
```

The agent will:
1. Use `skill-from-masters` to discover expert methodologies
2. Use `skill-creator` to initialize and build skill
3. Follow `planning-with-files` principles for complex tasks
4. Validate and package the final skill

## Base Skills

### [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
Initializes new skills from templates and packages them for distribution. *From Anthropic's [skills](https://github.com/anthropics/skills) repository.*

### [skill-from-masters](https://github.com/GBSOSS/skill-from-masters)
Discovers proven methodologies from domain experts before building a skill. Searches a curated database covering 15+ domains (writing, product, sales, engineering, etc.). *By GBSOSS.*

### [planning-with-files](https://github.com/OthmanAdi/planning-with-files)
File-based planning for complex multi-step tasks. Creates `task_plan.md`, `findings.md`, and `progress.md` to maintain context across long operations. *By Othman Adi.*

## Scripts Reference

These scripts are used by AI agents during skill creation:

### init_skill.py
Initializes a new skill directory with template files.

```bash
python .claude/skills/skill-creator/scripts/init_skill.py <name> --path skills/
```

Creates:
- `SKILL.md` - Main skill file with frontmatter and TODOs
- `scripts/` - Example executable script
- `references/` - Example reference documentation
- `assets/` - Example asset placeholder

### quick_validate.py
Validates skill structure and metadata.

```bash
python .claude/skills/skill-creator/scripts/quick_validate.py skills/<name>
```

| Check | Requirement |
|-------|-------------|
| SKILL.md | Must exist |
| Frontmatter | Valid YAML with `---` delimiters |
| Required fields | `name` and `description` |
| Allowed fields | `name`, `description`, `license`, `allowed-tools`, `metadata` |
| Name format | Hyphen-case, max 64 chars |
| Description | Max 1024 chars, no angle brackets |

### package_skill.py
Creates a distributable `.skill` file (zip format).

```bash
python .claude/skills/skill-creator/scripts/package_skill.py skills/<name> [output-dir]
```

Automatically runs validation first - invalid skills won't package.

## Skill Naming Conventions

- **Hyphen-case**: `pdf-rotate`, `doc-merge`, `api-client`
- **Short and precise**: aim for <20 characters
- **Directory = name**: folder must match `name` field in frontmatter

## Extending the Methodology Database

Add expert frameworks to `.claude/skills/skill-from-masters/references/methodology-database.md`:

```markdown
| Expert | Framework | Core Idea | Source |
|--------|-----------|-----------|--------|
| Name | Method Name | One-line summary | Book/Talk/Essay |
```

## Documentation

- `AGENTS.md` - Detailed guide for AI agents working in this repository

## Acknowledgments

This framework builds on excellent open-source skills from the community:

| Skill | Source | Credit |
|-------|--------|--------|
| skill-creator | [anthropics/skills/skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) | Anthropic |
| planning-with-files | [OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files) | Othman Adi |
| skill-from-masters | [GBSOSS/skill-from-masters](https://github.com/GBSOSS/skill-from-masters) | GBSOSS |

Thank you to these creators for sharing their work with the community.

## License

MIT