# gh-sdd-ai-workflow

**Centralized methodology for Spec-Driven Development with AI agents and GitHub Issues.**

## Repository Structure

```
├── README.md                    ← Methodology documentation (this file)
├── CLAUDE.md                    ← Instructions for this repo
├── BOOTSTRAP.md                 ← Checklist for new projects
├── skills/                      ← Claude Code skills
│   ├── start-work/SKILL.md      ← Start work on issue
│   ├── progress/SKILL.md        ← Add progress comment
│   ├── done/SKILL.md            ← Complete issue
│   ├── feedback/SKILL.md        ← Process feedback
│   └── feature-spec/SKILL.md    ← Bridge Feature→Spec-Kit
└── templates/
    └── .github/ISSUE_TEMPLATE/
        ├── feature.md           ← Feature with spec
        ├── bug.md               ← Bug report
        └── feedback.md          ← Feedback on feature
```

## About This Repository

This is a **methodology repository**, not a runtime dependency. Individual projects:
- Initialize Spec-Kit locally (`specify init`)
- Copy issue templates from `templates/`
- Reference this methodology in their CLAUDE.md
- Remain fully functional standalone

**Key principle:** Projects are self-contained. This repo provides templates, documentation, and optional shared skills - but each project works independently.

> **Language note:** Documentation is in English. Users can communicate with Claude in Czech or English.

---

# Methodology Documentation

## Goal

