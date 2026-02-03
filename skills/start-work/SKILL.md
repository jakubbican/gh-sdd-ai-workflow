---
name: start-work
description: Start working on a GitHub issue
---

Start work on issue #$ARGUMENTS:

1. **Load issue details:**
   ```bash
   gh issue view $ARGUMENTS --json number,title,body,labels
   ```

2. **Determine issue type** from labels and create appropriate branch:

   **Feature** (`type/feature`):
   ```bash
   git checkout -b feature/$ARGUMENTS-{short-slug}
   ```

   **Bug** (`type/bug`):
   ```bash
   git checkout -b fix/$ARGUMENTS-{short-slug}
   ```

   **Task** (`type/task`):
   - Check if already on a feature branch
   - If yes: stay on current branch (tasks are commits within feature branch)
   - If no: warn that Task should be worked on from its parent Feature branch

3. **Mark as work in progress:**
   ```bash
   gh issue edit $ARGUMENTS --add-label "status/wip"
   ```

4. **Add starting comment:**
   ```bash
   gh issue comment $ARGUMENTS -b "Starting work on this issue."
   ```

5. **Display issue summary** for context.

## Branch Naming

| Issue Type | Branch Pattern | Example |
|------------|----------------|---------|
| Feature | `feature/{number}-{slug}` | `feature/42-user-auth` |
| Bug | `fix/{number}-{slug}` | `fix/99-login-crash` |
| Task | (no new branch) | Work on parent feature branch |
