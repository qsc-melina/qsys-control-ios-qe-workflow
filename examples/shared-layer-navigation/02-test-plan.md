# Test Plan: Shared Layer Navigation

**Feature**: Shared Layer Navigation  
**Platform**: Q-SYS iOS Viewer  
**Version**: Q-SYS Core 9.10+ / iOS Viewer (current)  
**Author**: QE Team  
**Date**: 2026-05-28  
**Status**: Draft  

---

## 1. Objective

- Verify that shared layers render consistently on every page to which they are assigned across iPad and iPhone.
- Confirm that shared layer controls maintain their current Core state values when navigating between pages.
- Validate that page transitions involving shared layers are visually smooth — no flickering, duplication, or disappearance.
- Ensure that dynamic property updates within shared layer controls reflect Q-SYS Core data changes in real time regardless of which page is currently active.
- Confirm that tappable controls within shared layers trigger the correct Core Named Control changes from any assigned page.
- Verify that VoiceOver can navigate into shared layer controls from any page without losing focus.

---

## 2. Scope

### In Scope

- Rendering of shared layers on all pages to which they are assigned in the UCI design
- State consistency of shared layer controls across page navigation
- Visual transitions (no flickering, duplication, or stale content) when navigating to and from pages with shared layers
- Dynamic property updates within shared layer controls driven by Q-SYS Core Named Controls
- Shared layer behavior during orientation changes
- Shared layer behavior on app lifecycle transitions (background / foreground)
- Tappable control interaction within shared layers
- VoiceOver accessibility of shared layer controls from any page
- iOS version compatibility: iOS 16 and later
- iPad (landscape + portrait) and iPhone (portrait)
- Regression against standard page navigation, popup visibility, and existing UCI control interaction

### Out of Scope

- Shared layers in Q-SYS Designer preview
- Animation-driven layers (SMIL / CSS animations not in scope)
- Audio and video layers
- Q-SYS Designer and Q-SYS Core internal layer assignment logic (unless it is the direct integration point)
- Performance benchmarking beyond acceptable-time checks
- Android and web platform equivalents

---

## 3. Test Approach

Testing for this feature is **manual** and executed on physical iOS devices.

| Approach | Description |
|---|---|
| **Functional testing** | Execute step-by-step test cases covering all in-scope scenarios |
| **Exploratory testing** | Unscripted sessions to uncover flickering, z-order, and state transition edge cases |
| **Regression testing** | Re-execute previously passing cases to confirm no regressions in page navigation and UCI rendering |
| **Accessibility testing** | Manual VoiceOver and Dynamic Type evaluation on shared layer controls |

---

## 4. Test Environment

| Item | Value |
|---|---|
| **Platform** | iOS Viewer app on physical device |
| **iOS Versions** | iOS 18.x (latest) + iOS 16.x (minimum supported) |
| **Device Models** | iPad Pro 12.9" (6th gen), iPhone 15 Pro |
| **Q-SYS Core** | Core 9.10+ on Core 110f hardware |
| **Network** | Same subnet, mDNS enabled, 802.11ac Wi-Fi |
| **Q-SYS Design File** | `qe-shared-layer-nav-test-design.qsys` |
| **App Build** | TBD |

> **Design requirements**: The test design must include a shared layer assigned to at least 3 pages, at least one page that has both a shared layer and a popup, and a page with more than 20 controls within the shared layer.

---

## 5. Test Scenarios

> _High-level scenarios only. Detailed steps belong in `03-test-cases.md`._

### Functional

| # | Scenario |
|---|---|
| F-01 | Shared layer renders correctly on the first page at connection on iPad |
| F-02 | Shared layer renders correctly on the first page at connection on iPhone |
| F-03 | Shared layer persists and renders correctly when navigating to a second assigned page |
| F-04 | Shared layer persists and renders correctly when navigating back to the first page |
| F-05 | Shared layer control state (e.g., active toggle) is maintained after navigating away and returning |
| F-06 | Shared layer button control triggers the correct Core Named Control when tapped from any page |
| F-07 | Dynamic property in a shared layer control updates from Core while on any assigned page |
| F-08 | Shared layer containing multiple controls renders and all controls interact correctly |

### Negative

| # | Scenario |
|---|---|
| N-01 | Page with a shared layer and no incoming Core data loads and shows graceful placeholder state |
| N-02 | A layer not assigned to a page does not appear on that page |
| N-03 | A shared layer with conflicting z-order resolves without UI corruption or touch interception |

### Edge Cases

| # | Scenario |
|---|---|
| E-01 | Rapid navigation through 5+ pages in quick succession does not cause shared layer duplication or flickering |
| E-02 | Shared layer displays correct state after the app returns from the background mid-navigation |
| E-03 | Shared layer renders correctly after a disconnect and reconnect to Core |
| E-04 | Shared layer containing more than 20 controls renders within an acceptable time frame |
| E-05 | Navigation to a page with both a shared layer and an open popup behaves correctly with no z-order issues |

### iOS Platform

| # | Scenario |
|---|---|
| I-01 | Shared layer renders correctly on iPad in landscape orientation |
| I-02 | Orientation change from landscape to portrait preserves shared layer layout and state |
| I-03 | App backgrounded mid-navigation returns with shared layer state intact |

### Accessibility

| # | Scenario |
|---|---|
| A-01 | VoiceOver can navigate to controls within the shared layer on any assigned page |
| A-02 | Shared layer controls have correct accessibility labels and roles announced by VoiceOver |
| A-03 | VoiceOver focus is not lost when navigating between pages that share a layer |

### Regression

| # | Scenario |
|---|---|
| R-01 | Page navigation on pages without shared layers functions correctly and is unaffected |
| R-02 | Popup visibility and behavior on pages with shared layers is unaffected |
| R-03 | Existing UCI control interaction (sliders, buttons, text) on page-specific layers is unaffected |

---

## 6. Pass / Fail Criteria

- **Pass**: All functional scenarios pass on both iPad and iPhone; no P1 defects open at release; no shared layer flickering, duplication, or state loss is observed during navigation.
- **Fail**: Any P1 functional scenario fails; any visual corruption or crash is observed; shared layer content is duplicated, missing, or blocking user interaction with page controls.

---

## 7. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Shared layer state synchronization lag during rapid page navigation | High | High | Include dedicated rapid-navigation test case; define clear pass threshold (no duplication within 5 navigations) |
| Flickering or visual artifact during shared layer transition on older iOS 16 devices | Medium | High | Test on iOS 16.x device as well as latest; capture video evidence during execution |
| Popup z-order conflict with shared layer background elements | Medium | Medium | Include dedicated popup + shared layer test case; review layer z-index assignment in test design |
| VoiceOver focus loss when page navigation occurs while VoiceOver is active | Medium | Medium | Test VoiceOver navigation explicitly on both iPad and iPhone; include edge case of navigating while VoiceOver is mid-announcement |
| Shared layer with many controls causing memory pressure on iOS 16 minimum | Low | Medium | Use 20+ control shared layer page in test design; monitor Instruments during execution |
