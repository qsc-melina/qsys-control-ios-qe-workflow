---
description: Generate a QE Test Plan for a Q-SYS iOS Viewer feature
---

Generate a structured manual QE Test Plan for a Q-SYS iOS Viewer feature.

The output is a single `02-test-plan.md` — human-readable, markdown-based, covering scope, test approach, environment, high-level scenarios, pass/fail criteria, and risks.

---

**Input**: The argument after `/qe:test-plan` is the feature name or description. Can also be a path to a spec file or OpenSpec change name.

**Steps**

Follow the `qe-test-plan` skill instructions exactly. In summary:

1. Read the feature name or description from the user's input (after the slash command)
2. If input is missing or vague, ask: "Which Q-SYS iOS Viewer feature are you writing a test plan for?"
3. Gather feature context (platform, Core version, network topology, edge cases)
4. Determine test scope — in scope and out of scope
5. Write 3–6 test objectives (Verify / Confirm / Validate / Ensure)
6. Define test approach (manual, exploratory, regression, accessibility)
7. Define test environment (iOS versions, device models, Core version, network)
8. List high-level test scenarios by category (Functional, Negative, Edge Cases, Accessibility, Regression)
9. Define pass/fail criteria
10. Identify 2–5 risks with mitigations
11. Write the document using `templates/test-plan.md` as the structure
12. Save as `02-test-plan.md` in the appropriate directory
13. Show completion summary with scenario count and next step

**Next step after this command**: Run `/qe:test-cases` to generate detailed step-by-step test cases from this plan.
