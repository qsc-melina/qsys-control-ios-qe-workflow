# Test Cases: Shared Layer Navigation

**Feature**: Shared Layer Navigation  
**Feature Code**: SLN  
**Platform**: Q-SYS iOS Viewer  
**Date**: 2026-05-28  
**Author**: QE Team  
**Test Plan**: [02-test-plan.md](02-test-plan.md)  

---

## Summary

| ID | Title | Priority | Category | Status |
|---|---|---|---|---|
| TC-SLN-001 | Shared layer renders on first page load — iPad | P1 | Functional | ⬜ Not Run |
| TC-SLN-002 | Shared layer renders on first page load — iPhone | P1 | Functional | ⬜ Not Run |
| TC-SLN-003 | Shared layer persists when navigating to second assigned page | P1 | Functional | ⬜ Not Run |
| TC-SLN-004 | Shared layer persists when navigating back to first page | P1 | Functional | ⬜ Not Run |
| TC-SLN-005 | Shared layer control state maintained across navigation | P1 | Functional | ⬜ Not Run |
| TC-SLN-006 | Shared layer button triggers Core Named Control on tap | P1 | Functional | ⬜ Not Run |
| TC-SLN-007 | Dynamic property in shared layer updates from Core on any page | P2 | Functional | ⬜ Not Run |
| TC-SLN-008 | Shared layer with multiple controls renders and interacts correctly | P2 | Functional | ⬜ Not Run |
| TC-SLN-009 | Page with shared layer and no Core data shows graceful placeholder | P1 | Negative | ⬜ Not Run |
| TC-SLN-010 | Layer not assigned to a page does not appear on that page | P2 | Negative | ⬜ Not Run |
| TC-SLN-011 | Conflicting z-order resolves without UI corruption | P2 | Negative | ⬜ Not Run |
| TC-SLN-012 | Rapid navigation does not cause duplication or flickering | P1 | Edge Case | ⬜ Not Run |
| TC-SLN-013 | Shared layer state correct after app returns from background | P2 | Edge Case | ⬜ Not Run |
| TC-SLN-014 | Shared layer renders correctly after reconnect | P2 | Edge Case | ⬜ Not Run |
| TC-SLN-015 | Shared layer with many controls renders within acceptable time | P2 | Edge Case | ⬜ Not Run |
| TC-SLN-016 | Navigation to page with shared layer and open popup behaves correctly | P2 | Edge Case | ⬜ Not Run |
| TC-SLN-017 | Shared layer renders correctly — iPad landscape | P1 | iOS Platform | ⬜ Not Run |
| TC-SLN-018 | Orientation change preserves shared layer layout and state | P2 | iOS Platform | ⬜ Not Run |
| TC-SLN-019 | App backgrounded mid-navigation returns with correct shared layer state | P2 | iOS Platform | ⬜ Not Run |
| TC-SLN-020 | VoiceOver navigates to controls within shared layer | P2 | Accessibility | ⬜ Not Run |
| TC-SLN-021 | Shared layer controls have correct accessibility labels and roles | P2 | Accessibility | ⬜ Not Run |
| TC-SLN-022 | VoiceOver focus not lost during page navigation | P2 | Accessibility | ⬜ Not Run |
| TC-SLN-023 | Page navigation without shared layers functions correctly | P1 | Regression | ⬜ Not Run |
| TC-SLN-024 | Popup visibility on pages with shared layers is unaffected | P1 | Regression | ⬜ Not Run |
| TC-SLN-025 | Existing UCI control interaction on page-specific layers is unaffected | P1 | Regression | ⬜ Not Run |

**Totals**: 25 cases — P1: 9 · P2: 16

---

## Functional

---

### TC-SLN-001: Shared layer renders on first page load — iPad

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" running iOS 18.x, connected to same subnet as Q-SYS Core
- [ ] `qe-shared-layer-nav-test-design.qsys` loaded in Core 9.10+
- [ ] The design has a shared layer ("Header Layer") assigned to at least 3 pages
- [ ] iOS Viewer app is installed and not currently connected

