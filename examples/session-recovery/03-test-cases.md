# Test Cases: Session Recovery and Reconnect

**Feature**: Session Recovery and Reconnect  
**Feature Code**: SRR  
**Platform**: Q-SYS iOS Viewer (iPad + iPhone)  
**Test Plan**: [test-plan.md](./test-plan.md)  
**Author**: QE Team  
**Date**: 2026-05-21  

---

## Summary

| ID | Title | Priority | Category | Status |
|----|-------|----------|----------|--------|
| TC-SRR-001 | Auto-reconnect after Wi-Fi interruption — iPad | P1 | Functional | ⬜ Not Run |
| TC-SRR-002 | Auto-reconnect after Wi-Fi interruption — iPhone | P1 | Functional | ⬜ Not Run |
| TC-SRR-003 | Auto-reconnect after Core restart | P1 | Functional | ⬜ Not Run |
| TC-SRR-004 | Reconnecting indicator visible during reconnect | P1 | Functional | ⬜ Not Run |
| TC-SRR-005 | Session state restored — UCI page after reconnect | P1 | Functional | ⬜ Not Run |
| TC-SRR-006 | Session state restored — layer visibility after reconnect | P1 | Functional | ⬜ Not Run |
| TC-SRR-007 | UCI controls respond after reconnect without re-navigation | P2 | Functional | ⬜ Not Run |
| TC-SRR-008 | Manual Reconnect button shown after auto-retry exhausted | P1 | Functional | ⬜ Not Run |
| TC-SRR-009 | Manual reconnect while Core still offline shows error | P2 | Negative | ⬜ Not Run |
| TC-SRR-010 | Network never restored — stable offline state, no crash | P1 | Negative | ⬜ Not Run |
| TC-SRR-011 | Core restarted with design change — handled gracefully | P2 | Negative | ⬜ Not Run |
| TC-SRR-012 | Connection lost mid-control interaction — state recovers | P2 | Edge Case | ⬜ Not Run |
| TC-SRR-013 | Flapping network — no error loop | P2 | Edge Case | ⬜ Not Run |
| TC-SRR-014 | App backgrounded during reconnect — resumes on foreground | P2 | Edge Case | ⬜ Not Run |
| TC-SRR-015 | Device locked during reconnect — resumes on unlock | P2 | Edge Case | ⬜ Not Run |
| TC-SRR-016 | Core restarted while popup open — popup dismissed on reconnect | P2 | Edge Case | ⬜ Not Run |
| TC-SRR-017 | App backgrounded, Core restarted, app foregrounded | P1 | Network | ⬜ Not Run |
| TC-SRR-018 | Force-quit and relaunch while Core offline | P2 | iOS Platform | ⬜ Not Run |
| TC-SRR-019 | Lock screen during network interruption — reconnects on unlock | P2 | iOS Platform | ⬜ Not Run |
| TC-SRR-020 | VoiceOver announces "Reconnecting" banner | P2 | Accessibility | ⬜ Not Run |
| TC-SRR-021 | VoiceOver announces connection resolution | P2 | Accessibility | ⬜ Not Run |
| TC-SRR-022 | Manual Reconnect button accessible via VoiceOver | P2 | Accessibility | ⬜ Not Run |
| TC-SRR-023 | Normal connect flow unaffected — regression | P1 | Regression | ⬜ Not Run |
| TC-SRR-024 | Explicit Disconnect still returns to device list | P1 | Regression | ⬜ Not Run |
| TC-SRR-025 | First launch with no saved devices — no reconnect UI shown | P2 | Regression | ⬜ Not Run |

**Total**: 25 cases — P1: 9 · P2: 16

---

## Functional

---

### TC-SRR-001: Auto-reconnect after Wi-Fi interruption — iPad

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Q-SYS Core and the UCI design is displaying on Page 1
- [ ] Wi-Fi connection is active; iPad and Core are on the same subnet
- [ ] `qe-session-recovery-test-design.qsys` is running on the Core

