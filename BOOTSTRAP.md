# Bootstrap Checklist for Claude

When asked to bootstrap a new project with SDD workflow, follow these steps:

## Prerequisites

- [ ] Project folder exists
- [ ] Git initialized
- [ ] GitHub repo created and linked
- [ ] `uv` and `specify-cli` installed (for Spec-Kit)

## Bootstrap Steps

### 1. Initialize Spec-Kit

```bash
specify init . --here --ai claude
```

Verify created: `.claude/commands/`, `templates/`, `scripts/`, `memory/`

### 2. Create GitHub Labels

```bash
gh label create "type/feature" -c "0052CC" -d "Feature with spec"
gh label create "type/task" -c "5319E7" -d "Implementation task"
gh label create "type/bug" -c "D73A4A" -d "Bug fix"
gh label create "type/feedback" -c "C5DEF5" -d "Feedback"
gh label create "spec/draft" -c "FEF2C0" -d "Spec in progress"
gh label create "spec/approved" -c "0E8A16" -d "Spec approved"
gh label create "status/wip" -c "FBCA04" -d "Work in progress"
gh label create "status/blocked" -c "D93F0B" -d "Blocked"
```

### 3. Create Issue Templates

Create `.github/ISSUE_TEMPLATE/` with templates from:
`/workspace/personal/gh-sdd-ai-workflow/templates/.github/ISSUE_TEMPLATE/`

```bash
mkdir -p .github/ISSUE_TEMPLATE
cp /workspace/personal/gh-sdd-ai-workflow/templates/.github/ISSUE_TEMPLATE/* .github/ISSUE_TEMPLATE/
```

### 4. Create CLAUDE.md

Create `CLAUDE.md` with project-specific instructions. Include:

```markdown
# [Project Name]

## SDD Workflow

This project uses Spec-Driven Development. Full methodology:
https://github.com/jakubbican/gh-sdd-ai-workflow

### Issue Types

| Type | Label | Purpose |
|------|-------|---------|
| Feature | `type/feature` | New functionality, triggers Spec-Kit |
| Task | `type/task` | Implementation unit from spec |
| Bug | `type/bug` | Bug fix, no spec needed |
| Feedback | `type/feedback` | Feedback on existing feature |

### Feature Workflow

1. **Create Feature issue** on GitHub (label: `type/feature`, `spec/draft`)
2. **Read issue and run Spec-Kit:**
   - `/speckit.specify` with issue content
   - `/speckit.clarify` - answer questions
   - `/speckit.plan` - technical plan
   - `/speckit.tasks` - generate tasks
3. **Create task issues:** `/speckit.taskstoissues`
4. **Change label** to `spec/approved`
5. **Implement tasks**, commit with `Closes #N`

### Daily Work

```bash
gh issue list -l "status/wip"              # What's in progress
gh issue edit N --add-label "status/wip"   # Claim task
# ... work ...
git commit -m "feat: X\n\nCloses #N"       # Complete
```

### Spec-Kit Commands

| Command | Function |
|---------|----------|
| `/speckit.specify` | Create spec from description |
| `/speckit.clarify` | Refine spec (max 5 questions) |
| `/speckit.plan` | Technical implementation plan |
| `/speckit.tasks` | Generate task list |
| `/speckit.taskstoissues` | Create GitHub issues from tasks |

## Project-Specific Instructions

[Add project context, tech stack, conventions here]
```

### 5. Initial Commit

```bash
git add .
git commit -m "chore: initialize project with SDD workflow

- Spec-Kit initialized
- GitHub labels created
- Issue templates added
- CLAUDE.md with workflow"
git push -u origin main
```

## Verification

- [ ] `/speckit.specify` command available
- [ ] Labels visible on GitHub
- [ ] Issue templates work (try creating issue)
- [ ] CLAUDE.md contains full workflow

## Quick Bootstrap Command

For Claude, single command to run after prerequisites are met:

```
Bootstrap this project with SDD workflow from /workspace/personal/gh-sdd-ai-workflow
```