**Steps**:
1. Launch the iOS Viewer app on iPad.
2. Tap the saved device entry for the test Core or enter the Core IP manually.
3. Tap **Connect**.
4. Observe the first assigned page as it loads.
5. Visually inspect the shared "Header Layer" — confirm its controls (e.g., status indicator, label) are visible and correctly positioned.

**Expected Result**:
> The shared layer is fully rendered on the first page with all controls visible, correctly positioned, and reflecting the current Core values. The page is interactive within 3 seconds of connection.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Baseline render check. Screenshot the shared layer on load for comparison against later navigation cases.

---

### TC-SLN-002: Shared layer renders on first page load — iPhone

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPhone 15 Pro running iOS 18.x, connected to same subnet as Q-SYS Core
- [ ] `qe-shared-layer-nav-test-design.qsys` loaded in Core 9.10+
- [ ] Shared layer ("Header Layer") is assigned to at least 3 pages

**Steps**:
1. Launch the iOS Viewer app on iPhone.
2. Connect to the test Core.
3. Observe the first assigned page in portrait orientation.
4. Visually inspect the shared layer controls.

**Expected Result**:
> The shared layer is fully rendered on the smaller iPhone form factor with all controls visible and correctly proportioned. No overflow, clipping, or missing elements.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Compare layout with TC-SLN-001 iPad result to confirm consistent rendering across form factors.

---

### TC-SLN-003: Shared layer persists when navigating to second assigned page

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] First page ("Page-A") with shared layer is displayed

**Steps**:
1. Note the appearance and control states in the shared "Header Layer" on Page-A.
2. Tap the navigation control to move to "Page-B" (also assigned to the shared layer).
3. Observe the shared layer on Page-B.

**Expected Result**:
> The shared layer appears on Page-B in the same position and with the same control states as on Page-A. No flickering, disappearing, or blank-then-reappear transition is observed. The transition is immediate and smooth.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-004: Shared layer persists when navigating back to first page

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core, "Page-B" is currently displayed with shared layer visible

**Steps**:
1. Note the shared layer control states on Page-B.
2. Navigate back to Page-A.
3. Observe the shared layer on Page-A.

**Expected Result**:
> The shared layer is visible on Page-A with the same control states as observed on Page-B (reflecting current Core values). No stale values, missing elements, or visual artifacts appear on return navigation.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-005: Shared layer control state maintained across navigation

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Shared layer contains a toggle button whose state is bound to Core Named Control "SLN.Toggle1"
- [ ] "SLN.Toggle1" is currently set to active (ON) state via Core scripting

**Steps**:
1. Observe the shared layer toggle button on Page-A — confirm it shows the active (ON) state.
2. Navigate to Page-B.
3. Observe the toggle button state on Page-B.
4. Navigate to Page-C.
5. Observe the toggle button state on Page-C.

**Expected Result**:
> The toggle button displays the active (ON) state on Page-B and Page-C, identical to the state observed on Page-A. The state is not reset to a default or off state on navigation. The current Core value is always reflected.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-006: Shared layer button triggers Core Named Control on tap

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Shared layer contains a button bound to Core Named Control "SLN.Action1"
- [ ] A visible indicator on the page reflects the state of "SLN.Action1" (e.g., status light)
- [ ] Test is performed from Page-A, Page-B, and Page-C

**Steps**:
1. Navigate to Page-A. Observe the status light (off state).
2. Tap the shared layer button on Page-A.
3. Observe the status light.
4. Navigate to Page-B and tap the shared layer button again.
5. Observe the status light.
6. Navigate to Page-C and tap the shared layer button.
7. Observe the status light.

**Expected Result**:
> Each tap on the shared layer button triggers the Core Named Control "SLN.Action1" from any page. The status light toggles correctly after each tap. No page change is required to activate the button — it is always interactive.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-007: Dynamic property in shared layer updates from Core on any page

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Shared layer contains a text label bound to Core Named Control "SLN.StatusText"
- [ ] Tester has access to Core scripting to change "SLN.StatusText" value remotely