**Steps**:
1. Observe the connected design on Page 1 of the UCI
2. On the Wi-Fi router or access point, disconnect the iPad's network access (or toggle the iPad's Wi-Fi off for 10 seconds and re-enable it)
3. Observe the app during the disconnected period
4. Re-enable the iPad's Wi-Fi connection
5. Wait up to 30 seconds

**Expected Result**:
> During the disconnected period, a reconnecting banner or overlay is visible on screen. Within 30 seconds of Wi-Fi restoration, the app reconnects automatically and returns the user to Page 1 of the UCI design with no manual action required. The reconnecting indicator disappears and the design is fully interactive.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: Time the reconnect duration and note it. Target: <30 seconds from Wi-Fi restore.

---

### TC-SRR-002: Auto-reconnect after Wi-Fi interruption — iPhone

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPhone is connected to the Q-SYS Core and the UCI design is displaying (portrait orientation)
- [ ] Wi-Fi connection is active on the same subnet as the Core

**Steps**:
1. Observe the connected design on the iPhone
2. Toggle Wi-Fi off on the iPhone for 10 seconds
3. Observe the app's reconnecting state
4. Toggle Wi-Fi back on
5. Wait up to 30 seconds

**Expected Result**:
> The reconnecting indicator appears on the iPhone in portrait layout. After Wi-Fi is restored, the app auto-reconnects within 30 seconds and the UCI design is displayed without any user navigation.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: Verify the reconnecting banner does not overlap or clip in iPhone portrait layout.

---

### TC-SRR-003: Auto-reconnect after Core restart

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Q-SYS Core and a UCI design page is displayed
- [ ] Physical Q-SYS Core hardware is available for a controlled restart
- [ ] Core restart takes approximately 60–90 seconds

**Steps**:
1. Note the current UCI page and any active layer states
2. Restart the Q-SYS Core (via Q-SYS Designer or the Core's hardware reboot button)
3. Observe the iOS Viewer during the Core's offline period
4. Wait for the Core to complete its restart and come back online (up to 90 seconds)
5. Observe the iOS Viewer after the Core is reachable again

**Expected Result**:
> The app displays a reconnecting indicator while the Core is restarting. Once the Core is back online, the app reconnects automatically without user action. The user is returned to the UCI design. The reconnecting indicator disappears and the design is interactive.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: If auto-reconnect does not trigger within 120 seconds of Core coming online, proceed to TC-SRR-008 (manual reconnect). Note the Core boot time for context.

---

### TC-SRR-004: Reconnecting indicator visible during reconnect

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Q-SYS Core and the design is displayed
- [ ] Wi-Fi access is easily toggleable

**Steps**:
1. Toggle Wi-Fi off on the iPad
2. Immediately observe the iOS Viewer screen
3. Note what UI element or indicator appears within 5 seconds of disconnection
4. Record the appearance (banner text, spinner location, overlay style)
5. Toggle Wi-Fi back on
6. Observe when and how the indicator disappears

**Expected Result**:
> Within 5 seconds of disconnection, a clearly visible reconnecting indicator appears — either a banner with text such as "Reconnecting…" or a modal overlay. The indicator includes a spinner or animation signaling that a retry is in progress. Once reconnected, the indicator disappears without requiring user dismissal.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: Screenshot the reconnecting UI for the defect baseline if it does not appear.

---

### TC-SRR-005: Session state restored — UCI page after reconnect

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core and `qe-session-recovery-test-design.qsys` is displayed
- [ ] Design has at least 2 UCI pages

**Steps**:
1. Navigate to Page 2 of the UCI design using the page navigation control
2. Confirm Page 2 is displayed
3. Toggle Wi-Fi off for 10 seconds, then re-enable it
4. Wait for the auto-reconnect to complete
5. Observe which page is displayed after reconnect

**Expected Result**:
> After reconnect, the user is returned to Page 2 — the page they were on before the interruption. Page 1 is not shown. The page content renders completely with no blank controls or incomplete layout.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: If Page 1 is shown instead of Page 2, file a defect against session state restoration.

---

### TC-SRR-006: Session state restored — layer visibility after reconnect

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core and `qe-session-recovery-test-design.qsys` is displayed
- [ ] The design includes at least one toggleable shared layer; its initial state is hidden

**Steps**:
1. Toggle the shared layer to visible using its trigger control
2. Confirm the shared layer is visible on screen
3. Toggle Wi-Fi off for 10 seconds, then re-enable it
4. Wait for reconnect to complete
5. Observe the shared layer visibility state after reconnect

**Expected Result**:
> After reconnect, the shared layer is visible — matching the live state broadcast by the Core. No reset to the design's default layer state occurs. The layer renders completely with all controls visible.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: The expected result is the live Core state, not necessarily the last iOS Viewer state. If the Core reset the layer during reconnect, the app should show the Core's current state.

---

### TC-SRR-007: UCI controls respond after reconnect without re-navigation

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is reconnected to the Core after a simulated Wi-Fi interruption (run TC-SRR-001 first or simulate independently)
- [ ] The UCI design is displayed post-reconnect

**Steps**:
1. After reconnect completes, tap a slider on the current UCI page
2. Drag the slider to a new value
3. Observe the slider's response and any Core feedback
4. Tap a button on the page
5. Observe the button's response

**Expected Result**:
> Slider responds to touch immediately after reconnect; dragging changes the value and the Core reflects the updated value. Button press triggers the expected action without requiring a page navigation or a manual refresh.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-008: Manual Reconnect button shown after auto-retry exhausted

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core
- [ ] Ability to keep the network disconnected for the full auto-retry period (typically 30–60 seconds)

**Steps**:
1. Toggle Wi-Fi off on the iPad
2. Wait through the full auto-retry period without re-enabling Wi-Fi
3. Observe the UI once all auto-retries are exhausted

**Expected Result**:
> After auto-retries are exhausted, the reconnecting spinner/banner transitions to a failure state. A **Reconnect** button (or equivalent manual action) is clearly visible. An error message explains what happened (e.g., "Could not reconnect to [Core name]. Check your network and try again."). The user is not automatically navigated away to the device list.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: Note the exact time from disconnect to manual reconnect button appearance.

---

## Negative

---

### TC-SRR-009: Manual reconnect while Core still offline shows error

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] The manual **Reconnect** button is displayed (auto-retries exhausted — see TC-SRR-008)
- [ ] Wi-Fi is still disabled or the Core is still offline

