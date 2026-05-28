---
name: qe-regression-scope
description: Analyze a Q-SYS iOS Viewer feature and its QE artifacts to identify regression areas, impacted systems, high-risk zones, smoke coverage, and recommended validation areas. Use when the user wants a structured regression impact analysis before or during testing.
license: MIT
compatibility: Standalone. Works with or without OpenSpec CLI. Best used after qe-test-cases.
metadata:
  author: qsys-qe
  version: "1.0"
  domain: Q-SYS iOS Viewer
---

Generate a Regression Scope document for a Q-SYS iOS Viewer feature.

**Output**: A single `regression-scope.md` file — human-readable, markdown-based, mapping the feature to impacted subsystems, risk levels, a runnable smoke suite, and prioritized validation recommendations.

---

## Input

Accepts one of (in priority order):
1. A completed `test-cases.md` from the `qe-test-cases` skill
2. A completed `test-plan.md` from the `qe-test-plan` skill
3. A feature description or spec provided by the user

If no input is provided, use the **AskUserQuestion tool** to ask:
> "Which Q-SYS iOS Viewer feature are you scoping for regression? You can paste a feature description or reference a test plan or test cases file."

---

## Steps

### 1. Read Available Artifacts

Check the current working directory for:
- `test-cases.md` → read all test case IDs, titles, priorities, and categories
- `test-plan.md` → read Related features, In Scope, Out of Scope, Test Scenarios

Use whatever is available. If neither exists, proceed from the user-provided description.

From the artifacts, extract:
- The feature name and feature code (e.g., `DD`, `PVL`)
- All Regression-category test cases (already identified by the `qe-test-cases` skill)
- All P1 Functional test cases (these anchor the smoke suite)
- The Out of Scope section (exclude those subsystems from risk analysis)

### 2. Map the Feature to Q-SYS iOS Viewer Subsystems

Every regression scope document analyzes the feature against these standard Q-SYS iOS Viewer subsystems:

| Subsystem ID | Name | Description |
|---|---|---|
| `SYS-DISC` | Device Discovery | mDNS scanning, manual IP entry, saved device list |
| `SYS-CONN` | Connection Manager | Connect/disconnect lifecycle, authentication, timeout handling |
| `SYS-UCI` | UCI Rendering Engine | Rendering Q-SYS UCI design pages, controls, and layout |
| `SYS-POPUP` | Popup & Layer Manager | Popup open/close, fade transitions, shared layer visibility state |
| `SYS-NAV` | Navigation Stack | Page navigation within a connected design |
| `SYS-CTRL` | Control Interaction | Sliders, buttons, text inputs, toggles, knobs in the UCI |
| `SYS-NET` | Network Layer | TCP/IP communication, mDNS resolution, network state monitoring |
| `SYS-STORE` | Persistence & Storage | Saved devices, user preferences, cached design state |
| `SYS-A11Y` | Accessibility | VoiceOver focus management, Dynamic Type, minimum tap targets |
| `SYS-ALERT` | Notifications & Alerts | Error dialogs, connection status banners, permission prompts |
| `SYS-ORIENT` | Orientation & Layout | Portrait/landscape adaptation, safe area handling |

For each subsystem, determine:
- **Direct** — the subsystem IS the feature under test (always HIGH risk)
- **Shared state** — the subsystem reads or writes data the feature changes (HIGH risk)
- **Shared surface** — the subsystem renders UI that co-exists with the feature (MED risk)
- **Shared lifecycle** — the subsystem is affected by the same iOS lifecycle events (MED risk)
- **Isolated** — the subsystem has no known dependency on the changed feature (LOW risk)

### 3. Classify Risk Levels

Assign each subsystem a risk level:

| Risk | Definition |
|---|---|
| **HIGH** | Subsystem shares state, data, or logic with the feature. Regression failures here are **likely** if the feature code changes. Must be tested before sign-off. |
| **MED** | Subsystem shares UI surfaces, lifecycle events, or platform permissions. Regression is **possible** but not certain. Should be tested as part of the standard regression pass. |
| **LOW** | Subsystem is architecturally isolated from the feature. Regression is **unlikely**. A single spot-check or existing automated coverage is sufficient. |