**Steps**:
1. Navigate to Page-A. Note the current value of the shared layer text label.
2. Navigate to Page-B.
3. Change the value of "SLN.StatusText" to "ACTIVE" via Core scripting while on Page-B.
4. Observe the shared layer text label on Page-B.
5. Navigate to Page-C.
6. Observe the shared layer text label on Page-C.

**Expected Result**:
> The shared layer text label updates to "ACTIVE" within 500 ms of the Core change on Page-B, and shows "ACTIVE" on Page-C without requiring a page refresh. The value is always current regardless of which page is active.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-008: Shared layer with multiple controls renders and interacts correctly

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] A shared layer page ("Header-MultiControl") contains at least 5 controls: button, toggle, text label, status indicator, and slider

**Steps**:
1. Navigate to a page with the multi-control shared layer.
2. Observe all 5 controls — confirm all are visible and positioned correctly.
3. Tap the button.
4. Toggle the toggle.
5. Move the slider from minimum to maximum.
6. Observe the text label and status indicator update accordingly.

**Expected Result**:
> All 5 shared layer controls are visible and interactive. Each control responds to interaction and sends the correct value change to Core. No control overlaps another, and no touch event is swallowed by an adjacent control.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Negative

---

### TC-SLN-009: Page with shared layer and no Core data shows graceful placeholder

**Priority**: P1  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Core scripting is used to disconnect the Named Control subscriptions for the shared layer controls (simulating no incoming data)

**Steps**:
1. Disconnect Core Named Control bindings for the shared layer via scripting (or use a design page where shared layer controls have no Core binding).
2. Navigate to a page with the shared layer.
3. Observe shared layer controls.
4. Interact with any non-shared-layer controls on the page.

**Expected Result**:
> Shared layer controls with no incoming Core data display their default or placeholder state (empty string, default value, or neutral indicator). The app does not crash. Non-shared-layer controls on the page remain fully functional.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-010: Layer not assigned to a page does not appear on that page

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Design contains "Page-Unassigned" — a page intentionally not assigned to any shared layer

**Steps**:
1. Navigate to "Page-Unassigned".
2. Observe the page for any unexpected shared layer content.

**Expected Result**:
> No shared layer elements appear on "Page-Unassigned". The page shows only its own page-specific content. No ghost, faded, or leftover shared layer elements from previously visited pages are visible.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-011: Conflicting z-order resolves without UI corruption

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Design page "Page-ZConflict" has a shared layer background element at a high z-index value that overlaps with page-specific interactive controls

**Steps**:
1. Navigate to "Page-ZConflict".
2. Observe whether page-specific controls are visible above the shared layer.
3. Tap each page-specific control.

**Expected Result**:
> The app resolves the z-order conflict by applying the expected layer ordering defined in the design. Page-specific interactive controls are reachable and respond to taps. No UI corruption (blank areas, controls painting over each other incorrectly) is visible.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Edge Case

---

### TC-SLN-012: Rapid navigation does not cause duplication or flickering

**Priority**: P1  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Design has at least 5 pages all assigned to the shared layer
- [ ] A page navigation bar control in the shared layer or page provides quick access to all 5 pages

**Steps**:
1. Navigate to Page-A.
2. Rapidly tap through Page-B → Page-C → Page-D → Page-E → Page-A in quick succession (all taps within 3 seconds).
3. Observe the shared layer during and after navigation.
4. Repeat the sequence once more.

**Expected Result**:
> The shared layer does not duplicate, flicker, or disappear during or after rapid navigation. After settling on any page, the shared layer is rendered exactly once in the correct position with the correct control states. The app does not crash or freeze.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  
**Notes**: This is the highest-risk scenario for shared layer. Video capture strongly recommended.

---

### TC-SLN-013: Shared layer state correct after app returns from background

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Page-B with shared layer is displayed
- [ ] Core Named Control "SLN.StatusText" is set to "STANDBY"

**Steps**:
1. Note the shared layer text label value ("STANDBY") on Page-B.
2. Press the **Home** button to send the app to the background.
3. Use Core scripting to change "SLN.StatusText" to "RUNNING".
4. Return to the iOS Viewer app.
5. Observe the shared layer text label.

