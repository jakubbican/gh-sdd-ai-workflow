# gh-sdd-ai-workflow

Methodology for Spec-Driven Development with AI agents and GitHub Issues.

## About This Repository

This is a **methodology repository**, not a runtime dependency. Contains:
- `README.md` - complete methodology documentation
- `templates/` - issue templates to copy into projects
- `BOOTSTRAP.md` - checklist for setting up new projects

## Working with This Repository

### Editing Methodology

When editing `README.md`:
1. Keep consistency between documentation and files in `templates/`
2. If you change a template, update the corresponding section in documentation

### Structure

```
├── README.md                # Main documentation
├── CLAUDE.md                # This file
├── BOOTSTRAP.md             # Checklist for new projects
└── templates/
    └── .github/ISSUE_TEMPLATE/
        ├── feature.md
        ├── bug.md
        └── feedback.md
```

## Usage for New Project

See `BOOTSTRAP.md` or "Bootstrap New Project" section in `README.md`.

Quick command:
```bash
# In new project
specify init . --here --ai claude
cp -r /path/to/gh-sdd-ai-workflow/templates/.github .
# + create labels, CLAUDE.md
```

## Conventions

- **Documentation language:** English
- **Code and commits:** English
- **User communication:** Czech or English (user's preference)
