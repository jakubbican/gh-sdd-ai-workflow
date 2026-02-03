---
name: done
description: Complete work on an issue
---

Complete issue #$ARGUMENTS:

1. **Verify all acceptance criteria met** by reviewing the issue requirements.

2. **Stage relevant files:**
   ```bash
   git add [relevant files]
   ```

3. **Create commit with closing reference:**
   ```bash
   git commit -m "feat/fix: [description]

   Closes #$ARGUMENTS"
   ```

4. **Remove WIP label:**
   ```bash
   gh issue edit $ARGUMENTS --remove-label "status/wip"
   ```

5. **Add completion comment:**
   ```bash
   gh issue comment $ARGUMENTS -b "Completed. See commit for details."
   ```
