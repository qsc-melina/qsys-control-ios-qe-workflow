# Jira Story: SVG Rendering — Test Execution

---

## Metadata

| Field | Value |
|---|---|
| **Summary** | [QE] SVG Rendering — Test Execution |
| **Issue Type** | Story |
| **Epic Link** | `<Epic Link TBD>` |
| **Component** | iOS Viewer |
| **Labels** | `qe`, `manual-testing`, `ios-viewer`, `accessibility` |
| **Fix Version** | TBD |
| **Story Points** | 5 |
| **Assignee** | `<Assignee TBD>` |
| **Reporter** | `<Reporter TBD>` |
| **Sprint** | `<Sprint TBD>` |

---

## Description

### Context

This story covers manual QE execution for the SVG Rendering feature in the Q-SYS iOS Viewer. The feature governs how SVG graphics embedded in Q-SYS UCI designs are parsed, rendered, and dynamically updated from Q-SYS Core Named Control property changes. Validation must confirm correct initial render, real-time property updates (fill, stroke, opacity), graceful degradation for malformed or unsupported SVG, aspect-ratio preservation across orientations, and regression safety for existing non-SVG UCI controls.

### Testing Scope

- Initial render of SVG elements on page load across iPad and iPhone
- Real-time SVG fill, stroke, and opacity updates driven by Q-SYS Core Named Controls
- Scaling and aspect-ratio preservation across device sizes and orientations
- Dark mode rendering for SVG elements using system colors
- Graceful degradation for malformed SVG, unsupported filters, and missing external assets
- VoiceOver accessibility labels for SVG-based interactive controls
- Regression coverage for non-SVG UCI controls, page navigation, and device connection

### Out of Scope

- SVG animation (SMIL / CSS animation not supported by the app)
- SVG rendering in Q-SYS Designer preview
- PDF and bitmap graphics (separate rendering path)
- Android and web platform equivalents

### Test Environment

| Item | Value |
|---|---|
| Platform | iOS Viewer (iPad + iPhone) |
| iOS Version | iOS 18.x (latest) + iOS 16.x (minimum) |
| Q-SYS Core Version | Core 9.10+ |
| Network | Same subnet, mDNS enabled, 802.11ac Wi-Fi |
| Test Device(s) | iPad Pro 12.9" (6th gen), iPhone 15 Pro |
| Q-SYS Design File | `qe-svg-rendering-test-design.qsys` |

### References

- Test Plan: `examples/svg-rendering/02-test-plan.md`
- Test Cases: `examples/svg-rendering/03-test-cases.md`
- Regression Scope: `examples/svg-rendering/04-regression-scope.md`
- Feature Spec: `<link TBD>`
- Parent Epic: `<link TBD>`

---

## Acceptance Criteria

**AC-01**: Given the iOS Viewer is connected to a Core running a design with SVG elements, when the UCI page loads, then all SVG graphics are fully rendered with correct fill colors and dimensions within 3 seconds on both iPad and iPhone.

**AC-02**: Given an SVG element whose fill color is bound to a Q-SYS Core Named Control, when the Named Control value changes, then the SVG fill color updates within 500 ms of the Core change event with no flicker or intermediate incorrect color.

**AC-03**: Given an SVG element is visible, when the device is rotated between landscape and portrait, then the SVG element scales to fit the new viewport while preserving its aspect ratio with no visible distortion.

**AC-04**: Given a UCI page contains a malformed or unsupported SVG element, when the page loads, then the app does not crash and all other page controls remain functional.

**AC-05**: Given an SVG-based button control in the UCI design, when the tester taps the control in the iOS Viewer, then the corresponding Q-SYS Core Named Control reflects the correct value change and the button responds without requiring multiple taps.

**AC-06**: Given VoiceOver is enabled, when the tester navigates to a page with SVG-based interactive controls, then VoiceOver announces each control with its correct accessibility label and "Button" trait, and each control can be activated via double-tap.

**AC-07**: Given a UCI page contains both SVG elements and standard non-SVG UCI controls (sliders, buttons, text inputs), when the tester interacts with the non-SVG controls, then all controls respond correctly and identically to their behavior on pages without SVG content.

---

## Sub-Tasks

- [ ] **ST-01**: Set up test environment — load `qe-svg-rendering-test-design.qsys` on Core 9.10+; verify all SVG pages are accessible on iPad Pro 12.9" and iPhone 15 Pro
- [ ] **ST-02**: Execute functional test cases — TC-SVG-001 through TC-SVG-009 (9 cases: initial render, fill/stroke/opacity updates, dark mode, z-order)
- [ ] **ST-03**: Execute negative test cases — TC-SVG-010 through TC-SVG-012 (3 cases: malformed SVG, unsupported filter, missing asset)
- [ ] **ST-04**: Execute edge case test cases — TC-SVG-013 through TC-SVG-017 (5 cases: navigation re-render, large SVG, background/foreground, degraded network)
- [ ] **ST-05**: Execute iOS platform test cases — TC-SVG-018 through TC-SVG-020 (3 cases: iPad landscape, orientation change, background state)
- [ ] **ST-06**: Execute accessibility test cases — TC-SVG-021 through TC-SVG-022 (2 cases: VoiceOver label, Dynamic Type layout)
- [ ] **ST-07**: Execute regression test cases — TC-SVG-023 through TC-SVG-025 (3 cases: non-SVG controls, page navigation, device connection)
- [ ] **ST-08**: Execute smoke suite — SMOKE-SVG-001 through SMOKE-SVG-005 (~9 min); file P1 defect immediately if any smoke check fails
- [ ] **ST-09**: Log and triage all defects; confirm P1/P2 defects are resolved or risk-accepted before QE sign-off

---

## Notes

- **Design file dependency**: `qe-svg-rendering-test-design.qsys` must include dedicated pages for: multi-element SVG, SVG controls, SVG with layers, malformed SVG, unsupported filter, missing asset, and a large (≥ 500 KB) SVG element.
- **Core scripting access**: A Q-SYS scripting console or dedicated Named Control surface is required to inject rapid property value changes for TC-SVG-003 through TC-SVG-005 and TC-SVG-016.
- **Smoke-first rule**: Always run the full 5-check smoke suite (SMOKE-SVG-001 through SMOKE-SVG-005) before beginning detailed case execution. A smoke failure indicates a blocking regression — halt and file immediately.
- **P1 count**: 9 P1 cases. All must pass before QE sign-off. Any open P1 at sign-off is a release blocker.
