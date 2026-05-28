# Test Cases: Popup Visibility and Layer Transition Behavior

**Feature**: Popup Visibility and Layer Transition Behavior  
**Feature Code**: PVL  
**Platform**: Q-SYS iOS Viewer  
**Test Plan**: [test-plan.md](./test-plan.md)  
**Author**: QE Team  
**Date**: 2026-05-20  
**Build**: TBD  

---

## Summary Table

| ID | Title | Priority | Category | Status |
|----|-------|----------|----------|--------|
| TC-PVL-001 | Popup opens with fade-in transition (iPad, landscape) | P1 | Functional | ⬜ Not Run |
| TC-PVL-002 | Popup opens with fade-in transition (iPhone, portrait) | P1 | Functional | ⬜ Not Run |
| TC-PVL-003 | Popup closes with fade-out via close control | P1 | Functional | ⬜ Not Run |
| TC-PVL-004 | Popup closes via tap-outside dismissal | P2 | Functional | ⬜ Not Run |
| TC-PVL-005 | Shared layer remains visible while popup is open | P1 | Functional | ⬜ Not Run |
| TC-PVL-006 | Shared layer visibility restored after popup closes | P1 | Functional | ⬜ Not Run |
| TC-PVL-007 | Second popup opens correctly over a first open popup | P2 | Functional | ⬜ Not Run |
| TC-PVL-008 | Layer visibility state persists after navigating away and back | P2 | Functional | ⬜ Not Run |
| TC-PVL-009 | Open trigger on already-open popup — no duplicate popup | P2 | Negative | ⬜ Not Run |
| TC-PVL-010 | Close trigger on already-closed popup — no crash or error | P2 | Negative | ⬜ Not Run |
| TC-PVL-011 | Network disconnect mid-transition — handled gracefully | P2 | Negative | ⬜ Not Run |
| TC-PVL-012 | Rapid successive open/close taps — no corruption or hang | P1 | Edge Case | ⬜ Not Run |
| TC-PVL-013 | New popup triggered during previous popup's fade-out | P2 | Edge Case | ⬜ Not Run |
| TC-PVL-014 | App backgrounded mid-fade-transition — correct state on resume | P2 | Edge Case | ⬜ Not Run |
| TC-PVL-015 | Long popup content is scrollable and fully accessible | P2 | Edge Case | ⬜ Not Run |
| TC-PVL-016 | App backgrounded while popup is open — popup state preserved | P2 | iOS Platform | ⬜ Not Run |
| TC-PVL-017 | Popup layout correct in landscape orientation (iPad) | P3 | iOS Platform | ⬜ Not Run |
| TC-PVL-018 | VoiceOver focus moves into popup on open | P2 | Accessibility | ⬜ Not Run |
| TC-PVL-019 | VoiceOver navigates within popup and activates close control | P2 | Accessibility | ⬜ Not Run |
| TC-PVL-020 | Dynamic Type (Accessibility XL) — no truncation in popup | P3 | Accessibility | ⬜ Not Run |
| TC-PVL-021 | Shared layer controls function correctly after popup cycle | P2 | Regression | ⬜ Not Run |
| TC-PVL-022 | Page navigation unaffected after popup open/close cycle | P2 | Regression | ⬜ Not Run |

---

## Functional

---

### TC-PVL-001: Popup opens with fade-in transition (iPad, landscape)

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] Q-SYS iOS Viewer is connected to the test Core and displaying `qe-popup-layer-test-design.qsys`
- [ ] The test design page containing the popup trigger button is visible
- [ ] No popup is currently open
- [ ] iPad Pro 12.9" is in landscape orientation

**Steps**:
1. Locate the **Open Popup** button on the UCI design page.
2. Tap the **Open Popup** button.
3. Observe the popup appearance behavior.

**Expected Result**:
> The popup appears with a smooth fade-in animation (opacity transitioning from 0 to 1). The popup is fully rendered and interactive when the fade-in completes. No visual artifacts, flickering, or instant-appear behavior occurs.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Note approximate fade duration; log if it appears to skip animation entirely.

---

### TC-PVL-002: Popup opens with fade-in transition (iPhone, portrait)

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] Q-SYS iOS Viewer is connected to the test Core and displaying `qe-popup-layer-test-design.qsys`
- [ ] The test design page containing the popup trigger button is visible
- [ ] No popup is currently open
- [ ] iPhone 15 is in portrait orientation

**Steps**:
1. Locate the **Open Popup** button on the UCI design page.
2. Tap the **Open Popup** button.
3. Observe the popup appearance and layout in portrait orientation.

