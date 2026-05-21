---
description: Generate a Jira story for a Q-SYS iOS Viewer QE effort
---

Generate a Jira story document for a Q-SYS iOS Viewer QE effort.

The output is a `jira-story.md` — structured with Jira metadata, description, acceptance criteria, and sub-tasks. Ready to copy-paste into Jira.

---

**Input**: The argument after `/qe:jira-stories` is the feature name, a path to `test-plan.md`, `test-cases.md`, or a Jira epic link.

**Steps**

Follow the `qe-jira-stories` skill instructions exactly. In summary:

1. Read the feature name or artifact paths from the user's input (after the slash command)
2. Check for `test-plan.md` and `test-cases.md` in the current directory
3. If artifacts exist, read them to populate context, scope, environment, and case counts
4. Determine story type: Test Planning | Test Execution | Defect Investigation | QE Sign-Off
5. Derive story metadata:
   - Summary: `[QE] <Feature> — <Story Type>`
   - Labels: `qe`, `manual-testing`, `ios-viewer` (+ `accessibility` if applicable)
   - Story points estimated from test case count
6. Write Description with: Context, Testing Scope, Out of Scope, Test Environment, References
7. Write Acceptance Criteria using Given/When/Then format (minimum 4 ACs including 1 negative and 1 accessibility)
8. Generate Sub-Tasks (ST-01 through ST-08) based on story type
9. Write Definition of Done checklist
10. Use `templates/jira-story.md` as the structure
11. Save as `jira-story.md` in the appropriate directory
12. Show completion summary with point estimate and tips for Jira import

**Tip**: Copy the Metadata block into the Jira story creation form. Copy the Description section into the Jira Description field.
