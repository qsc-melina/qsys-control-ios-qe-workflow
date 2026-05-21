---
name: qe-jira-stories
description: Generate Jira story markdown for a Q-SYS iOS Viewer QE feature. Use when the user wants a ready-to-paste Jira story with acceptance criteria, sub-tasks, and story metadata.
license: MIT
compatibility: Standalone. Works with or without OpenSpec CLI. Best used after qe-test-plan or qe-test-cases.
metadata:
  author: qsys-qe
  version: "1.0"
  domain: Q-SYS iOS Viewer
---

Generate a Jira story document for a Q-SYS iOS Viewer QE effort.

**Output**: A `jira-story.md` file — human-readable, structured for copy-paste into Jira or used as a reference artifact.

---

## Input

Accepts one of (in priority order):
1. A completed `test-plan.md` from the `qe-test-plan` skill
2. A completed `test-cases.md` from the `qe-test-cases` skill
3. A feature description or acceptance criteria provided by the user
4. A Jira epic link or parent story reference

If no input is provided, use the **AskUserQuestion tool** to ask:
> "What feature or testing effort should this Jira story cover? You can describe it, link a test plan, or paste existing acceptance criteria."

---

## Steps

### 1. Read Available Artifacts

Check the current directory for:
- `test-plan.md` → read Objectives, Scenarios, Risks, Environment
- `test-cases.md` → read Summary table (IDs, titles, priorities)

Use whatever is available. If neither exists, proceed from the user-provided description.

### 2. Determine Story Type

Identify whether this story is:
- **Test Planning** — covers planning, environment setup, test case authoring
- **Test Execution** — covers running test cases against a build
- **Defect Investigation** — covers root cause analysis and regression verification
- **QE Sign-Off** — covers final verification and release acceptance

Default to **Test Execution** if unclear.

### 3. Derive Story Metadata

Generate values for each Jira field:

| Field | Guidance |
|---|---|
| **Summary** | `[QE] <Feature Name> — <Story Type>` (e.g., `[QE] Device Discovery — Test Execution`) |
| **Issue Type** | `Story` |
| **Epic Link** | Derived from user input or left as `<Epic Link TBD>` |
| **Component** | `iOS Viewer` |
| **Labels** | Always include `qe`, `manual-testing`, `ios-viewer`. Add `accessibility` if accessibility cases exist. |
| **Fix Version** | From user input or `TBD` |
| **Story Points** | Estimate based on test case count (see table below) |
| **Assignee** | `<Assignee TBD>` |
| **Reporter** | `<Reporter TBD>` |
| **Sprint** | `<Sprint TBD>` |

**Story point estimation table:**

| Test Case Count | Suggested Story Points |
|---|---|
| 1–5 | 1 |
| 6–10 | 2 |
| 11–20 | 3 |
| 21–35 | 5 |
| 36+ | 8 (consider splitting) |

### 4. Write the Description

The Jira **Description** field should contain:

```
## Context

<2–3 sentences describing the feature from a QE perspective. Focus on what is being validated, not how it was built.>

## Testing Scope

<Bullet list: what is in scope. Mirror test plan scope section if available.>

## Out of Scope

<Bullet list: what is explicitly not covered by this story.>

## Test Environment

| Item | Value |
|---|---|
| Platform | iOS Viewer (iPad + iPhone) |
| iOS Version | <version range> |
| Q-SYS Core Version | <version> |
| Network | <topology> |
| Test Device(s) | <device models> |

## References

- Test Plan: `<path or link>`
- Test Cases: `<path or link>`
- Feature Spec: `<path or link>`
- Parent Epic: `<link>`
```

### 5. Write Acceptance Criteria

Use the **Given / When / Then** format. Generate one criterion per major scenario from the test plan or feature description.

```
**AC-01**: Given <precondition>, when <action>, then <expected outcome>.
**AC-02**: Given <precondition>, when <action>, then <expected outcome>.
```

Rules:
- Each AC is independently verifiable
- Use the user's perspective ("the user", "the tester")
- Reference specific Q-SYS iOS Viewer UI elements where known
- Derive ACs from P1 test cases first, then P2
- Include at least one negative/error AC

### 6. Generate Sub-Tasks

Create one sub-task per test case category present in `test-cases.md` (or derived from the feature):

```
Sub-Tasks:
- [ ] ST-01: Author test cases — <Category> (<count> cases, <P1/P2/P3 mix>)
- [ ] ST-02: Execute test cases — <Category> (on <device>)
- [ ] ST-03: Execute test cases — Accessibility (VoiceOver + Dynamic Type)
- [ ] ST-04: Execute regression suite
- [ ] ST-05: Log and triage defects
- [ ] ST-06: QE sign-off and close story
```

Adjust based on story type:
- **Test Planning** stories: sub-tasks focus on authoring, environment setup
- **Test Execution** stories: sub-tasks focus on execution, defect logging
- **QE Sign-Off** stories: sub-tasks focus on final check, release notes review

### 7. Format the Output

Use the template from `templates/jira-story.md`.

The output document includes:
1. YAML-like metadata block at the top (Summary, Type, Epic, Labels, Points)
2. Description section
3. Acceptance Criteria section
4. Sub-Tasks section
5. Notes section (optional team notes, blockers, dependencies)

### 8. Save the Output

Save as:
```
<output-dir>/jira-story.md
```

Where `<output-dir>` is:
- `openspec/changes/<change-name>/` if inside an OpenSpec workflow
- The directory the user specified
- The workspace root if no preference given

---

## Output Summary

After creating the file, show:

```
Jira Story created: <path>

Summary:     [QE] <Feature> — <Story Type>
Story Points: <estimate>
Labels:       <labels>
Sub-Tasks:    <count>
AC Count:     <count>

Tip: Copy the metadata block and paste it into the Jira story creation form.
     Copy the Description section into the Jira Description field.
     Create sub-tasks manually or bulk-import from the ST list.
```

---

## Composability

This skill pairs with:
- **`qe-test-plan`** — read its output to populate context, scope, and environment
- **`qe-test-cases`** — read its summary table to count cases, derive ACs, and generate sub-tasks
- **`openspec-propose`** — run before this skill to generate specs that feed the story description