**Expected Result**:
> After returning from the background, the shared layer text label shows "RUNNING" (the current Core value), not the stale "STANDBY" value from before backgrounding. The update resolves within 2 seconds of foreground restoration.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-014: Shared layer renders correctly after reconnect

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Page-B with shared layer is displayed

**Steps**:
1. Note the appearance of the shared layer on Page-B.
2. Tap **Disconnect** (or navigate to the device list and disconnect).
3. Confirm the app returns to the device list.
4. Reconnect to the same Core.
5. Navigate to Page-B.
6. Observe the shared layer.

**Expected Result**:
> After reconnecting, the shared layer renders correctly on Page-B with all controls visible and reflecting current Core values. The reconnect does not leave any ghost elements or stale state from the previous session.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-015: Shared layer with many controls renders within acceptable time

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Design page "Page-LargeLayer" has a shared layer containing more than 20 controls

**Steps**:
1. Navigate to "Page-LargeLayer".
2. Measure the time from tap to full layer render (stopwatch or direct observation).
3. Interact with 3 controls in the large shared layer.

**Expected Result**:
> The large shared layer renders fully within 3 seconds. All 20+ controls are visible and correctly positioned. Interactive controls respond normally without delay after render completes.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-016: Navigation to page with shared layer and open popup behaves correctly

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Design contains "Page-PopupLayer" which has both a shared layer and a popup control
- [ ] "Page-PopupLayer" is currently displayed

**Steps**:
1. Navigate to "Page-PopupLayer". Confirm shared layer is visible.
2. Open the popup on this page by tapping the popup trigger control.
3. Observe whether the popup appears above the shared layer.
4. Close the popup.
5. Confirm the shared layer is still visible and correct after popup close.

**Expected Result**:
> The popup opens above the shared layer with no z-order corruption. The shared layer is not obscured by the popup in unexpected areas. After closing the popup, the shared layer remains intact and all its controls are still accessible.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## iOS Platform

---

### TC-SLN-017: Shared layer renders correctly — iPad landscape

**Priority**: P1  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" running iOS 18.x, connected to Core, Page-A with shared layer displayed
- [ ] Device is in landscape orientation

**Steps**:
1. Confirm iPad is in landscape orientation.
2. Observe the shared layer on Page-A.
3. Navigate to Page-B and observe the shared layer.
4. Navigate to Page-C and observe the shared layer.

**Expected Result**:
> The shared layer is correctly rendered in the iPad landscape viewport across all three pages. No controls overflow, clip beyond the safe area, or overlap with page-specific content. The layer position is consistent across all pages.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-018: Orientation change preserves shared layer layout and state

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" running iOS 18.x, connected to Core, Page-B with shared layer displayed in landscape orientation
- [ ] Shared layer toggle is in the active (ON) state

**Steps**:
1. Note the shared layer layout and toggle state in landscape orientation.
2. Rotate the iPad to portrait orientation.
3. Observe the shared layer layout and toggle state.
4. Rotate back to landscape.
5. Observe again.

**Expected Result**:
> The shared layer re-lays out correctly for portrait orientation while preserving the toggle state (ON). All controls remain visible and accessible in both orientations. No control disappears or resets state during orientation change.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-019: App backgrounded mid-navigation returns with correct shared layer state

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core, navigating between Page-A and Page-B with shared layer

**Steps**:
1. Tap the navigation control to move from Page-A to Page-B.
2. Immediately press the **Home** button to background the app during the transition.
3. Wait 5 seconds.
4. Return to the iOS Viewer app.
5. Observe which page is displayed and the shared layer state.

**Expected Result**:
> The app returns to a fully rendered page (either Page-A or Page-B depending on transition completion). The shared layer is correctly rendered on that page with current Core values. No partial render, blank area, or frozen transition state is visible.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Accessibility

---

### TC-SLN-020: VoiceOver navigates to controls within shared layer

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Page-A with shared layer is displayed
- [ ] VoiceOver is enabled (Settings > Accessibility > VoiceOver > On)

