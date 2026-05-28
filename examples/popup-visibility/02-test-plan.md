# Test Plan: Popup Visibility and Layer Transition Behavior

**Feature**: Popup Visibility and Layer Transition Behavior  
**Platform**: Q-SYS iOS Viewer  
**Version**: Q-SYS Core TBD / iOS Viewer TBD  
**Author**: QE Team  
**Date**: 2026-05-20  
**Status**: Draft  

---

## 1. Objective

- Verify that popups open and close with the correct fade-in and fade-out transition animations as defined in the Q-SYS UCI design
- Confirm that shared layers remain visible and functional when a popup is open above them
- Validate that shared layer visibility state is correctly restored after a popup is dismissed
- Ensure rapid successive open/close interactions (tap storms) do not produce visual corruption, duplicate popups, or application crashes
- Verify that negative states — triggering open on an already-open popup, or close on an already-closed popup — are handled gracefully without errors
- Confirm all popup and layer transition interactions are accessible to VoiceOver users and remain legible at Dynamic Type Accessibility XL

---

## 2. Scope

### In Scope

- Popup open and close behavior triggered by UCI design buttons
- Fade-in transition when a popup opens
- Fade-out transition when a popup closes
- Popup close via the popup's own close control and (where applicable) tap-outside dismissal
- Shared layer visibility while a popup is displayed over the design
- Shared layer visibility restoration after popup dismissal
- Multiple popup stacking: a second popup opened while a first is visible
- Layer visibility state persistence when navigating away from and back to a page
- Rapid successive open/close interactions (stress tapping)
- Popup behavior when the app is backgrounded mid-transition and foregrounded
- iOS version compatibility: iOS 16 and later
- iPad (landscape and portrait) and iPhone (portrait)
- Light mode and dark mode
- VoiceOver focus management when a popup opens and closes
- Dynamic Type (Accessibility XL) rendering within popups

### Out of Scope

- Q-SYS Designer popup configuration authoring (not a Viewer concern)
- Q-SYS Core scripting logic that controls popup visibility (only the rendered result is verified)
- Network latency-driven popup state updates (covered separately in network reliability testing)
- Android and web platform equivalents
- Performance benchmarking of transition frame rates

---

## 3. Test Approach

Testing for this feature is **manual** and executed on physical iOS devices.

| Approach | Description |
|---|---|
| **Functional testing** | Execute step-by-step test cases covering popup open/close, fade transitions, and layer state |
| **Exploratory testing** | Unscripted sessions targeting rapid interaction patterns, edge-case transition sequences, and visual glitches |
| **Stress testing** | Manual rapid-tap sequences to validate robustness against duplicate triggers and corrupted transition state |
| **Accessibility testing** | Manual VoiceOver and Dynamic Type evaluation of popup focus management, element labeling, and layout |
| **Regression testing** | Re-execute key existing navigation and control interaction cases to confirm no regressions |

---

## 4. Test Environment

| Item | Value |
|---|---|
| **Platform** | Q-SYS iOS Viewer app on physical device |
| **iOS Versions** | iOS 17.x (primary), iOS 16.x (secondary) |
| **Device Models** | iPad Pro 12.9" (landscape + portrait), iPhone 15 (portrait) |
| **Q-SYS Core** | TBD — minimum Core version that supports popup scripting |
| **Core Hardware** | Q-SYS Core 510i (or equivalent lab hardware) |
| **Network** | Same subnet LAN; stable connection unless explicitly testing network disruption |
| **Q-SYS Design File** | `qe-popup-layer-test-design.qsys` — custom design containing: popup panels, shared layers, layer-visibility buttons, rapid-trigger controls |
| **App Build** | TBD — TestFlight build provided by iOS Viewer team |

> **Design requirements**: The test design must include at minimum:
> - One popup with a defined fade-in and fade-out transition
> - One shared layer visible on the page that contains the popup trigger
> - A second popup to validate stacking behavior
> - A button that triggers rapid repeated open signals (for stress testing)

---

## 5. Test Scenarios

### Functional

