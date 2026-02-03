---
name: done
description: Complete work on an issue
---

Complete issue #$ARGUMENTS:

1. **Load issue details** to determine type:
   ```bash
   gh issue view $ARGUMENTS --json number,title,labels
   ```

2. **Verify all acceptance criteria met** by reviewing the issue requirements.

3. **Stage relevant files:**
   ```bash
   git add [relevant files]
   ```

4. **Handle based on issue type:**

   ---

   **Task** (`type/task`):

   Commit with reference (issue closes when PR merges):
   ```bash
   git commit -m "feat: [description]

   Implements #$ARGUMENTS"
   ```

   Remove WIP label:
   ```bash
   gh issue edit $ARGUMENTS --remove-label "status/wip"
   ```

   Add comment:
   ```bash
   gh issue comment $ARGUMENTS -b "Implementation complete. Will close when Feature PR merges."
   ```

   **Note:** Stay on feature branch, continue with next Task.

   ---

   **Feature** (`type/feature`):

   Create PR that closes Feature and all its Tasks:
   ```bash
   gh pr create --title "[Feature title]" --body "## Summary
   [Description]

   Closes #$ARGUMENTS

   ## Tasks
   - Closes #[task1]
   - Closes #[task2]
   - Closes #[task3]"
   ```

   Remove WIP label:
   ```bash
   gh issue edit $ARGUMENTS --remove-label "status/wip"
   ```

   ---

   **Bug** (`type/bug`):

   Commit and create PR:
   ```bash
   git commit -m "fix: [description]

   Fixes #$ARGUMENTS"

   gh pr create --title "Fix: [bug title]" --body "Fixes #$ARGUMENTS

   ## What was wrong
   [description]

   ## How it was fixed
   [description]"
   ```

   Remove WIP label:
   ```bash
   gh issue edit $ARGUMENTS --remove-label "status/wip"
   ```

## Summary

| Issue Type | Commit Keyword | Creates PR | Closes Issue |
|------------|----------------|------------|--------------|
| Task | `Implements #N` | No | Via Feature PR |
| Feature | `Closes #N` | Yes | Via PR merge |
| Bug | `Fixes #N` | Yes | Via PR merge |
