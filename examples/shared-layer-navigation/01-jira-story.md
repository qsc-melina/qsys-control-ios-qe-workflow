# Jira Story: Shared Layer Navigation — Test Execution

---

## Metadata

| Field | Value |
|---|---|
| **Summary** | [QE] Shared Layer Navigation — Test Execution |
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

This story covers manual QE execution for the Shared Layer Navigation feature in the Q-SYS iOS Viewer. Shared layers are UCI design layers that persist visually across multiple pages — typically used for headers, footers, and persistent background elements. Validation must confirm that shared layers render consistently on all assigned pages, that their control state is maintained across navigation, that transitions between pages are visually smooth, that dynamic Core property updates reach shared layer controls regardless of which page is active, and that VoiceOver can navigate into shared layer controls from any page.

### Testing Scope

- Rendering of shared layers on all pages to which they are assigned
- State consistency of shared layer controls when navigating between pages
- Visual transitions (no flickering, duplication, or stale content) during page navigation
- Dynamic property updates within shared layer controls driven by Q-SYS Core Named Controls
- Shared layer behavior on orientation change and app lifecycle transitions (background/foreground)
- Tappable control interaction within shared layers
- VoiceOver accessibility of controls within shared layers across all pages
- Regression coverage for standard page navigation, popup visibility, and existing UCI control interaction

### Out of Scope

- Shared layers in Q-SYS Designer preview
- Animation-driven layers (SMIL / CSS animations not in scope)
- Audio and video layers
- Android and web platform equivalents

### Test Environment

| Item | Value |
|---|---|
| Platform | iOS Viewer (iPad + iPhone) |
| iOS Version | iOS 18.x (latest) + iOS 16.x (minimum) |
| Q-SYS Core Version | Core 9.10+ |
| Network | Same subnet, mDNS enabled, 802.11ac Wi-Fi |
| Test Device(s) | iPad Pro 12.9" (6th gen), iPhone 15 Pro |
| Q-SYS Design File | `qe-shared-layer-nav-test-design.qsys` |

### References

- Test Plan: `examples/shared-layer-navigation/02-test-plan.md`
- Test Cases: `examples/shared-layer-navigation/03-test-cases.md`
- Regression Scope: `examples/shared-layer-navigation/04-regression-scope.md`
- Feature Spec: `<link TBD>`
- Parent Epic: `<link TBD>`

---

## Acceptance Criteria

**AC-01**: Given the iOS Viewer is connected to a Core with a design containing shared layers, when any assigned page loads, then the shared layer is fully rendered with correct control states and positions on both iPad and iPhone.

**AC-02**: Given a shared layer is visible on Page A, when the tester navigates to Page B (which also has the shared layer), then the shared layer is visible on Page B with no flickering, duplication, or disappearance during the transition.

**AC-03**: Given a shared layer control has a specific state (e.g., a toggle is active), when the tester navigates away from the page and returns, then the shared layer control displays the current Core value — not a stale or default value.

**AC-04**: Given a shared layer contains a Named Control binding, when the Q-SYS Core updates the Named Control value while the tester is on any page, then the shared layer control updates within 500 ms regardless of which page is currently displayed.

**AC-05**: Given a shared layer contains a tappable button control, when the tester taps the button on any page where the shared layer is present, then the corresponding Core Named Control registers the expected value change.

**AC-06**: Given VoiceOver is enabled, when the tester navigates between pages with a shared layer, then VoiceOver focus is not lost and the tester can navigate into shared layer controls from any page using standard swipe gestures.

**AC-07**: Given a design page contains both a shared layer and page-specific popup controls, when the tester opens the popup, then the popup is displayed correctly above the shared layer with no z-order corruption or shared layer bleed-through.

---

## Sub-Tasks

- [ ] **ST-01**: Set up test environment — load `qe-shared-layer-nav-test-design.qsys` on Core 9.10+; verify shared layer is assigned to ≥ 3 pages; confirm test devices (iPad Pro 12.9", iPhone 15 Pro) are connected
- [ ] **ST-02**: Execute functional test cases — TC-SLN-001 through TC-SLN-008 (8 cases: initial render, persistence across navigation, state maintenance, control interaction, dynamic updates)
- [ ] **ST-03**: Execute negative test cases — TC-SLN-009 through TC-SLN-011 (3 cases: no Core data, unassigned layer, z-order conflict)
- [ ] **ST-04**: Execute edge case test cases — TC-SLN-012 through TC-SLN-016 (5 cases: fast navigation, background/foreground, reconnect, large layer, popup interaction)
- [ ] **ST-05**: Execute iOS platform test cases — TC-SLN-017 through TC-SLN-019 (3 cases: iPad landscape, orientation change, background mid-navigation)
- [ ] **ST-06**: Execute accessibility test cases — TC-SLN-020 through TC-SLN-022 (3 cases: VoiceOver navigation, label correctness, focus on page change)
- [ ] **ST-07**: Execute regression test cases — TC-SLN-023 through TC-SLN-025 (3 cases: standard navigation, popup visibility, UCI control interaction)
- [ ] **ST-08**: Execute smoke suite — SMOKE-SLN-001 through SMOKE-SLN-005 (~9 min); file P1 defect immediately if any smoke check fails
- [ ] **ST-09**: Log and triage all defects; confirm P1/P2 defects are resolved or risk-accepted before QE sign-off

---

## Notes

- **Design file dependency**: `qe-shared-layer-nav-test-design.qsys` must include a shared layer assigned to at least 3 pages, at least one page with both a shared layer and a popup, and a page with > 20 controls in the shared layer.
- **Rapid navigation test**: TC-SLN-012 requires fast page navigation (5+ pages in under 5 seconds). Use a dedicated page navigation bar control in the design to facilitate this.
- **Core scripting access**: A Q-SYS scripting console or Named Control surface is required to inject value changes into shared layer controls for TC-SLN-007 and TC-SLN-004.
- **Smoke-first rule**: Always run the full 5-check smoke suite (SMOKE-SLN-001 through SMOKE-SLN-005) before beginning detailed case execution.
- **P1 count**: 9 P1 cases. All must pass before QE sign-off. Any open P1 at sign-off is a release blocker.