| # | Scenario |
|---|---|
| F-01 | A popup opens with a visible fade-in transition when its trigger button is tapped |
| F-02 | A popup closes with a visible fade-out transition when its close control is tapped |
| F-03 | A popup closes when the user taps outside the popup boundary (if the design supports dismiss-on-outside-tap) |
| F-04 | A shared layer displayed behind a popup remains visible while the popup is open |
| F-05 | A shared layer's visibility state is correctly restored after the popup is dismissed |
| F-06 | A second popup can be opened while a first popup is already visible (stacking) |
| F-07 | Layer visibility state on a page is preserved when the user navigates away and returns |

### Negative

| # | Scenario |
|---|---|
| N-01 | Tapping the popup open trigger when the popup is already open does not produce a duplicate popup |
| N-02 | Tapping the popup close control when no popup is open does not crash or produce errors |
| N-03 | A network disconnection while a popup is mid-transition (fade-in) is handled gracefully |

### Edge Cases

| # | Scenario |
|---|---|
| E-01 | Rapidly tapping the open and close trigger alternately does not produce visual corruption or a hung transition state |
| E-02 | Opening a new popup while the previous popup's fade-out transition is still running produces a correct visual result |
| E-03 | The app is backgrounded mid-fade-transition and foregrounded — the popup is in the correct open or closed state |
| E-04 | A popup whose content is taller than the screen height is scrollable and does not clip content |

### Accessibility

| # | Scenario |
|---|---|
| A-01 | VoiceOver focus moves into the popup when it opens; focus does not remain trapped on elements behind the popup |
| A-02 | VoiceOver can navigate all interactive elements within the popup and activate the close control |
| A-03 | Dynamic Type set to Accessibility XL does not cause text truncation or element overlap within the popup |

### Regression

| # | Scenario |
|---|---|
| R-01 | Controls on a shared layer (buttons, sliders) function correctly after a popup open/close cycle |
| R-02 | Page navigation (switching between UCI pages) is unaffected after a popup open/close cycle |

---

## 6. Pass / Fail Criteria

### Pass

- All Functional scenarios pass on iPad Pro 12.9" and iPhone 15
- All Negative scenarios produce a graceful, non-crashing result
- No P1 or P2 defects remain open at time of sign-off
- All Accessibility scenarios pass with VoiceOver enabled on iOS 17.x
- Rapid-interaction Edge Case (E-01) passes on both iPad and iPhone
- Regression scenarios show no change in behavior

### Fail

- Any Functional scenario results in a crash, missing transition, or incorrect layer state
- Rapid interaction (E-01) causes a crash or permanent visual corruption
- Any P1 defect is open at time of sign-off
- VoiceOver focus does not move into the popup on open (A-01 fails)

---

## 7. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Test design file not available at sprint start | Med | High | Author `qe-popup-layer-test-design.qsys` in the previous sprint; validate it against Core before testing begins |
| Transition timing differs between iOS 16 and iOS 17 animation APIs | Med | Med | Run E-01 (rapid interaction) and E-02 (overlapping transitions) on both iOS versions |
| Popup stacking behavior undefined in spec | Med | High | Clarify with iOS Viewer dev team before test case authoring; document the agreed behavior in test cases |
| Shared layer state management regression from a parallel feature branch | Low | High | Run R-01 and R-02 on every build, not only the final candidate |
| Physical device unavailable for stress-testing sessions | Low | Med | Book device 24h in advance; stress tests can be run on simulator as a secondary check |

---

## 8. Dependencies

- [ ] iOS Viewer TestFlight build containing the popup / layer transition feature is available
- [ ] `qe-popup-layer-test-design.qsys` design file authored, reviewed, and deployed to test Core
- [ ] Q-SYS Core minimum version confirmed with dev team
- [ ] Popup stacking behavior definition confirmed and documented
- [ ] Test cases authored and reviewed (see `test-cases.md`)
- [ ] Test lab network verified: stable LAN, no packet loss on primary test VLAN

---

## 9. References

| Reference | Link / Path |
|---|---|
| Feature Spec | `openspec/specs/popup-visibility-layer-transitions/spec.md` |
| OpenSpec Change | `openspec/changes/popup-visibility-layer-transitions/` |
| Related Jira Story | `[QE] Popup Visibility and Layer Transition Behavior — Test Execution` |
| Test Cases | [`test-cases.md`](./test-cases.md) |
| Jira Story | [`jira-story.md`](./jira-story.md) |
| Device Discovery Test Plan (reference) | [`examples/device-discovery/test-plan.md`](../device-discovery/test-plan.md) |
