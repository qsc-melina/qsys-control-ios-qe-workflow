---
description: Analyze a Q-SYS iOS Viewer feature to identify regression areas, impacted systems, smoke coverage, and validation recommendations
---

Generate a Regression Scope document for a Q-SYS iOS Viewer feature.

The output is a `regression-scope.md` — mapping the feature to impacted subsystems with risk levels, a runnable smoke suite, and prioritized validation recommendations.

---

**Input**: The argument after `/qe:regression-scope` is the feature name, or a path to `test-cases.md` / `test-plan.md`.

**Steps**

Follow the `qe-regression-scope` skill instructions exactly. In summary:

1. Read the feature name or artifact paths from user input (after the slash command)
2. Check the current directory for `test-cases.md` and `test-plan.md`; read them if present
3. Extract the feature name, feature code, Regression-category test cases, and P1 Functional test cases
4. Map the feature against all 11 standard Q-SYS iOS Viewer subsystems (`SYS-DISC`, `SYS-CONN`, `SYS-UCI`, `SYS-POPUP`, `SYS-NAV`, `SYS-CTRL`, `SYS-NET`, `SYS-STORE`, `SYS-A11Y`, `SYS-ALERT`, `SYS-ORIENT`)
5. Classify each subsystem as HIGH / MED / LOW risk with a 1–3 sentence rationale
6. Identify integration points: Q-SYS Core protocol, iOS APIs, persistence layer, shared state
7. Build the smoke suite (3–7 checks, each ≤2 min, total ≤10 min); reference existing TC IDs, create `SMOKE-FEAT-NNN` for new checks
8. Write Recommended Validation Areas grouped by P1 / P2 / P3 priority
9. Use `templates/regression-scope.md` as the structure
10. Save as `regression-scope.md` in the same directory as other QE artifacts
11. Show completion summary: impacted system counts, smoke suite size, next step

**Next step after this command**: Run `/qe:jira-stories` — it will incorporate the HIGH-risk validation areas and smoke suite into Jira sub-tasks and acceptance criteria.