For each HIGH and MED subsystem, write 1–3 sentences explaining **why** it is at risk. Be specific:
- What shared data or state creates the risk?
- What code path or UI surface is shared?
- What symptom would a regression produce?

### 4. Identify Integration Points

List all integration boundaries where the feature touches external systems:

- **Q-SYS Core protocol** — any Q-SYS Core commands, events, or properties the feature sends or receives
- **iOS APIs** — any iOS SDK APIs the feature invokes (Network framework, UIKit, SwiftUI, accessibility APIs)
- **Persistence layer** — any data stored to disk, UserDefaults, Keychain, or Core Data
- **Shared state objects** — any app-level singletons, managers, or observable state the feature reads or writes

Integration points are high-value regression targets because regressions at boundaries are harder to catch than unit-level failures.

### 5. Define the Smoke Suite

Write 3–7 smoke tests — the fastest possible checks that confirm nothing is catastrophically broken.

Rules for smoke tests:
- Each should be executable in under 2 minutes on a single device
- Together the full suite should complete in under 10 minutes
- Cover at minimum: one connectivity check, one rendering/navigation check, one feature-under-test check
- Reference existing TC IDs (from `test-cases.md`) wherever possible — do NOT duplicate already-written test cases
- If no TC exists for a needed smoke check, write a new minimal check using the format:
  `SMOKE-<FEATURE_CODE>-<NNN>: <Title> — <one-line description of what to do and what to observe>`

Smoke suite format:

| ID | Title | Subsystem | TC Reference | Est. Time |
|----|-------|-----------|--------------|-----------|
| SMOKE-FEAT-001 | ... | SYS-xxx | TC-FEAT-NNN or NEW | < 2 min |

### 6. Write Recommended Validation Areas

Generate a prioritized list of recommended validation areas — these are the areas a tester should focus on when verifying that the feature did not cause regressions.

Group by priority:

**P1 — Must validate before any sign-off:**
- List 2–5 specific behaviors or test cases that are absolute blockers for regression sign-off

**P2 — Validate as part of standard regression pass:**
- List 3–8 behaviors or subsystem areas to sweep during the sprint's regression pass

**P3 — Spot-check or defer to next cycle:**
- List any LOW-risk areas worth a quick glance but not requiring full coverage

For each recommendation, note:
- Which subsystem it covers (`SYS-xxx`)
- Which existing test case ID covers it (if one exists)
- Whether it requires a physical device or can be done on simulator

### 7. Write the Document

Use the template from `templates/regression-scope.md`.

Fill in all sections. Write in the present tense from the perspective of a QE engineer preparing for a test cycle. Be specific about what could go wrong, not just what exists.

Mark any section as `TBD` if the information is not available at document creation time.

### 8. Save the Output

Save as:
```
<output-dir>/04-regression-scope.md
```

Where `<output-dir>` is:
- `openspec/changes/<change-name>/` if inside an OpenSpec workflow
- The directory the user specified
- The same directory as `test-cases.md` / `test-plan.md` if they exist

---

## Output Summary

After creating the file, show:

```
Regression Scope created: <path>

Feature:          <feature name>
Impacted Systems: <count> total — <count> HIGH, <count> MED, <count> LOW
Smoke Suite:      <count> checks (~<total time> min)
Integration Points: <count>

Next step: Run /qe:jira-stories to generate a Jira QE story that incorporates
           these regression sub-tasks and validation recommendations.
```

---

## Composability

This skill pairs with:
- **`qe-test-cases`** — read its output to seed impacted systems and smoke suite TC references
- **`qe-test-plan`** — read its Related Features and Out of Scope sections to bound the analysis
- **`qe-jira-stories`** — pass this file as input; the skill will incorporate regression sub-tasks and HIGH-risk validation into the Jira story's acceptance criteria and sub-tasks
- **`openspec-propose`** — run before this skill when the feature is being designed; regression scope informs proposal risks

### Where this fits in the pipeline

```
/qe:test-plan          →  test-plan.md
/qe:test-cases         →  test-cases.md
/qe:regression-scope   →  regression-scope.md   ← this skill
/qe:jira-stories       →  jira-story.md
```
