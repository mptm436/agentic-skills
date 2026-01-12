# AGENTS.md - Agent Guide for agentic-skills

Personal skill factory for creating AI skills using base skill tools.

## Repository Structure

```
agentic-skills/
├── .claude/skills/           # Base skills (tools for creating skills)
│   ├── skill-creator/        # Skill initialization and packaging
│   ├── skill-from-masters/   # Methodology discovery
│   └── planning-with-files/  # File-based planning patterns
└── skills/                   # Your created skills (output)
```

## Dependencies

**Required**: Python 3.x with PyYAML

Before running any skill-creator scripts, ensure dependencies are installed:

```bash
# Check if PyYAML is available
python -c "import yaml" 2>/dev/null || pip install pyyaml
```

If using UV for virtual environment:
```bash
uv venv && source .venv/bin/activate && uv pip install pyyaml
```

**Agent responsibility**: Always verify the Python environment and install missing dependencies before executing `init_skill.py`, `quick_validate.py`, or `package_skill.py`.

## Skill Creation Workflow

Follow planning-with-files principles for complex skills:

### 1. Plan (for complex skills)
Create `task_plan.md`, `findings.md`, `progress.md` to track progress.

### 2. Discover Methodologies
Run `skill-from-masters` first to find expert frameworks:
- Check methodology database
- Search for additional experts
- Select frameworks to incorporate

### 3. Create Skill
Run `skill-creator` to build the skill:
```bash
python .claude/skills/skill-creator/scripts/init_skill.py <skill-name> --path skills/
```

### 4. Edit and Refine
- Complete TODOs in SKILL.md
- Add scripts/, references/, assets/ as needed
- Delete unused resource directories

### 5. Validate
```bash
python .claude/skills/skill-creator/scripts/quick_validate.py skills/<skill-name>
```

### 6. Package
```bash
python .claude/skills/skill-creator/scripts/package_skill.py skills/<skill-name> [output-dir]
```

## Commands Reference

| Command | Description |
|---------|-------------|
| `init_skill.py <name> --path skills/` | Create new skill from template |
| `quick_validate.py skills/<name>` | Validate skill structure |
| `package_skill.py skills/<name>` | Package into .skill file |

## Naming Conventions

- **Precise and short**: `pdf-rotate` not `pdf-rotation-helper-utility`
- **Hyphen-case**: lowercase letters, digits, hyphens only
- **Max 64 chars**: aim for under 20
- **Directory = name**: folder must match `name` field in frontmatter

Examples:
- ✅ `doc-merge`, `api-client`, `data-viz`
- ❌ `MyDocumentMerger`, `api_client`, `data-visualization-tool-v2`

## SKILL.md Format

### Frontmatter (Required)
```yaml
---
name: skill-name
description: What it does and WHEN to use it. Be specific about triggers.
---
```

| Field | Required | Constraints |
|-------|----------|-------------|
| `name` | Yes | Hyphen-case, max 64 chars |
| `description` | Yes | Max 1024 chars, no angle brackets |
| `license` | No | License reference |
| `allowed-tools` | No | Tool restrictions |
| `metadata` | No | Additional metadata |

### Body
- Use imperative form
- Keep under 500 lines
- Link to references/ for detailed docs

## Adding Methodologies

Edit `.claude/skills/skill-from-masters/references/methodology-database.md`:

### Table Format
```markdown
| Expert | Framework | Core Idea | Source |
|--------|-----------|-----------|--------|
| Name | Method Name | One-line summary | Book/Talk/Essay |
```

### Adding New Domains
1. Create new `## Domain Name` section
2. Add subsections as needed (`### Subdomain`)
3. Include primary sources (books, talks, essays)
4. Prefer experts with proven, documented frameworks

## Code Style

### Python Scripts
```python
#!/usr/bin/env python3
"""Script description."""

import sys
from pathlib import Path
```

- Shebang: `#!/usr/bin/env python3`
- Imports: stdlib first, alphabetical
- Paths: use `pathlib.Path`, call `.resolve()`
- Validation functions: return `(bool, str)` tuple

### Error Messages
- ✅ Success, ❌ Error, 🚀 Starting, 📦 Packaging
- Include actionable fix guidance
- Show usage examples on error

## Planning-with-Files Principles

For complex multi-step skill creation:

### Core Pattern
```
Context Window = RAM (volatile, limited)
Filesystem = Disk (persistent, unlimited)
```

### Files to Create
| File | Purpose |
|------|---------|
| `task_plan.md` | Phases, progress, decisions |
| `findings.md` | Research, discoveries |
| `progress.md` | Session log, test results |

### Rules
1. **2-Action Rule**: Save findings after every 2 operations
2. **Read before decide**: Re-read plan before major decisions
3. **Log all errors**: Prevents repetition
4. **3-Strike Protocol**: Diagnose → Alternative → Rethink → Escalate

## Testing

1. **Validate**: `quick_validate.py` runs automatically before packaging
2. **Script execution**: Ensure scripts run without import errors
3. **Manual testing**: Test skill on sample inputs before distribution