**Steps**:
1. With VoiceOver active, navigate to Page-A.
2. Swipe right with one finger to move VoiceOver focus through all elements.
3. Continue swiping until VoiceOver focus reaches the shared layer button control.
4. Listen to the VoiceOver announcement.
5. Double-tap to activate the button.

**Expected Result**:
> VoiceOver traverses into the shared layer and announces the button with its correct label and "Button" trait. The button is reachable via standard swipe navigation and can be activated via double-tap. VoiceOver does not skip the shared layer entirely.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-021: Shared layer controls have correct accessibility labels and roles

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Page-A with shared layer is displayed
- [ ] VoiceOver is enabled

**Steps**:
1. With VoiceOver active, swipe through all shared layer controls on Page-A.
2. For each control, note the announced label and role (Button, Toggle, Slider, etc.).
3. Navigate to Page-B.
4. Repeat the traversal on Page-B and compare announcements.

**Expected Result**:
> Each shared layer control announces its correct accessibility label and the appropriate role (Button, Toggle, Adjustable for sliders, etc.) on both pages. The announcements are identical on Page-A and Page-B — the label does not change or disappear on navigation.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-022: VoiceOver focus not lost during page navigation

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] VoiceOver is enabled
- [ ] VoiceOver focus is on a shared layer control on Page-A

**Steps**:
1. Position VoiceOver focus on the shared layer button on Page-A.
2. Use a page-specific navigation control (with a different finger or using the rotor) to navigate to Page-B.
3. Observe whether VoiceOver announces the page change.
4. Confirm VoiceOver focus lands on a known element.

**Expected Result**:
> VoiceOver announces the page transition (or the first element of Page-B). VoiceOver focus does not become lost or silent. The tester can continue navigating via swipe without needing to tap the screen to re-establish focus.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Regression

---

### TC-SLN-023: Page navigation without shared layers functions correctly

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Design contains "Page-NoLayer-1" and "Page-NoLayer-2" — pages with no shared layer

**Steps**:
1. Navigate to "Page-NoLayer-1". Confirm the page loads correctly and controls are interactive.
2. Navigate to "Page-NoLayer-2". Confirm the page loads correctly.
3. Navigate back to "Page-NoLayer-1".
4. Navigate to a shared-layer page, then back to "Page-NoLayer-1".

**Expected Result**:
> Navigation to and from pages without shared layers functions correctly in all cases. Pages load in under 3 seconds. No controls are missing or unresponsive. The presence of shared layers on other pages does not affect navigation performance or rendering for non-shared-layer pages.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-024: Popup visibility on pages with shared layers is unaffected

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] "Page-PopupLayer" has both a shared layer and a popup control

**Steps**:
1. Navigate to "Page-PopupLayer".
2. Open the popup by tapping its trigger.
3. Confirm the popup opens and is fully visible.
4. Interact with at least one control inside the popup.
5. Close the popup.
6. Confirm the popup closes and the shared layer and page-specific controls are intact.

**Expected Result**:
> The popup opens and closes correctly. Popup controls respond to interaction. The popup is displayed above the shared layer without any z-order issues. After closing, the page state (including shared layer) is identical to before the popup was opened.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SLN-025: Existing UCI control interaction on page-specific layers is unaffected

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-shared-layer-nav-test-design.qsys` active
- [ ] Page-A has a shared layer plus page-specific UCI controls (slider, toggle button, text input)

**Steps**:
1. Navigate to Page-A.
2. Drag the page-specific slider from minimum to maximum.
3. Tap the page-specific toggle button to change its state.
4. Confirm the text label on Page-A updates to reflect the toggle state.
5. Check Core scripting console to confirm Named Control values updated correctly.

**Expected Result**:
> All page-specific UCI controls (slider, toggle, text label) behave exactly as expected — identical to their behavior on a page without a shared layer. The shared layer does not intercept touch events, override Named Control bindings, or alter the layout of page-specific controls.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Execution Log

| Date | Build | Executed By | iPad Result | iPhone Result | Notes |
|---|---|---|---|---|---|
| | | | | | |
