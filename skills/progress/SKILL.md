---
name: progress
description: Add progress comment to issue
---

Add progress update to issue #$ARGUMENTS:

1. **Summarize completed work** from recent commits and changes.

2. **List remaining items** based on issue requirements.

3. **Note any blockers** or discovered issues.

4. **Post progress comment:**
   ```bash
   gh issue comment $ARGUMENTS -b "## Progress: $(date +%Y-%m-%d)

   ### Done
   - [x] [Completed items]

   ### Next
   - [ ] [Remaining items]

   ### Notes
   - [Any blockers or discoveries]"
   ```
