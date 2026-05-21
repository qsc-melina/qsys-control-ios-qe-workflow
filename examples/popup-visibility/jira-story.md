# Jira Story: [QE] Popup Visibility and Layer Transition Behavior — Test Execution

---

## Metadata

```
Summary:       [QE] Popup Visibility and Layer Transition Behavior — Test Execution
Issue Type:    Story
Epic Link:     <Epic Link TBD>
Component:     iOS Viewer
Labels:        qe, manual-testing, ios-viewer, accessibility
Fix Version:   TBD
Story Points:  5
Assignee:      <Assignee TBD>
Reporter:      <Reporter TBD>
Sprint:        <Sprint TBD>
Priority:      High
```

> **Story Type**: Test Execution

---

## Description

### Context

The Popup Visibility and Layer Transition Behavior feature governs how modal popup panels open and close within the Q-SYS iOS Viewer UCI (User Control Interface), including their fade-in and fade-out animations, and how underlying shared layers remain visible and correctly restore state after popup dismissal. This story covers manual QE verification of the full popup lifecycle — open, close, transition, shared layer state, rapid interaction stress, accessibility, and regression of existing controls and navigation.

---

### Testing Scope

**In Scope:**
- Popup fade-in on open and fade-out on close
- Popup open/close triggered by UCI design buttons (tap the **Open Popup** / **Close** controls)
- Popup dismiss via tap-outside (where the design supports it)
- Shared layer visibility while a popup is displayed
- Shared layer state restoration after popup dismissal
- Multiple popup stacking (second popup opened over first)
- Layer visibility persistence across page navigation
- Rapid successive open/close interaction (stress testing)
- Overlapping transitions: new popup triggered during a running fade-out
- App backgrounded mid-transition; app backgrounded while popup is open
- Negative states: open trigger on already-open popup; close trigger on no popup; network disconnect mid-transition
- Long (scrollable) popup content
- iOS 16.x and iOS 17.x on iPad Pro 12.9" and iPhone 15
- Light mode and dark mode
- VoiceOver focus management (enters popup on open, returns to page on close)
- Dynamic Type Accessibility XL rendering within popups

**Out of Scope:**
- Q-SYS Designer popup authoring and configuration
- Q-SYS Core scripting logic that drives popup state changes
- Network latency-driven state updates (covered in network reliability testing)
- Android and web platform equivalents
- Animation frame rate or performance benchmarking

---

### Test Environment

| Item | Value |
|---|---|
| **Platform** | Q-SYS iOS Viewer on physical iOS device |
| **iOS Version** | iOS 17.x (primary), iOS 16.x (secondary) |
| **Device Models** | iPad Pro 12.9" (M2), iPhone 15 |
| **Q-SYS Core Version** | TBD — minimum version supporting popup scripting |
| **Core Hardware** | Q-SYS Core 510i (or equivalent lab hardware) |
| **Network Topology** | Same subnet LAN, stable connection |
| **Q-SYS Design File** | `qe-popup-layer-test-design.qsys` |

> **Design prerequisite**: `qe-popup-layer-test-design.qsys` must include a popup with fade transitions, a shared layer with interactive controls, a second popup for stacking tests, and a tall popup for scroll testing.

---

### References

| Reference | Link |
|---|---|
| Test Plan | [`test-plan.md`](./test-plan.md) |
| Test Cases | [`test-cases.md`](./test-cases.md) |
| Feature Spec | `openspec/specs/popup-visibility-layer-transitions/spec.md` |
| Parent Epic | `<Epic Link TBD>` |
| OpenSpec Change | `openspec/changes/popup-visibility-layer-transitions/` |

---

## Acceptance Criteria

**AC-01**: Given the UCI design is displayed and no popup is open, when the user taps the Open Popup trigger button, then the popup appears with a visible fade-in animation and is fully interactive when the transition completes.

**AC-02**: Given a popup is fully open, when the user taps the close control within the popup, then the popup disappears with a visible fade-out animation, and the underlying UCI design page is fully visible and interactive after the transition.