**Expected Result**:
> The popup appears with a fade-in animation. The popup is fully contained within the screen bounds in portrait orientation — no content is clipped or positioned off-screen. The popup is interactive when the animation completes.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-003: Popup closes with fade-out via close control

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] TC-PVL-001 passed — a popup is currently open and fully visible

**Steps**:
1. Locate the **Close** button (or dismiss control) within the open popup.
2. Tap the **Close** button.
3. Observe the popup dismissal behavior.

**Expected Result**:
> The popup disappears with a smooth fade-out animation (opacity transitioning from 1 to 0). After the animation completes, the popup is fully removed from the view hierarchy. The underlying UCI design page is fully visible and interactive. No ghost elements or residual overlay remain.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-004: Popup closes via tap-outside dismissal

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design is configured to allow dismiss-on-outside-tap for this popup
- [ ] A popup is currently open and fully visible

**Steps**:
1. Tap a visible area of the UCI design page that is outside the popup boundary.
2. Observe the popup dismissal behavior.

**Expected Result**:
> The popup dismisses with a fade-out animation, identical to using the close control. The tapped area outside the popup does not trigger any unintended control actions.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Skip and mark ⏭ if the test design does not support dismiss-on-outside-tap; log the skip reason.

---

### TC-PVL-005: Shared layer remains visible while popup is open

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design page displays a shared layer containing at least one labelled control (e.g., a volume slider or status label)
- [ ] No popup is currently open

**Steps**:
1. Confirm the shared layer and its controls are visible on the UCI design page.
2. Tap the **Open Popup** button to open the popup.
3. While the popup is fully open, observe the area of the screen where the shared layer controls are located.

**Expected Result**:
> The shared layer remains visible beneath or around the popup. The shared layer controls are rendered and their labels are legible. The popup does not occlude the shared layer unless the popup bounds explicitly overlap that area.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-006: Shared layer visibility restored after popup closes

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] TC-PVL-005 passed — a popup was opened while a shared layer was visible
- [ ] The popup is currently open

**Steps**:
1. Tap the **Close** button to dismiss the popup.
2. Wait for the fade-out animation to complete.
3. Observe the full UCI design page.

**Expected Result**:
> After the popup closes, the shared layer and all its controls are fully visible in their correct positions. No controls are missing, greyed out, or in an incorrect state compared to before the popup was opened.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-007: Second popup opens correctly over a first open popup

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design contains a second popup that can be triggered from within the first popup
- [ ] The first popup is currently open and fully visible

**Steps**:
1. While the first popup is open, tap the button within it that triggers the second popup.
2. Observe the stacking behavior.

**Expected Result**:
> The second popup opens with a fade-in animation on top of the first popup. Both popups are visible simultaneously (or the first is hidden if the design specifies exclusive display). The second popup is interactive and can be independently closed.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: The expected stacking behavior must be confirmed with the dev team and documented before test execution.

---

### TC-PVL-008: Layer visibility state persists after navigating away and back

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design has at least two UCI pages, with the popup/layer feature on Page 1
- [ ] A specific layer visibility state has been set on Page 1 (e.g., a layer is toggled on)

**Steps**:
1. On Page 1, confirm the current layer visibility state.
2. Tap the navigation control to switch to Page 2.
3. Observe Page 2 renders correctly.
4. Tap the navigation control to return to Page 1.
5. Observe the layer visibility state on Page 1.

**Expected Result**:
> The layer visibility state on Page 1 is identical to what it was before navigating away. No layers are reset, hidden, or shown unexpectedly. The page renders without any transition or layer state glitch.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Negative

---

### TC-PVL-009: Open trigger on already-open popup — no duplicate popup

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] A popup is currently open and fully visible (TC-PVL-001 state)

**Steps**:
1. While the popup is already open, tap the **Open Popup** trigger button again.
2. Observe the behavior.

**Expected Result**:
> No second or duplicate popup appears. The existing popup remains visible and unchanged. No crash, error dialog, or visual corruption occurs. The app does not stack an invisible second popup layer.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-010: Close trigger on already-closed popup — no crash or error

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] No popup is currently open
- [ ] A button in the UCI design can send a close-popup signal (e.g., via Q-SYS Core scripting)

**Steps**:
1. Confirm no popup is visible on screen.
2. Tap the control that sends the popup close signal (even though no popup is open).
3. Observe the behavior.

