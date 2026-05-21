# Test Plan: {{FEATURE_NAME}}

<!-- 
  TEMPLATE INSTRUCTIONS (remove before publishing):
  - Replace all {{PLACEHOLDER}} values
  - Remove sections that do not apply
  - TBD is acceptable for unknown values at draft time
  - Keep language clear and tester-facing
-->

**Feature**: {{FEATURE_NAME}}  
**Platform**: Q-SYS iOS Viewer  
**Version**: {{QSYS_CORE_VERSION}} / iOS Viewer {{IOS_VIEWER_VERSION}}  
**Author**: {{AUTHOR}}  
**Date**: {{DATE}}  
**Status**: Draft | In Review | Approved  

---

## 1. Objective

> _What does this test plan aim to validate? Write 3–6 bullet points, each starting with "Verify", "Confirm", "Validate", or "Ensure"._

- Verify that ...
- Confirm that ...
- Validate that ...

---

## 2. Scope

### In Scope

- {{INSCOPE_ITEM_1}}
- {{INSCOPE_ITEM_2}}
- iOS version compatibility: iOS 16 and later
- iPad (landscape + portrait) and iPhone (portrait)
- Light mode and dark mode
- VoiceOver and Dynamic Type accessibility
- Network interruption and reconnection scenarios

### Out of Scope

- {{OUTOFSCOPE_ITEM_1}}
- Q-SYS Designer and Q-SYS Core internal logic (unless it is the direct integration point)
- Performance benchmarking (unless feature is performance-sensitive)
- Android and web platform equivalents

---

## 3. Test Approach

Testing for this feature is **manual** and executed on physical iOS devices.

| Approach | Description |
|---|---|
| **Functional testing** | Execute step-by-step test cases covering all in-scope scenarios |
| **Exploratory testing** | Unscripted sessions to uncover edge cases and UX issues |
| **Regression testing** | Re-execute previously passing cases to confirm no regressions |
| **Accessibility testing** | Manual VoiceOver and Dynamic Type evaluation |

---

## 4. Test Environment

| Item | Value |
|---|---|
| **Platform** | iOS Viewer app on physical device |
| **iOS Versions** | {{IOS_VERSION_RANGE}} (e.g., iOS 17.x + iOS 16.x) |
| **Device Models** | {{DEVICE_MODELS}} (e.g., iPad Pro 12.9", iPhone 15) |
| **Q-SYS Core** | {{QSYS_CORE_VERSION}} on {{CORE_HARDWARE}} |
| **Network** | {{NETWORK_TOPOLOGY}} (e.g., same subnet, mDNS enabled) |
| **Q-SYS Design File** | `{{DESIGN_FILE_NAME}}` |
| **App Build** | {{BUILD_NUMBER}} |

---

## 5. Test Scenarios

> _High-level scenarios only. Detailed steps belong in `test-cases.md`._

### Functional

| # | Scenario |
|---|---|
| F-01 | {{FUNCTIONAL_SCENARIO_1}} |
| F-02 | {{FUNCTIONAL_SCENARIO_2}} |

### Negative

| # | Scenario |
|---|---|
| N-01 | {{NEGATIVE_SCENARIO_1}} |
| N-02 | {{NEGATIVE_SCENARIO_2}} |

### Edge Cases

| # | Scenario |
|---|---|
| E-01 | {{EDGE_SCENARIO_1}} |
| E-02 | {{EDGE_SCENARIO_2}} |

### Accessibility

| # | Scenario |
|---|---|
| A-01 | VoiceOver can navigate all interactive elements in the {{FEATURE_NAME}} flow |
| A-02 | Dynamic Type (Accessibility XL) does not truncate or overlap critical labels |

### Regression

| # | Scenario |
|---|---|
| R-01 | {{REGRESSION_SCENARIO_1}} |

---

## 6. Pass / Fail Criteria

### Pass

- All Functional and Negative scenarios pass on at least 1 iPad and 1 iPhone
- No P1 or P2 defects remain open at time of sign-off
- All Accessibility scenarios pass with VoiceOver enabled

### Fail

- Any Functional scenario produces a crash, data loss, or incorrect state
- Any P1 defect is open at time of sign-off
- The feature is blocked on all tested device configurations

---

## 7. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| {{RISK_1}} | Low / Med / High | Low / Med / High | {{MITIGATION_1}} |
| {{RISK_2}} | Low / Med / High | Low / Med / High | {{MITIGATION_2}} |
| Physical device unavailable for testing | Low | High | Maintain a shared device pool; book devices 24h in advance |
| Q-SYS Core firmware unstable during testing | Med | High | Pin Core firmware version; coordinate with firmware team |

---

## 8. Dependencies

- [ ] {{DEPENDENCY_1}} (e.g., "Feature branch merged to `main`")
- [ ] {{DEPENDENCY_2}} (e.g., "Q-SYS Core {{version}} available in test lab")
- [ ] Test cases authored and reviewed (see `test-cases.md`)
- [ ] Test environment provisioned (see §4)

---

## 9. References

| Reference | Link / Path |
|---|---|
| Feature Spec | `{{SPEC_PATH}}` |
| OpenSpec Change | `openspec/changes/{{CHANGE_NAME}}/` |
| Related Jira Story | `{{JIRA_STORY_LINK}}` |
| Q-SYS iOS Viewer Docs | `{{DOCS_LINK}}` |
| Previous Test Plan | `{{PREV_TEST_PLAN_PATH}}` |
