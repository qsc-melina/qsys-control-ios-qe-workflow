---
description: Generate detailed manual QE Test Cases for a Q-SYS iOS Viewer feature
---

Generate detailed manual QE Test Cases for a Q-SYS iOS Viewer feature.

The output is a `test-cases.md` — step-by-step, executable on a physical iOS device, organized by category with a summary table.

---

**Input**: The argument after `/qe:test-cases` is the feature name, a path to `test-plan.md`, or a feature description.

**Steps**

Follow the `qe-test-cases` skill instructions exactly. In summary:

1. Read feature name or path from the user's input (after the slash command)
2. If a `test-plan.md` exists in the current directory or was referenced, read it for scope and scenarios
3. If no test plan, derive scope from the feature description
4. Determine test case categories: Functional, Negative, Edge Case, Network, iOS Platform, Accessibility, Regression
5. Assign IDs in format `TC-<FEATURE_CODE>-<NNN>`
6. Write test cases for each category using the format:
   - Priority (P1/P2/P3)
   - Category and Test Type
   - Preconditions as checkboxes
   - Numbered steps (imperative voice, one action per step)
   - Observable Expected Result
   - Blank Actual Result and Status fields
7. Ensure mandatory cases are present (happy path iPad, happy path iPhone, network interruption, app backgrounded, VoiceOver)
8. Build the summary table at the top of the document
9. Use `templates/test-case.md` as the structure
10. Save as `test-cases.md` in the appropriate directory
11. Show completion summary with case counts by priority and next step

**Next step after this command**: Run `/qe:jira-stories` to generate a Jira story from this test plan and cases.
