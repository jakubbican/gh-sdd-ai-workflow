---
name: feature-spec
description: Run Spec-Kit workflow for a Feature issue
---

For Feature issue #$ARGUMENTS:

1. **Load issue:**
   ```bash
   gh issue view $ARGUMENTS --json title,body,labels,number
   ```

2. **Validate:**
   - Must have label "type/feature"
   - Must have label "spec/draft"

3. **Extract content:**
   - Title as feature name
   - Body sections: Description, User Value, Scope Hints

4. **Run Spec-Kit:**
   ```
   /speckit.specify "[title]: [body summary]"
   ```

5. **Update issue:**
   - Add comment: "Spec created: specs/###-[feature-name]/spec.md"
   - Update checklist item in issue body

6. **Offer next steps:**
   - "Run /speckit.clarify to refine the spec"
   - "Run /speckit.plan when spec is approved"