**Steps**:
1. Tap the **Reconnect** button
2. Observe the app's response

**Expected Result**:
> The app attempts to reconnect and then displays a clear, specific error message such as "Could not connect to [Core name]. The Core may be offline or unreachable." A retry option or a **Back to Device List** option is presented. The app does not crash or freeze.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-010: Network never restored — stable offline state, no crash

**Priority**: P1  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad is connected to the Core

**Steps**:
1. Toggle airplane mode on
2. Wait 5 minutes without restoring network access
3. Observe the app throughout the 5-minute period
4. Note the final UI state at the 5-minute mark

**Expected Result**:
> The app enters the manual reconnect state and remains stable. No crash, no infinite spinner, no repeated error dialogs. The reconnect failure UI is displayed continuously without looping or animating in a distracting or broken manner. Memory usage does not grow continuously.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: Monitor for memory warnings or console errors during the 5-minute period.

---

### TC-SRR-011: Core restarted with design change — handled gracefully

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad is connected to `qe-session-recovery-test-design.qsys` with a 2-page design
- [ ] Access to Q-SYS Designer to modify the design while Core is down

**Steps**:
1. Note the current page and design layout
2. Restart the Core
3. While the Core is restarting, use Q-SYS Designer to update the design (e.g., add a new page or remove a control)
4. Allow the Core to finish starting with the new design
5. Observe the iOS Viewer's reconnect behavior

**Expected Result**:
> The app reconnects and loads the updated design. If the previously active page no longer exists, the app navigates to page 1 without crashing. No stale controls from the old design remain visible. No crash occurs during the design schema change.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

## Edge Cases

---

### TC-SRR-012: Connection lost mid-control interaction — state recovers

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] iPad is connected to the Core and a slider is present on the visible UCI page

