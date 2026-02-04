---
name: done
description: Complete work on an issue (Feature or Bug)
---

Complete issue #$ARGUMENTS:

1. **Load issue details** to determine type:
   ```bash
   gh issue view $ARGUMENTS --json number,title,labels
   ```

2. **Verify all work is complete:**
   - For Feature: all phases implemented, tests pass
   - For Bug: fix verified, tests pass

3. **Ensure all changes committed and pushed:**
   ```bash
   git status
   git push
   ```

4. **Handle based on issue type:**

   ---

   **Feature** (`type/feature`):

   Create PR that closes the Feature:
   ```bash
   gh pr create --title "feat: [Feature title]" --body "## Summary
   [Description of what was implemented]

   ## Phases Completed
   - Phase 1: Setup
   - Phase 2: Foundation
   - Phase 3: Core functionality
   - ...

   ## Test Plan
   - [x] All tests pass
   - [x] Manual verification done

   Closes #$ARGUMENTS"
   ```

   Remove WIP label:
   ```bash
   gh issue edit $ARGUMENTS --remove-label "status/wip"
   ```

   ---

   **Bug** (`type/bug`):

   Create PR that fixes the Bug:
   ```bash
   gh pr create --title "fix: [Bug description]" --body "## Problem
   [What was wrong]

   ## Solution
   [How it was fixed]

   ## Test Plan
   - [x] Bug no longer reproducible
   - [x] Tests added/updated

   Fixes #$ARGUMENTS"
   ```

   Remove WIP label:
   ```bash
   gh issue edit $ARGUMENTS --remove-label "status/wip"
   ```

## Summary

| Issue Type | PR Keyword | Closes Issue |
|------------|------------|--------------|
| Feature | `Closes #N` | Via PR merge |
| Bug | `Fixes #N` | Via PR merge |

## After PR Merge

The issue will be automatically closed when PR is merged.