Replace MD-based workflow (PLAN.md + DEVLOG.md) with GitHub Issues integrated with [GitHub Spec-Kit](https://github.com/github/spec-kit) for spec-driven development.

---

## Issue Structure

Simplified two-level hierarchy + supporting types:

```mermaid
flowchart TB
    subgraph features[" "]
        F[**FEATURE**<br/>type/feature]
        F --> T1[Task]
        F --> T2[Task]
        F --> T3[Task]
    end

    subgraph standalone[" "]
        B[**BUG**<br/>type/bug]
        FB[**FEEDBACK**<br/>type/feedback]
    end

    FB -.->|minor| T4[New Task]
    FB -.->|major| F2[Spec Update]

    style F fill:#0052CC,color:#fff
    style T1 fill:#5319E7,color:#fff
    style T2 fill:#5319E7,color:#fff
    style T3 fill:#5319E7,color:#fff
    style T4 fill:#5319E7,color:#fff
    style B fill:#D73A4A,color:#fff
    style FB fill:#C5DEF5,color:#000
    style F2 fill:#0052CC,color:#fff
```

| Type | Label | Has Spec | Generates Tasks |
|------|-------|----------|-----------------|
| Feature | `type/feature` | Yes (`specs/###-name/`) | Yes |
| Task | `type/task` | No (part of Feature) | No |
| Bug | `type/bug` | No | No |
| Feedback | `type/feedback` | No | Routes to Task or Spec update |

**Why simplified:**
- Epic/Story levels replaced by spec files (Spec-Kit workflow)
- User Stories are part of `spec.md`, not separate issues
- Tasks are tagged `[US1]`, `[US2]` for traceability

---

## Labels

```bash
# Issue types
gh label create "type/feature" -c "0052CC" -d "Feature with spec (triggers Spec-Kit)"
gh label create "type/task" -c "5319E7" -d "Implementation task (generated from spec)"
gh label create "type/bug" -c "D73A4A" -d "Bug fix (no spec needed)"
gh label create "type/feedback" -c "C5DEF5" -d "Feedback to process"

# Spec workflow
gh label create "spec/draft" -c "FEF2C0" -d "Spec in progress"
gh label create "spec/approved" -c "0E8A16" -d "Spec approved, ready for tasks"

# Status
gh label create "status/wip" -c "FBCA04" -d "Work in progress"
gh label create "status/blocked" -c "D93F0B" -d "Blocked"

# Priority (optional)
gh label create "priority/high" -c "B60205" -d "High priority"
```

---

## Milestones

Create **ad-hoc** when it makes sense (release, important milestone, MVP done).

```bash
# Example - only when needed
gh api repos/{owner}/{repo}/milestones -f title="v0.1-mvp" -f description="Minimum viable product"
```

---

## Spec-Driven Development (SDD)

### Why SDD

- AI agents need **unambiguous instructions** for autonomous work
- Spec as "shared source of truth" between you and AI
- You invest time in spec → AI invests time in implementation
- Measurable criteria = validatable results

### Tool: GitHub Spec-Kit

[Spec-Kit](https://github.com/github/spec-kit) is the official GitHub toolkit for SDD. It supports Claude Code and other AI agents.

**Installation:**
```bash
# Requires uv (Python package manager)
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# Initialize for Claude Code
specify init . --here --ai claude
```

**Commands:**
| Command | Function |
|---------|----------|
| `/speckit.specify` | Creates spec from feature description |
| `/speckit.clarify` | Detects ambiguity, asks questions (max 5) |
| `/speckit.plan` | Technical plan from spec |
| `/speckit.tasks` | Generates tasks.md |
| `/speckit.taskstoissues` | Creates GitHub Issues from tasks |
| `/speckit.implement` | Autonomous implementation |

### Connecting Spec-Kit with Feature Issues

Spec-Kit commands work with local files (`specs/###-feature-name/`) and don't automatically read GitHub Issues. **Manual bridging is required.**

**Recommended workflow:**

```
1. Create Feature issue manually (high-level description)

2. Read the issue and pass content to Spec-Kit:
   "Read Feature issue #123 and run /speckit.specify with its content"

   Claude will:
   - gh issue view 123 --json title,body
   - Extract description
   - Run /speckit.specify "[extracted content]"

3. After spec is created, update the Feature issue:
   - Add comment with link to spec.md
   - Update checklist in issue body

4. Continue with /speckit.clarify, /speckit.plan, etc.
```

**Automation:** Use `/feature-spec` skill from `skills/feature-spec/` to automate this bridging.

### Workflow: Feature → Spec → Tasks → Issues

```mermaid
flowchart TD
    A[**Feature Issue**<br/>type/feature, spec/draft] --> B[/speckit.specify/]
    B --> C[spec.md<br/>User Stories, Requirements]
    C --> D[/speckit.clarify/]
    D -->|Questions?| D
    D -->|Approved| E[/speckit.plan/]
    E --> F[plan.md, data-model.md]
    F --> G[/speckit.tasks/]
    G --> H[tasks.md]
    H --> I[/speckit.taskstoissues/]
    I --> J[**Task Issues**<br/>type/task]
    J --> K[Implementation]
    K -->|Closes #N| L[Done]

    style A fill:#0052CC,color:#fff
    style J fill:#5319E7,color:#fff
    style L fill:#0E8A16,color:#fff
```

**Steps:**
1. **Feature Issue** - Create with high-level description
2. **/speckit.specify** - Creates `specs/###-name/spec.md`
3. **/speckit.clarify** - AI asks questions, refine until approved
4. **/speckit.plan** - Creates `plan.md`, `data-model.md`, `contracts/`
5. **/speckit.tasks** - Generates `tasks.md` with `[US1]`, `[US2]`, `[P]` tags
6. **/speckit.taskstoissues** - Creates Task issues as sub-issues
7. **Implement** - Work on tasks, commit with `Closes #N`

---

## Issue Templates

Issue templates are in `templates/.github/ISSUE_TEMPLATE/`:

| Template | Purpose |
|----------|---------|
| `feature.md` | New feature with spec workflow checklist |
| `bug.md` | Bug report (observed/expected/repro) |
| `feedback.md` | Feedback on existing feature |

Copy to your project:
```bash
cp -r templates/.github .
```

---

## Processing Feedback Issues

Feedback can range from minor tweaks to spec changes. Use `/feedback` skill to classify:

| Classification | Action |
|----------------|--------|
| **A) MINOR** | Create task, implement directly |
| **B) SPEC UPDATE** | Update spec.md, regenerate tasks |
| **C) STANDALONE** | Create new Feature or handle as bug |