**Steps**:
1. Begin dragging a slider on the UCI page (hold finger on slider, start moving)
2. While still dragging, toggle Wi-Fi off
3. Release the slider
4. Re-enable Wi-Fi and wait for auto-reconnect

**Expected Result**:
> The slider interaction ends gracefully when the connection is lost; no crash or freeze. After reconnect, the slider reflects the current live value from the Core (not a stuck mid-drag value). The slider is fully interactive after reconnect.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-013: Flapping network — no error loop

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] iPad is connected to the Core

**Steps**:
1. Toggle Wi-Fi off for 5 seconds, then on
2. Immediately (before reconnect completes) toggle Wi-Fi off again for 5 seconds, then on
3. Repeat this off/on cycle 5 times total over ~60 seconds
4. After the final Wi-Fi-on, wait 30 seconds and observe

**Expected Result**:
> The app handles each interruption without entering an error loop or displaying cascading error dialogs. After the final network restore, the app reconnects and returns to a normal connected state. No crash, memory warning, or stuck spinner is observed.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-014: App backgrounded during active reconnect — resumes on foreground

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core

**Steps**:
1. Toggle Wi-Fi off to trigger the reconnect flow
2. Within 5 seconds (while reconnecting indicator is showing), press the Home button to background the app
3. Wait 10 seconds
4. Re-enable Wi-Fi
5. Foreground the app by tapping its icon

**Expected Result**:
> On foreground, the app either shows the reconnecting indicator briefly (if reconnect is still in progress) or the fully connected design (if reconnect completed while backgrounded). The app does not show a stale static screen or require a manual reconnect action.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-015: Device locked during reconnect — resumes on unlock

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core

**Steps**:
1. Toggle Wi-Fi off to trigger the reconnect flow
2. Press the iPad's power button to lock the screen
3. Wait 15 seconds
4. Re-enable Wi-Fi
5. Unlock the iPad (Face ID or passcode)

**Expected Result**:
> After unlocking, the app resumes and either the reconnect is completing (indicator visible) or the connected design is shown. No blank screen, no navigation to the device list, and no crash.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-016: Core restarted while popup is open — popup dismissed on reconnect

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core; `qe-session-recovery-test-design.qsys` has a popup configured
- [ ] The popup is currently open on screen

**Steps**:
1. With the popup open, restart the Q-SYS Core
2. Observe the popup and reconnecting state during Core restart
3. Wait for the Core to come back online and auto-reconnect to complete
4. Observe the UI state after reconnect

**Expected Result**:
> When disconnected, the popup is dismissed or the whole design overlay transitions to the reconnecting indicator. After reconnect, the popup is not shown unless the Core's current design state calls for it. The user is returned to the base UCI page without a stale popup overlay remaining.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

## Network

---

### TC-SRR-017: App backgrounded, Core restarted, app foregrounded — reconnect initiates

**Priority**: P1  
**Category**: Network  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core and the design is displayed
- [ ] Physical Q-SYS Core is available for restart

**Steps**:
1. Press the Home button to background the iOS Viewer
2. Restart the Q-SYS Core while the app is in the background
3. Wait for the Core to complete its restart (up to 90 seconds)
4. Tap the iOS Viewer icon to foreground the app

**Expected Result**:
> On foreground, the app immediately attempts to reconnect and shows the reconnecting indicator. Within 30 seconds of the Core being reachable, the app reconnects and displays the UCI design. The user does not need to navigate to the device list or tap a Connect button.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

## iOS Platform

---

### TC-SRR-018: Force-quit and relaunch while Core offline

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad was previously connected to the Core and a session was active

**Steps**:
1. Toggle Wi-Fi off (Core is unreachable)
2. Force-quit the iOS Viewer from the app switcher
3. Relaunch the iOS Viewer
4. Observe the initial screen

**Expected Result**:
> The app launches and shows the device list. The previously used Core appears in the Recents or saved devices list. No reconnect spinner is shown on first launch — reconnect is initiated only after the user explicitly taps the device entry. No crash on launch.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-019: Lock screen during network interruption — reconnects on unlock

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPhone is connected to the Core (portrait orientation)