**Expected Result**:
> The app handles the spurious close signal silently. No error dialog, crash, or unexpected UI change occurs. The UCI design page remains in its current state, unaffected.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-011: Network disconnect mid-transition — handled gracefully

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] Q-SYS iOS Viewer is connected to the test Core and the design page is visible

**Steps**:
1. Tap the **Open Popup** button.
2. Immediately after tapping (within 0.5 seconds, during the fade-in), go to **Settings → Wi-Fi** and disable Wi-Fi.
3. Return to Q-SYS iOS Viewer.
4. Observe the app state.

**Expected Result**:
> The app displays a connection lost or disconnected state without crashing. The popup — whether fully open, partially transitioned, or not yet opened — does not leave a broken or unresponsive overlay on screen. The user can recover by re-enabling Wi-Fi.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Timing this test requires a second person or a quick gesture. Use the control center Wi-Fi toggle for speed.

---

## Edge Cases

---

### TC-PVL-012: Rapid successive open/close taps — no corruption or hang

**Priority**: P1  
**Category**: Edge Case  
**Test Type**: Negative  

**Preconditions**:
- [ ] The test design page is visible with the popup trigger button accessible
- [ ] No popup is currently open

**Steps**:
1. Tap the **Open Popup** button rapidly 10 times in quick succession (approximately 1 tap per 200ms).
2. Pause for 3 seconds.
3. Observe the UI state.
4. If a popup is open, tap the **Close** button.
5. Observe whether the popup closes cleanly.

**Expected Result**:
> After the rapid taps, the UI settles into a stable state — either with the popup fully open or fully closed. No duplicate popups, partially-rendered overlays, frozen animations, or unresponsive controls are present. The app does not crash.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Repeat this test 3 times consecutively. Log any variation in behavior across runs.

---

### TC-PVL-013: New popup triggered during previous popup's fade-out

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Negative  

**Preconditions**:
- [ ] A popup is currently open and fully visible

**Steps**:
1. Tap the **Close** button to begin the popup fade-out animation.
2. Immediately (within the first 50% of the fade-out duration) tap the **Open Popup** button again.
3. Observe the resulting transition behavior.

**Expected Result**:
> The app handles the overlap gracefully. Either the in-progress fade-out is cancelled and the popup fades back in, or the fade-out completes and the popup immediately reopens. No visual corruption, frozen frame, or crash results. The final state is a fully visible, interactive popup.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: This test requires precise timing. Make two attempts and record both outcomes.

---

### TC-PVL-014: App backgrounded mid-fade-transition — correct state on resume

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design page is visible

**Steps**:
1. Tap the **Open Popup** button to begin the fade-in transition.
2. Immediately press the **Home button** (or swipe up) to background the app during the transition.
3. Wait 5 seconds.
4. Return to Q-SYS iOS Viewer via the App Switcher.
5. Observe the popup state.

**Expected Result**:
> The popup is in a definite state — either fully open or fully closed — when the app is foregrounded. No mid-transition frame is frozen on screen. If the popup is open, it is fully interactive; if closed, the design page is fully rendered.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Repeat with close transition: tap Close, immediately background, then foreground.

---

### TC-PVL-015: Long popup content is scrollable and fully accessible

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design includes a popup whose content height exceeds the visible screen area
- [ ] The popup is currently open

**Steps**:
1. Open the tall popup from the trigger button.
2. Observe the initial popup state — confirm content is visible and not clipped.
3. Swipe upward within the popup to scroll down.
4. Scroll to the bottom of the popup content.
5. Verify the close control is accessible (scroll back to top if needed, or verify it remains pinned).

**Expected Result**:
> The popup scrolls smoothly within its bounds. Content at the bottom is reachable. The close control remains accessible (either pinned to the top/bottom or reachable by scrolling). No content bleeds outside the popup boundary.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Test on both iPad and iPhone — iPhone's smaller viewport makes clipping more likely.

---

## iOS Platform

---

### TC-PVL-016: App backgrounded while popup is open — popup state preserved

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] A popup is fully open and visible on screen

**Steps**:
1. Press the **Home button** (or swipe up) to background the app while the popup is open.
2. Wait 15 seconds.
3. Return to Q-SYS iOS Viewer via the App Switcher.
4. Observe the UI state.

**Expected Result**:
> The popup is still open and fully rendered when the app is foregrounded. The underlying UCI design page is visible behind the popup. No crash, blank screen, or incorrect state occurs.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-017: Popup layout correct in landscape orientation (iPad)

**Priority**: P3  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" is in portrait orientation
- [ ] No popup is currently open

