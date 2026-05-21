# Jira Story: {{STORY_SUMMARY}}

<!-- 
  TEMPLATE INSTRUCTIONS (remove before publishing):
  - Replace all {{PLACEHOLDER}} values
  - The "Metadata" block maps to Jira fields — copy into the story creation form
  - The "Description" section maps to the Jira Description field
  - Sub-tasks are created as child issues in Jira
  - Story Points use Fibonacci: 1, 2, 3, 5, 8, 13
-->

---

## Metadata

```
Summary:       [QE] {{FEATURE_NAME}} — {{STORY_TYPE}}
Issue Type:    Story
Epic Link:     {{EPIC_LINK}}
Component:     iOS Viewer
Labels:        qe, manual-testing, ios-viewer{{EXTRA_LABELS}}
Fix Version:   {{FIX_VERSION}}
Story Points:  {{STORY_POINTS}}
Assignee:      {{ASSIGNEE}}
Reporter:      {{REPORTER}}
Sprint:        {{SPRINT}}
Priority:      {{PRIORITY}}
```

> **Story Type options**: Test Planning | Test Execution | Defect Investigation | QE Sign-Off

---

## Description

### Context

{{CONTEXT_PARAGRAPH}}

> _Example: The Device Discovery feature allows Q-SYS iOS Viewer users to find and connect to Q-SYS Core devices on their local network. This story covers manual QE verification of the discovery and connection flows across supported iOS devices and network topologies._

---

### Testing Scope

**In Scope:**
- {{INSCOPE_1}}
- {{INSCOPE_2}}
- iOS {{IOS_VERSION_RANGE}} on iPad and iPhone
- Light mode and dark mode
- VoiceOver and Dynamic Type accessibility

**Out of Scope:**
- {{OUTOFSCOPE_1}}
- Q-SYS Core internal behavior beyond the integration boundary
- Android and web platforms

---

### Test Environment

| Item | Value |
|---|---|
| **Platform** | Q-SYS iOS Viewer on physical iOS device |
| **iOS Version** | {{IOS_VERSION_RANGE}} |
| **Device Models** | {{DEVICE_MODELS}} |
| **Q-SYS Core Version** | {{QSYS_CORE_VERSION}} |
| **Core Hardware** | {{CORE_HARDWARE}} |
| **Network Topology** | {{NETWORK_TOPOLOGY}} |
| **Q-SYS Design File** | `{{DESIGN_FILE}}` |

---

### References

| Reference | Link |
|---|---|
| Test Plan | `{{TEST_PLAN_PATH}}` |
| Test Cases | `{{TEST_CASES_PATH}}` |
| Feature Spec | `{{SPEC_PATH}}` |
| Parent Epic | `{{EPIC_LINK}}` |
| OpenSpec Change | `openspec/changes/{{CHANGE_NAME}}/` |

---

## Acceptance Criteria

> _Each AC must be independently verifiable. Use Given/When/Then format._

**AC-01**: Given {{GIVEN_1}}, when {{WHEN_1}}, then {{THEN_1}}.

**AC-02**: Given {{GIVEN_2}}, when {{WHEN_2}}, then {{THEN_2}}.

**AC-03**: Given {{GIVEN_3}}, when {{WHEN_3}}, then {{THEN_3}}.

**AC-04 (Negative)**: Given {{GIVEN_4_NEGATIVE}}, when {{WHEN_4}}, then {{THEN_4_ERROR_STATE}}.

**AC-05 (Accessibility)**: Given VoiceOver is enabled, when the user navigates the {{FEATURE_NAME}} flow, then all interactive elements are reachable and announce meaningful labels.

---

## Sub-Tasks

> _Create these as child issues in Jira. Link back to this story._

- [ ] **ST-01**: Author test cases — Functional ({{FUNCTIONAL_CASE_COUNT}} cases, P1/P2)
- [ ] **ST-02**: Author test cases — Negative / Edge Cases ({{NEGATIVE_CASE_COUNT}} cases, P2/P3)
- [ ] **ST-03**: Execute test cases — Functional on iPad + iPhone
- [ ] **ST-04**: Execute test cases — Network and iOS Platform scenarios
- [ ] **ST-05**: Execute test cases — Accessibility (VoiceOver + Dynamic Type)
- [ ] **ST-06**: Execute regression suite
- [ ] **ST-07**: Log and triage defects; retest fixes
- [ ] **ST-08**: QE sign-off — update story with results and close

---

## Definition of Done

- [ ] All P1 and P2 test cases pass on at least 1 iPad and 1 iPhone
- [ ] No P1 or P2 defects open at time of sign-off
- [ ] All Accessibility test cases pass with VoiceOver enabled
- [ ] Test results documented in the Execution Log (`test-cases.md`)
- [ ] Story updated with final status and linked defects
- [ ] QE sign-off comment added to story

---

## Notes

> _(Use this section for blockers, dependencies, or team communication during the sprint.)_

- {{NOTE_1}}