**Steps**:
1. Toggle Wi-Fi off
2. Lock the iPhone screen (power button)
3. Wait 20 seconds
4. Re-enable Wi-Fi
5. Unlock the iPhone

**Expected Result**:
> After unlock, the app shows the reconnecting indicator and auto-reconnects within 30 seconds of Wi-Fi restore. The UCI design is displayed. No manual reconnect action is required.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

## Accessibility

---

### TC-SRR-020: VoiceOver announces "Reconnecting" banner

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad has VoiceOver enabled (Settings → Accessibility → VoiceOver → On)
- [ ] iPad is connected to the Core and the UCI design is displayed

**Steps**:
1. Toggle Wi-Fi off to trigger the reconnect flow
2. Listen for a VoiceOver announcement within 5 seconds

**Expected Result**:
> VoiceOver announces a message consistent with the reconnecting state — e.g., "Reconnecting" or "Connection lost, reconnecting" — within 5 seconds of the banner appearing. The announcement is made without requiring the user to navigate to the banner element.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: iOS accessibility notification type should be `.screenChanged` or `.announcement`.

---

### TC-SRR-021: VoiceOver announces connection resolution

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] VoiceOver is enabled; iPad is in the reconnecting state (Wi-Fi off, banner showing)

**Steps**:
1. Re-enable Wi-Fi
2. Wait for auto-reconnect to complete
3. Listen for a VoiceOver announcement at the moment of reconnect

**Expected Result**:
> VoiceOver announces "Connected" or equivalent when the reconnect succeeds. If reconnect fails and the manual Reconnect button appears, VoiceOver announces the failure state and the presence of the Reconnect button.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-022: Manual Reconnect button accessible via VoiceOver

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] VoiceOver is enabled; the manual **Reconnect** button is visible (auto-retries exhausted)

**Steps**:
1. Swipe right with one finger to move VoiceOver focus through the reconnect failure screen
2. Confirm the **Reconnect** button is reachable by swipe navigation
3. Listen to VoiceOver's label announcement for the button
4. Double-tap to activate the **Reconnect** button via VoiceOver

**Expected Result**:
> The **Reconnect** button is reachable by swiping through the UI with VoiceOver. VoiceOver announces a descriptive label — e.g., "Reconnect, button" — not "Button" or "unlabeled". Double-tapping activates the reconnect attempt.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

## Regression

---

### TC-SRR-023: Normal connect flow unaffected — regression

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is at the device list screen (app launched fresh or after a disconnect)
- [ ] Q-SYS Core is online and reachable

**Steps**:
1. Tap the Q-SYS Core entry in the device list
2. If prompted for a passcode, enter the correct passcode
3. Wait for the UCI design to load

**Expected Result**:
> The connect flow proceeds without any new reconnect UI appearing during a normal first connection. The UCI design loads within 10 seconds. No reconnect banner, spinner, or error dialog is shown during a standard connection.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

### TC-SRR-024: Explicit Disconnect returns to device list — regression

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad is connected to the Core and the UCI design is displayed

**Steps**:
1. Tap the **Disconnect** button (or navigate to the disconnect action via the menu)
2. Observe the resulting screen

**Expected Result**:
> The app navigates to the device list immediately. No reconnect attempt is made after a deliberate disconnect. The device list shows the previously connected Core in the Recents section. No reconnecting indicator appears.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  
**Notes**: Explicit disconnect must never trigger the auto-reconnect flow.

---

### TC-SRR-025: First launch with no saved devices — no reconnect UI shown

**Priority**: P2  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] App has been freshly installed or all saved devices have been cleared from the device list

**Steps**:
1. Launch the iOS Viewer
2. Observe the initial screen

**Expected Result**:
> The app shows the empty device list with an "Add Device" prompt. No reconnecting indicator, no error dialog, and no auto-reconnect spinner are shown. The reconnect flow is not triggered when there is no previous session to restore.

**Actual Result**: _(leave blank)_  
**Status**: ⬜ Not Run  

---

## Execution Log

| Date | Tester | Build | Device | iOS | Pass | Fail | Blocked | Notes |
|------|--------|-------|--------|-----|------|------|---------|-------|
| | | | | | | | | |
