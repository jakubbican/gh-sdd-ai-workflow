---
name: feedback
description: Analyze feedback and route to spec update or standalone fix
---

Analyze feedback issue #$ARGUMENTS:

1. **Read feedback:**
   ```bash
   gh issue view $ARGUMENTS --json title,body,labels
   ```

2. **Identify affected Feature:**
   - From links in issue body
   - Or from context (which spec.md it relates to)

3. **Classify impact:**

   **A) MINOR** (< 1 task of work, doesn't change spec)
   - Create task issue under affected Feature
   - Link: "Addresses feedback #$ARGUMENTS"
   - Implement directly

   **B) SPEC UPDATE** (changes requirements/behavior)
   - Load affected specs/###/spec.md
   - Propose specific changes to spec
   - After user approval:
     - Update spec.md
     - /speckit.plan (if architecture changes)
     - /speckit.tasks (delta - only new/changed)
     - Create/update task issues

   **C) STANDALONE** (unrelated to existing Feature)
   - Propose: create new Feature issue
   - Or: handle as one-off task/bug

4. **Report:**
   - Classification (A/B/C)
   - Proposed actions
   - Wait for confirmation before executing

5. **After completion:**
   - Close feedback issue with link to resolution
   - "Resolved by #task or spec update in commit xyz"
