---
name: start-work
description: Start working on a GitHub issue
---

Start work on issue #$ARGUMENTS:

1. **Load issue details:**
   ```bash
   gh issue view $ARGUMENTS --json title,body,labels
   ```

2. **Mark as work in progress:**
   ```bash
   gh issue edit $ARGUMENTS --add-label "status/wip"
   ```

3. **Create feature branch:**
   ```bash
   git checkout -b issue-$ARGUMENTS
   ```

4. **Add starting comment:**
   ```bash
   gh issue comment $ARGUMENTS -b "Starting work on this issue."
   ```

5. **Display issue summary** for context.