**Steps**:
1. Tap the **Open Popup** button while the iPad is in portrait orientation.
2. Confirm the popup is fully visible in portrait.
3. Rotate the iPad to landscape orientation while the popup is open.
4. Observe the popup layout in landscape.

**Expected Result**:
> The popup adapts to the landscape layout without visual glitches. All popup content and controls are visible and within screen bounds. No element overlaps the notch or unsafe areas. The fade overlay (if present) covers the full screen.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Also rotate back to portrait and verify re-adaptation.

---

## Accessibility

---

### TC-PVL-018: VoiceOver focus moves into popup on open

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] **Settings → Accessibility → VoiceOver** is enabled on the test device
- [ ] The test design page is visible; VoiceOver focus is on a control outside the popup

**Steps**:
1. With VoiceOver active, double-tap the **Open Popup** trigger button to open the popup.
2. Observe where VoiceOver focus moves immediately after the popup opens.
3. Attempt to swipe left (backward) to see if VoiceOver can reach elements behind the popup.

**Expected Result**:
> VoiceOver focus moves into the popup when it opens — specifically to the first interactive element or the popup's heading. VoiceOver cannot navigate to elements behind the popup (focus is trapped within the popup while it is open). The popup's purpose is announced by VoiceOver.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Focus trapping is critical for accessibility compliance. Log the exact element VoiceOver announces first.

---

### TC-PVL-019: VoiceOver navigates within popup and activates close control

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] VoiceOver is enabled
- [ ] The popup is currently open (from TC-PVL-018 state)

**Steps**:
1. With VoiceOver active and focus inside the popup, swipe right to navigate forward through all popup elements.
2. Verify each element announces a meaningful label (not "Button" or "unlabeled").
3. Navigate to the close control using VoiceOver swipe gestures.
4. Double-tap the close control to dismiss the popup.
5. Observe where VoiceOver focus returns after the popup closes.

**Expected Result**:
> All interactive elements within the popup have descriptive accessibility labels. The close control is reachable via VoiceOver swipe navigation. After dismissal, VoiceOver focus returns to a logical position on the underlying design page (e.g., the open trigger button, or the first element of the page).

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Log the label announced for each element. Flag any element that announces only "Button" with no context.

---

### TC-PVL-020: Dynamic Type (Accessibility XL) — no truncation in popup

**Priority**: P3  
**Category**: Accessibility  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] **Settings → Accessibility → Display & Text Size → Larger Text** is set to **Accessibility XL**
- [ ] The popup is currently open with visible text labels

**Steps**:
1. With Accessibility XL text size active, observe all text within the open popup.
2. Check popup title, body text, button labels, and any status labels for truncation.
3. Check that no text elements overlap each other.
4. Scroll within the popup (if scrollable) and verify text at all scroll positions.

**Expected Result**:
> No text is truncated with an ellipsis that obscures meaning. No text elements overlap. The popup layout adapts to the larger text size without breaking the overall layout structure.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Regression

---

### TC-PVL-021: Shared layer controls function correctly after popup cycle

**Priority**: P2  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design page is visible with a shared layer containing interactive controls (e.g., a volume slider, a toggle button)
- [ ] Note the current state of each shared layer control

**Steps**:
1. Tap the **Open Popup** button to open the popup.
2. Interact with the popup (e.g., adjust a control inside it).
3. Tap the **Close** button to dismiss the popup.
4. After the fade-out completes, tap each shared layer control (slider, button) on the underlying design page.
5. Observe whether they respond correctly.

**Expected Result**:
> All shared layer controls respond to touch input and behave identically to how they behaved before the popup cycle. No control is unresponsive, frozen, or in an incorrect state.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-PVL-022: Page navigation unaffected after popup open/close cycle

**Priority**: P2  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] The test design has multiple UCI pages reachable via navigation controls
- [ ] A full popup open/close cycle has been completed (TC-PVL-001 and TC-PVL-003 passed)

**Steps**:
1. After completing a popup open/close cycle, locate the page navigation controls on the UCI design.
2. Navigate to a different page.
3. Confirm the destination page renders correctly.
4. Navigate back to the original page.
5. Confirm the original page renders correctly.

**Expected Result**:
> Page navigation functions normally. No navigation stack is corrupted. Both pages render without blank sections, missing layers, or residual popup overlays. The popup trigger button on the original page is present and tappable.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Execution Log

| Date | Tester | Build | Devices Tested | Pass | Fail | Blocked | Notes |
|------|--------|-------|----------------|------|------|---------|-------|
| | | | | | | | |
