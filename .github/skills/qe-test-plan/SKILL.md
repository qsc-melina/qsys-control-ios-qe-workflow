---
name: qe-test-plan
description: Generate a structured QE Test Plan for a Q-SYS iOS Viewer feature. Use when the user wants to plan manual testing coverage for a new or changed feature.
license: MIT
compatibility: Standalone. Works with or without OpenSpec CLI.
metadata:
  author: qsys-qe
  version: "1.0"
  domain: Q-SYS iOS Viewer
---

Generate a manual QE Test Plan document for a Q-SYS iOS Viewer feature.

**Output**: A single `test-plan.md` file — human-readable, markdown-based, focused on manual iOS testing.

---

## Input

The user provides one of:
- A feature name or short description
- An existing spec file or OpenSpec change artifact
- A Jira story or ticket summary

If input is missing or vague, use the **AskUserQuestion tool** to ask:
> "Which Q-SYS iOS Viewer feature are you writing a test plan for? Briefly describe what it does."

---

## Steps

### 1. Gather Feature Context

Collect the following from user input, specs, or conversation context:

| Field | Description |
|---|---|
| Feature name | Short name (e.g., "Device Discovery") |
| Feature summary | What the feature does from a user perspective |
| Platform | iOS Viewer (iPad / iPhone) |
| Q-SYS Core version | Minimum supported Core version, if known |
| Network context | LAN, WAN, mDNS, manual IP, etc. |
| Related features | Anything that interacts with this feature |
| Known edge cases | Boundary conditions the team has flagged |

If any critical field is unknown, note it as `TBD` in the output document.

### 2. Determine Test Scope

Identify what IS and IS NOT in scope:

**In scope (always consider):**
- Happy path user flows
- iOS permissions (network, notifications, Bluetooth if relevant)
- Device states: foreground, background, killed
- Network interruption and reconnection
- iOS version compatibility (minimum: iOS 16)
- iPad and iPhone form factors
- Orientation: portrait and landscape
- Dark mode and light mode
- Accessibility (VoiceOver, Dynamic Type)

**Out of scope (common exclusions):**
- Q-SYS Designer / Q-SYS Core internal behavior (unless integration point)
- Android / web platform equivalents
- Performance benchmarking (unless feature is performance-critical)

### 3. Define Test Objectives

Write 3–6 objective statements as bullet points:
- Each starts with an action verb: "Verify", "Confirm", "Validate", "Ensure"
- Each maps to a user-facing behavior
- Avoid implementation details

### 4. Describe Test Approach

State the testing methodology:
- **Manual testing** on physical iOS device(s)
- Exploratory testing for edge cases and UX issues
- Regression testing against previously passing scenarios
- Reference to shared template in `templates/test-plan.md`

### 5. Define Test Environment

Specify:
- iOS device models (minimum 1 iPad, 1 iPhone)
- iOS version (latest + N-1)
- Q-SYS Core version and hardware model
- Network topology (same subnet vs. cross-subnet, VPN, etc.)
- Any required Q-SYS design file or asset

### 6. List High-Level Test Scenarios

Group scenarios into categories. Each scenario is a single sentence describing the user action and expected outcome. Categories typically include:

- **Functional** — core feature behavior
- **Negative** — invalid inputs, error conditions
- **Edge Cases** — boundaries, timeouts, state transitions
- **Accessibility** — VoiceOver, Dynamic Type, contrast
- **Regression** — existing behaviors that must not break

Do NOT write detailed steps here — that is the job of `qe-test-cases` skill.

### 7. Define Pass / Fail Criteria

Write clear, binary criteria:
- **Pass**: All functional scenarios pass; no P1/P2 defects open at release
- **Fail**: Any functional scenario fails; any crash or data loss defect open

### 8. Identify Risks and Mitigations

List 2–5 risks as a table:

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| example | Low/Med/High | Low/Med/High | mitigation action |

### 9. Write the Document

Use the template from `templates/test-plan.md`.

Fill in all sections. Use:
- `TBD` for unknown values
- Inline code for Q-SYS Core commands, API values, or file names
- Tables for structured data (environment, risks)
- Numbered lists for ordered content
- Bullet lists for unordered content

Write in the third person from the tester's perspective.

### 10. Output Location

Save as:
```
<output-dir>/test-plan.md
```

Where `<output-dir>` is:
- `openspec/changes/<change-name>/` if inside an OpenSpec workflow
- The directory the user specified
- The workspace root if no preference given

---

## Output Summary

After creating the file, show:
```
Test Plan created: <path>

Scope:    <feature name>
Scenarios: <count> high-level scenarios across <count> categories
Next step: Run /qe:test-cases to generate detailed test cases from this plan.
```

---

## Composability

This skill pairs with:
- **`qe-test-cases`** — consumes this plan to generate step-by-step test cases
- **`qe-jira-stories`** — consumes this plan to generate a Jira QE story
- **`openspec-propose`** — run before this skill to generate specs from which this plan is derived
