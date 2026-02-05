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

> **TODO:** Migrate to native GitHub Issue Types when `gh` CLI fully supports them.

```bash
gh label create "type/feature" -c "0052CC" -d "Feature with spec"
gh label create "type/task" -c "5319E7" -d "Implementation task (optional)"
gh label create "type/bug" -c "D73A4A" -d "Bug fix"
gh label create "type/feedback" -c "C5DEF5" -d "Feedback"
gh label create "spec/draft" -c "FEF2C0" -d "Spec in progress"
gh label create "spec/approved" -c "0E8A16" -d "Spec approved"
gh label create "status/wip" -c "FBCA04" -d "Work in progress"
gh label create "status/blocked" -c "D93F0B" -d "Blocked"
```

### 3. Create Issue Templates

```bash
mkdir -p .github/ISSUE_TEMPLATE
cp /workspace/personal/gh-sdd-ai-workflow/templates/.github/ISSUE_TEMPLATE/* .github/ISSUE_TEMPLATE/
```

### 4. Create CLAUDE.md

Create `CLAUDE.md` with project-specific instructions:

```markdown
# [Project Name]

## SDD Workflow

This project uses Spec-Driven Development. Full methodology:
https://github.com/jakubbican/gh-sdd-ai-workflow

### Issue Types

| Type | Label | Purpose |
|------|-------|---------|
| Feature | `type/feature` | Main work unit, tracked in GitHub |
| Task | `type/task` | Optional - tasks live in `tasks.md` |
| Bug | `type/bug` | Bug fix, no spec needed |
| Feedback | `type/feedback` | Routes to spec update or bug |

### Mandatory Label Transitions

| Trigger | Command |
|---------|---------|
| After `/speckit.tasks` | `gh issue edit N --remove-label "spec/draft" --add-label "spec/approved"` |
| Start implementation | `gh issue edit N --add-label "status/wip"` |
| Before PR | `gh issue edit N --remove-label "status/wip"` |

### Feature Workflow

1. **Create Feature issue** (label: `type/feature`, `spec/draft`)
2. **Create branch** `feature/{N}-{slug}` and **link to issue**
3. **Run Spec-Kit phases** - update issue after each:
   - `/speckit.specify` → update issue
   - `/speckit.clarify` → update issue
   - `/speckit.plan` → update issue
   - `/speckit.tasks` → update issue + **change label to `spec/approved`**
4. **Start implementation** → **add `status/wip` label**
5. **Implement per phase** - commit + push + update issue after each
6. **Before PR** → **remove `status/wip` label**
7. **Create PR** with `Closes #N`

### Recommended Prompts

#### Spec Phases
```
Read Feature issue #N and run /speckit.specify with its content.
After completion, update the issue with link to spec.md and status.
```

#### Implementation (per phase)
```
/speckit.implement next incomplete phase from Feature #N.
After completion: commit all changes, push, update Feature issue with progress.
```

### Feature Issue Updates (REQUIRED)

After EVERY Spec-Kit command or implementation phase, update the Feature issue with:
- Link to created documents (in branch)
- Current workflow status
- Next step

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
- [ ] CLAUDE.md contains workflow with mandatory label transitions

## Quick Bootstrap Command

```
Bootstrap this project with SDD workflow from /workspace/personal/gh-sdd-ai-workflow
```
