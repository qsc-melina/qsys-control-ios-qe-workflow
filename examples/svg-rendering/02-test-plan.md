# Test Plan: SVG Rendering

**Feature**: SVG Rendering  
**Platform**: Q-SYS iOS Viewer  
**Version**: Q-SYS Core 9.10+ / iOS Viewer (current)  
**Author**: QE Team  
**Date**: 2026-05-28  
**Status**: Draft  

---

## 1. Objective

- Verify that SVG graphics embedded in Q-SYS UCI designs render correctly on initial page load across iPad and iPhone.
- Confirm that SVG fill, stroke, and opacity properties update in real time when driven by Q-SYS Core data changes.
- Validate that SVG elements scale correctly and preserve aspect ratio across all supported screen sizes and orientations.
- Ensure malformed or unsupported SVG content degrades gracefully without crashing the app.
- Confirm that VoiceOver accessibility labels are applied correctly to SVG-based interactive controls.
- Verify that SVG rendering changes do not regress existing non-SVG UCI control rendering.

---

## 2. Scope

### In Scope

- Initial render of SVG-based controls and decorative elements on page load
- Dynamic updates to SVG fill, stroke, and opacity driven by Q-SYS Core Named Control property changes
- Scaling and aspect-ratio preservation across device sizes and orientations
- Dark mode and light mode rendering
- SVG rendering during and after orientation change
- Graceful degradation for malformed or unsupported SVG syntax
- VoiceOver label accessibility for SVG-based interactive controls
- iOS version compatibility: iOS 16 and later
- iPad (landscape + portrait) and iPhone (portrait)
- Regression against existing non-SVG UCI control rendering

### Out of Scope

- SVG animation (SMIL or CSS animation within SVG is not supported by the app)
- SVG rendering in Q-SYS Designer preview
- PDF and bitmap graphics (separate rendering path)
- Q-SYS Designer and Q-SYS Core internal logic (unless it is the direct integration point)
- Performance benchmarking beyond acceptable-time checks
- Android and web platform equivalents

---

## 3. Test Approach

Testing for this feature is **manual** and executed on physical iOS devices.

| Approach | Description |
|---|---|
| **Functional testing** | Execute step-by-step test cases covering all in-scope scenarios |
| **Exploratory testing** | Unscripted sessions to uncover rendering edge cases and visual artifacts |
| **Regression testing** | Re-execute previously passing cases to confirm no regressions in UCI controls |
| **Accessibility testing** | Manual VoiceOver and Dynamic Type evaluation |

---

## 4. Test Environment

| Item | Value |
|---|---|
| **Platform** | iOS Viewer app on physical device |
| **iOS Versions** | iOS 18.x (latest) + iOS 16.x (minimum supported) |
| **Device Models** | iPad Pro 12.9" (6th gen), iPhone 15 Pro |
| **Q-SYS Core** | Core 9.10+ on Core 110f hardware |
| **Network** | Same subnet, mDNS enabled, 802.11ac Wi-Fi |
| **Q-SYS Design File** | `qe-svg-rendering-test-design.qsys` |
| **App Build** | TBD |

---

## 5. Test Scenarios

> _High-level scenarios only. Detailed steps belong in `test-cases.md`._

### Functional

| # | Scenario |
|---|---|
| F-01 | SVG graphic renders on first page load without visible artifacts on iPad |
| F-02 | SVG graphic renders on first page load without visible artifacts on iPhone |
| F-03 | SVG fill color updates within an acceptable time after a Q-SYS Core Named Control change |
| F-04 | SVG stroke color updates correctly when driven by a Core property change |
| F-05 | SVG opacity transitions smoothly when driven by a Core Named Control value change |
| F-06 | Multiple SVG elements on the same page render simultaneously without visual conflict |
| F-07 | SVG-based button control triggers the correct Core Named Control when tapped |
| F-08 | SVG background layer does not obscure foreground interactive UCI controls |
| F-09 | SVG renders correctly in dark mode |

### Negative

| # | Scenario |
|---|---|
| N-01 | Page containing a malformed SVG element loads without crashing |
| N-02 | SVG containing an unsupported filter element renders with graceful fallback (no crash) |
| N-03 | SVG referencing a missing external asset renders a blank or placeholder without crashing |

### Edge Cases

| # | Scenario |
|---|---|
| E-01 | SVG re-renders correctly after navigating away from and back to the page |
| E-02 | Large SVG file (> 500 KB) renders within an acceptable time frame |
| E-03 | SVG renders correctly when the app returns from the background mid-render |
| E-04 | SVG fill and stroke properties update correctly during degraded network conditions |
| E-05 | SVG on a page in a large design (> 20 pages) renders correctly |

### iOS Platform

| # | Scenario |
|---|---|
| I-01 | SVG renders correctly on iPad in landscape orientation |
| I-02 | SVG aspect ratio is preserved after rotating from landscape to portrait |
| I-03 | App backgrounded mid-SVG property update returns to correct visual state on foreground |

### Accessibility

| # | Scenario |
|---|---|
| A-01 | VoiceOver announces SVG-based button controls with the correct accessibility label |
| A-02 | Dynamic Type large text setting does not break SVG layout or clip elements |

### Regression

| # | Scenario |
|---|---|
| R-01 | Non-SVG UCI controls (sliders, buttons, text inputs) on the same page render and interact correctly |
| R-02 | Page navigation between SVG-containing and non-SVG pages functions correctly |
| R-03 | Existing device connection and discovery flow is unaffected by SVG rendering changes |

---

## 6. Pass / Fail Criteria

- **Pass**: All functional scenarios pass on both iPad and iPhone; no P1 defects open; no crashes observed during negative or edge case execution.
- **Fail**: Any P1 functional scenario fails; any crash or visual corruption defect is open; SVG content blocks the user from interacting with UCI controls.

---

## 7. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| SVG parser encounters unsupported syntax from a real-world design | Medium | High | Prepare test designs covering common SVG patterns; include malformed SVG in negative suite |
| Core property update timing causes SVG state to lag behind actual Core value | Medium | Medium | Use Core simulator to inject rapid value changes; measure update latency |
| Aspect ratio distortion on non-standard screen sizes | Medium | Medium | Test on both iPad Pro 12.9" and iPhone 15 Pro; verify with ruler tool in device simulator as supplement |
| VoiceOver label mapping from Q-SYS UCI property to iOS accessibility element | Low | Medium | Review UCI design accessibility field population; include explicit VoiceOver test cases |
| Large SVG asset causing memory pressure on older iOS 16 devices | Low | High | Use 500 KB threshold SVG in test design; monitor Xcode Instruments during execution |