See `skills/feedback/SKILL.md` for full workflow.

---

## Iterative Workflow

### Daily Work

```bash
# 1. What's in progress?
gh issue list -l "status/wip"

# 2. Pick a task
gh issue edit <num> --add-label "status/wip"

# 3. Work (70% context blocks)

# 4. Commit with reference
git commit -m "feat: implement X

Closes #123"

# 5. Progress comment (for longer tasks)
gh issue comment <num> -b "Progress: done X, next Y"
```

### Closing Keywords

In commit message, automatically close issues:
- `Closes #123`, `Fixes #123`, `Resolves #123`

### Progress Comment Template

```markdown
## Progress: 2026-01-27

### Done
- [x] Implemented X
- [x] Added tests for Y

### Next
- [ ] Still need Z

### Notes
- Discovered issue with W, created #456
```

---

## Skills

Skills are in `skills/` folder. Copy to your project's `.claude/skills/` or use globally via `~/.claude/skills/`.

| Skill | Purpose |
|-------|---------|
| `/start-work` | Start working on issue (branch, WIP label, comment) |
| `/progress` | Add progress comment to issue |
| `/done` | Complete issue (commit, remove WIP, comment) |
| `/feedback` | Analyze and route feedback |
| `/feature-spec` | Bridge Feature issue → Spec-Kit |

---

## Project Organization

```
┌─────────────────────────────────────────────────────────────────┐
│  gh-sdd-ai-workflow (this repo)                                 │
│  ─────────────────────────────                                  │
│  • Methodology documentation                                    │
│  • Custom skills (beyond Spec-Kit)                             │
│  • Issue templates (source of truth)                           │
│  • Bootstrap instructions                                       │
│  • NOT a runtime dependency                                    │
└─────────────────────────────────────────────────────────────────┘
                              ↓
              Bootstrap new project (one-time)
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  Individual Project                                             │
│  ──────────────────                                             │
│  • Spec-Kit initialized locally (specify init)                 │
│  • Issue templates copied from methodology                     │
│  • CLAUDE.md references methodology doc                        │
│  • Fully self-contained and standalone                         │
│  • Works for anyone who clones just this repo                  │
└─────────────────────────────────────────────────────────────────┘
```

**Why this approach:**
1. **Projects are portable** - Anyone can clone a single project and it works
2. **Methodology evolves** - Update here, projects can pull changes
3. **Spec-Kit stays local** - Each project has its own `/speckit.*` commands
4. **Skills can be shared** - Via `~/.claude/skills/` or symlinks (optional)

---

## Bootstrap New Project

See **[BOOTSTRAP.md](BOOTSTRAP.md)** for complete checklist.

Quick steps:
1. Create project folder and GitHub repo
2. `specify init . --here --ai claude`
3. Create labels (see Labels section)
4. `cp -r /path/to/gh-sdd-ai-workflow/templates/.github .`
5. Create CLAUDE.md with workflow reference
6. Commit and push

---

## gh CLI Quick Reference

```bash
# Issues
gh issue list                           # List
gh issue list -l "type/feature"         # Filter
gh issue view 123                       # Detail
gh issue create -t "Title" -b "Body" -l "type/feature"
gh issue edit 123 --add-label "x"
gh issue comment 123 -b "Text"
gh issue close 123

# Labels
gh label list
gh label create "name" -c "color" -d "description"
```

---

## Resources

- [GitHub Spec-Kit](https://github.com/github/spec-kit) - Official SDD toolkit
- [Spec-driven development blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [GitHub Sub-issues](https://github.blog/engineering/architecture-optimization/introducing-sub-issues-enhancing-issue-management-on-github/)
- [Claude Code Skills Docs](https://code.claude.com/docs/en/skills)
- [gh CLI Manual](https://cli.github.com/manual/)
