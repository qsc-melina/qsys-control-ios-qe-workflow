# QE Conventions and Standards

This is the canonical reference for all QE conventions used in this repository. Skill definitions, templates, and example artifacts all follow these standards.

---

## Contents

1. [IDs and Naming](#ids-and-naming)
2. [Test Case Writing Standards](#test-case-writing-standards)
3. [Test Case Categories](#test-case-categories)
4. [Priority Scale](#priority-scale)
5. [Status Icons](#status-icons)
6. [Jira Story Standards](#jira-story-standards)
7. [Regression Scope Standards](#regression-scope-standards)
8. [Platform and Environment](#platform-and-environment)
9. [Artifact File Names](#artifact-file-names)
10. [Template Syntax](#template-syntax)

---

## IDs and Naming

### Feature Codes

Feature codes are **2–4 uppercase letters** derived from the feature name. They are used as the namespace for all artifact IDs in a feature's test set.

| Feature | Code |
|---|---|
| Device Discovery and Connection | `DD` |
| Popup Visibility and Layer Transition Behavior | `PVL` |
| SVG Rendering | `SVG` |
| Shared Layer Navigation | `SLN` |
| Audio Routing | `AR` |
| _(new features)_ | Derive from first letters of significant words |

> **Rule**: If the derived code conflicts with an existing one, add a disambiguating letter. Codes must be unique within the repository.

---

### Test Case IDs

```
TC-<FEATURE_CODE>-<NNN>
```

| Part | Rule |
|---|---|
| `TC` | Fixed prefix — always `TC` |
| `FEATURE_CODE` | Uppercase feature code (see above) |
| `NNN` | 3-digit zero-padded sequential number, starting at `001` |

**Examples**: `TC-DD-001`, `TC-PVL-012`, `TC-SVG-003`

IDs are assigned sequentially within a feature set. Do not reuse or skip IDs. If a test case is removed, its ID is retired (not reassigned).

---

### Smoke Test IDs

```
SMOKE-<FEATURE_CODE>-<NNN>
```

Smoke test IDs follow the same pattern as test case IDs but use the `SMOKE` prefix. They identify checks in the regression smoke suite that are **distinct from regular test cases** — either new minimal checks written for the smoke suite, or selected references to existing TCs.

**Examples**: `SMOKE-DD-001`, `SMOKE-PVL-003`

When a smoke check directly maps to an existing test case, use the TC reference in the `TC Reference` column of the smoke suite table rather than creating a new SMOKE ID.

---

### Jira Story Summaries

```
[QE] <Feature Name> — <Story Type>
```

**Examples**:
- `[QE] Device Discovery and Connection — Test Execution`
- `[QE] Popup Visibility and Layer Transition Behavior — Test Execution`
- `[QE] Audio Routing — Test Planning`

**Story types** and when to use them:

| Story Type | Use when |
|---|---|
| Test Planning | Covers authoring test cases, environment setup, and document review |
| Test Execution | Covers running test cases against a build and logging results |
| Defect Investigation | Covers root cause analysis, regression verification, and defect triage |
| QE Sign-Off | Covers final verification, release readiness assessment |

Default to **Test Execution** if the story type is not specified.

---

## Test Case Writing Standards

### Steps

Every step must be:

| Rule | Example |
|---|---|
| **Imperative voice** — start with a verb | "Tap", "Enter", "Swipe", "Rotate", "Kill", "Enable", "Disable" |
| **One action per step** — no compound steps | ❌ "Tap Connect and wait for the design to load" → ✅ split into two steps |
| **Specific to the iOS Viewer UI** | "Tap the **Connect** button in the Device List" not "tap the button" |
| **Ordered**: setup → action → verification | Preconditions handle setup; steps begin at the action under test |

---

### Expected Results

Every expected result must be:

| Rule | Example |
|---|---|
| **Observable** — what the tester sees, hears, or experiences | ✅ "The device list refreshes and the Core appears within 10 seconds" |
| **Specific** — no vague qualifiers | ❌ "The app behaves correctly" · ❌ "The feature works as expected" |
| **Complete** — covers primary outcome and notable secondary UI changes | Include both the main assertion and any state changes visible to the tester |

---

### Preconditions

Preconditions are written as checkboxes (`- [ ]`). Each precondition is a state that must be true before step 1 is executed. Do not include setup actions as preconditions — if setup requires interaction, it belongs in the Steps.

---

### Test Case Structure

```
### TC-FEAT-NNN: <Title>

**Priority**: P1 / P2 / P3
**Category**: <Category>
**Test Type**: Positive / Negative / Exploratory

**Preconditions**:
- [ ] <condition one>
- [ ] <condition two>

**Steps**:
1. <action step one>
2. <action step two>

**Expected Result**:
> <Observable, specific, complete assertion>

**Actual Result**: _(leave blank — filled during execution)_
**Status**: ⬜ Not Run
**Notes**: _(optional)_
```

---

## Test Case Categories

Generate test cases across these seven categories. Include a category only if it applies to the feature; note excluded categories with a brief reason.

| Category | Description |
|---|---|
| **Functional** | Core user-facing behavior works as specified |
| **Negative** | Invalid inputs, missing data, error states handled gracefully |
| **Edge Case** | Boundaries, timeouts, state transitions, re-entry conditions |
| **Network** | Behavior on poor network, disconnection, reconnection |
| **iOS Platform** | Permissions prompts, background/foreground transitions, orientation, multitasking |
| **Accessibility** | VoiceOver navigation, Dynamic Type scaling, minimum 44×44 pt tap targets |
| **Regression** | Previously passing flows that must not break with this feature change |

### Mandatory iOS Scenarios

Every feature test set must include **at minimum**:

| Scenario | Requirement |
|---|---|
| Happy path — iPad (landscape) | Primary use case, nominal conditions, landscape orientation |
| Happy path — iPhone (portrait) | Same flow on smaller form factor, portrait orientation |
| Network interruption mid-flow | Disable Wi-Fi during the feature interaction; observe; re-enable |
| App backgrounded mid-flow | Press Home; return to app; verify state is preserved or gracefully recovered |
| VoiceOver navigation | All interactive elements are reachable by VoiceOver and have meaningful labels |

Add further mandatory cases for the feature's domain (e.g., Bluetooth permission denied if the feature uses Bluetooth).

---

## Priority Scale

| Priority | Meaning | Criteria |
|---|---|---|
| **P1** | Blocker | Feature does not function at all; blocks shipment; must pass before any sign-off |
| **P2** | Significant | Core feature is degraded; significant user impact; should pass before sprint sign-off |
| **P3** | Minor | Cosmetic, edge-case, or low-impact; can be deferred to the next cycle |

> **Rule**: Assign priority based on user impact, not technical complexity. A P1 case is one whose failure means the feature cannot ship.

---

## Status Icons

Used in test case execution logs, smoke suite execution logs, and summary tables:

| Icon | Meaning |
|---|---|
| ⬜ | Not Run — test has not been executed |
| ✅ | Pass — expected result observed |
| ❌ | Fail — expected result NOT observed; a defect should be filed |
| ⏭ | Skipped — test intentionally not run (with reason in Notes) |
| 🔄 | Blocked — test cannot run due to a dependency or environment issue |

---

## Jira Story Standards

### Metadata Fields

| Field | Standard value |
|---|---|
| Issue Type | `Story` |
| Component | `iOS Viewer` |
| Labels | Always: `qe`, `manual-testing`, `ios-viewer`. Add `accessibility` if accessibility cases exist. |
| Fix Version | From sprint planning or `TBD` |
| Assignee / Reporter / Sprint | `<TBD>` at creation time |

### Story Point Estimation

| Test Case Count | Story Points |
|---|---|
| 1–5 | 1 |
| 6–10 | 2 |
| 11–20 | 3 |
| 21–35 | 5 |
| 36+ | 8 — consider splitting into two stories |

### Acceptance Criteria Format

Use **Given / When / Then** format. Each AC must be:

- Independently verifiable
- Written from the user or tester perspective
- Referenced to a P1 test case where one exists
- Including at least one negative/error scenario

```
AC-01: Given <precondition>, when <action>, then <expected outcome>.
```

### Sub-Tasks

Create one sub-task per test case category present, plus standard execution and sign-off tasks:

```
ST-01: Author test cases — <Category> (<count> cases)
ST-02: Execute test cases — <Category>
...
ST-NN: Execute regression suite
ST-NN: Log and triage defects
ST-NN: QE sign-off and close story
```

---

## Regression Scope Standards

### Risk Levels

| Level | Definition | Action required |
|---|---|---|
| 🔴 **HIGH** | Subsystem shares state, data, or logic with the feature. Regression failures are **likely** if the feature code changes. | Must be tested before sign-off. Include in P1 validation areas. |
| 🟡 **MED** | Subsystem shares UI surfaces, lifecycle events, or platform permissions. Regression is **possible** but not certain. | Test during the standard regression pass. Include in P2 validation areas. |
| 🟢 **LOW** | Subsystem is architecturally isolated from the feature. Regression is **unlikely**. | Single spot-check or existing automated coverage is sufficient. |

### Subsystem Relationship Types

| Relationship | Risk assigned |
|---|---|
| **Direct** — this subsystem IS the feature under test | Always HIGH |
| **Shared state** — subsystem reads or writes data the feature changes | HIGH |
| **Shared logic** — subsystem uses code paths the feature modifies | HIGH |
| **Shared surface** — subsystem renders UI that co-exists with the feature | MED |
| **Shared lifecycle** — subsystem is affected by the same iOS lifecycle events | MED |
| **Isolated** — no known dependency on the changed feature | LOW |

### Standard Q-SYS iOS Viewer Subsystems

Every regression scope analysis maps the feature against these 11 standard subsystems:

| Subsystem ID | Name | Responsibility |
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

### Smoke Suite Rules

| Rule | Value |
|---|---|
| Minimum checks | 3 |
| Maximum checks | 7 |
| Maximum time per check | 2 minutes |
| Maximum total suite time | 10 minutes |
| Required coverage | At least: one connectivity check, one rendering/navigation check, one feature-under-test check |
| ID reuse | Reference existing TC IDs where possible; create new SMOKE IDs only for checks without a matching TC |

---

## Platform and Environment

| Item | Standard |
|---|---|
| **Devices** | Physical iOS devices only — **no simulators** |
| **Form factors** | iPad (primary: landscape) and iPhone (primary: portrait) — both required |
| **Minimum iOS version** | iOS 16 |
| **Q-SYS Core** | Latest stable release unless the feature targets a specific version |
| **Network topology** | Local LAN for baseline; also test on restricted/corporate network if applicable |
| **App build** | QE builds only — no production App Store builds for in-sprint testing |

### Objective Verbs for Test Plan Goals

Use these verbs when writing test plan objectives to ensure testability:

| Verb | Use for |
|---|---|
| **Verify** | Confirming a specific behavior occurs as specified |
| **Confirm** | Checking that a state or value is as expected |
| **Validate** | End-to-end confirmation that a user scenario completes successfully |
| **Ensure** | Asserting a negative or safety condition (e.g., "Ensure no data loss occurs") |

Avoid "test that", "check if", or "see whether" — these are vague and not measurable.

---

## Artifact File Names

Each skill produces a specific output file. Names are fixed to support composability — later skills in the pipeline locate earlier outputs by filename.

| Artifact | File name | Produced by |
|---|---|---|
| Jira Story | `01-jira-story.md` | `qe-jira-stories` |
| QE Test Plan | `02-test-plan.md` | `qe-test-plan` |
| QE Test Cases | `03-test-cases.md` | `qe-test-cases` |
| Regression Scope | `04-regression-scope.md` | `qe-regression-scope` |

All four files for a feature live in the same directory. The numeric prefix (`01-`, `02-`, `03-`, `04-`) controls the display order in the file explorer: Jira Story → Test Plan → Test Cases → Regression Scope. In the `examples/` folder, this is `examples/<feature-slug>/`. In an OpenSpec workflow, this is `openspec/changes/<change-name>/`.

---

## Template Syntax

All templates in `templates/` use `{{PLACEHOLDER}}` syntax:

| Token | Meaning |
|---|---|
| `{{FEATURE_NAME}}` | Full human-readable feature name |
| `{{FEATURE_CODE}}` | 2–4 letter uppercase feature code |
| `{{AUTHOR}}` | Tester or document owner name |
| `{{DATE}}` | ISO 8601 date (YYYY-MM-DD) |
| `{{BUILD_NUMBER}}` | App build identifier |
| `{{RISK}}` | HIGH / MED / LOW |
| `{{TC_REF}}` | TC-FEAT-NNN reference |

**Rule**: All `{{PLACEHOLDER}}` tokens must be replaced before an artifact is published or submitted for review. If a value is unknown at creation time, replace the token with `TBD`.

---

## See Also

- [AGENTS.md](../AGENTS.md) — Skill index, workflow diagrams, and repository structure
- [docs/README.md](README.md) — Quick start, slash commands, and examples index
- [templates/](../templates/) — Fill-in-the-blank artifact templates
- [examples/](../examples/) — Complete worked examples for two real features