**AC-03**: Given a shared layer is visible on the design page, when the user opens a popup over it, then the shared layer remains visible throughout the popup's open state and is fully restored (same position, same state) after the popup is dismissed.

**AC-04**: Given a popup is currently open, when the user taps the Open Popup trigger button again, then no duplicate or second popup appears — the existing popup remains unchanged and the app does not crash.

**AC-05 (Negative)**: Given no popup is currently open, when the app receives a close-popup signal, then no crash, error dialog, or unexpected UI change occurs.

**AC-06 (Edge Case)**: Given the user rapidly taps the open and close trigger alternately 10 times, when the tapping stops, then the UI settles into a stable state with no visual corruption, frozen animation, or unresponsive controls.

**AC-07 (Accessibility)**: Given VoiceOver is enabled, when a popup opens, then VoiceOver focus moves into the popup automatically, elements behind the popup are not reachable via swipe, and VoiceOver can navigate within the popup and activate the close control.

---

## Sub-Tasks

> _Create these as child issues in Jira linked to this story._

- [ ] **ST-01**: Author / review test design file — `qe-popup-layer-test-design.qsys` must include popup, shared layer, second popup, tall popup, and rapid-trigger controls
- [ ] **ST-02**: Execute test cases — Functional (TC-PVL-001 through TC-PVL-008) on iPad Pro 12.9" + iPhone 15
- [ ] **ST-03**: Execute test cases — Negative scenarios (TC-PVL-009 through TC-PVL-011)
- [ ] **ST-04**: Execute test cases — Edge Cases including rapid interaction stress (TC-PVL-012 through TC-PVL-015)
- [ ] **ST-05**: Execute test cases — iOS Platform (TC-PVL-016, TC-PVL-017) including orientation change
- [ ] **ST-06**: Execute test cases — Accessibility (TC-PVL-018 through TC-PVL-020) with VoiceOver and Dynamic Type Accessibility XL
- [ ] **ST-07**: Execute regression suite (TC-PVL-021, TC-PVL-022)
- [ ] **ST-08**: Log and triage all defects found; retest fixed issues; update Execution Log in `test-cases.md`
- [ ] **ST-09**: QE sign-off — update story with final results, link defects, add sign-off comment

---

## Definition of Done

- [ ] All P1 test cases (TC-PVL-001, TC-PVL-002, TC-PVL-003, TC-PVL-005, TC-PVL-006, TC-PVL-012) pass on iPad Pro 12.9" and iPhone 15
- [ ] All P2 test cases pass on at least one device; known failures have linked Jira defects
- [ ] No P1 or P2 defects remain open at time of sign-off
- [ ] TC-PVL-018 and TC-PVL-019 pass with VoiceOver enabled on iOS 17.x (VoiceOver focus enters popup; popup is navigable; focus returns on close)
- [ ] TC-PVL-021 and TC-PVL-022 (regression) pass
- [ ] Execution Log in `test-cases.md` is filled with tester, date, build, and result for every executed case
- [ ] QE sign-off comment added to this Jira story: "QE PASS — [date] — [tester] — [build]"
- [ ] All linked defects are either Closed (Fixed) or have an accepted Won't Fix justification

---

## Notes

- **Design dependency (blocker for ST-02+)**: `qe-popup-layer-test-design.qsys` must be authored and deployed to the test Core before any execution sub-tasks can begin. Assign ST-01 to the QE lead and track completion separately.
- **Popup stacking behavior**: The expected behavior when a second popup is triggered over a first open popup (TC-PVL-007) must be confirmed with the dev team before test execution. Update TC-PVL-007's Expected Result with the confirmed behavior.
- **Rapid interaction (TC-PVL-012)**: Run this test case 3 times per device to check for non-deterministic failures. Log all three results.
- **Transition timing (iOS 16 vs iOS 17)**: Run TC-PVL-012 and TC-PVL-013 on both iOS versions — animation APIs differ between versions and edge cases may surface only on one.
