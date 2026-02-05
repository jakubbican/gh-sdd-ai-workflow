---
name: Feature
about: New feature with spec
title: '[FEATURE] '
labels: type/feature, spec/draft
assignees: ''
---

## Description

<!-- High-level description of what we want to achieve -->

## User Value

<!-- Why are we doing this? Who benefits? -->

## Scope Hints

<!-- Optional: what should/shouldn't be included -->

---

**Workflow:**

### Setup
- [ ] Create branch via `/speckit.specify` (creates `###-feature-name`)
- [ ] Add branch link comment to this issue

### Spec Phases (update issue after each!)
- [ ] `/speckit.specify` → comment with spec.md link
- [ ] `/speckit.clarify` → comment with status
- [ ] `/speckit.plan` → comment with plan.md link
- [ ] `/speckit.tasks` → comment with phases overview
- [ ] **Label:** `gh issue edit N --remove-label "spec/draft" --add-label "spec/approved"`

### Implementation
- [ ] **Label:** `gh issue edit N --add-label "status/wip"`
- [ ] Phase 1: Setup → commit, push, comment
- [ ] Phase 2: Foundation → commit, push, comment
- [ ] Phase N: ... → commit, push, comment

### Completion
- [ ] **Label:** `gh issue edit N --remove-label "status/wip"`
- [ ] Create PR with `Closes #N`
- [ ] Link to spec: `specs/###-name/`
