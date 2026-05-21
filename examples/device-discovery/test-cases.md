# Test Cases: Device Discovery and Connection

**Feature**: Device Discovery and Connection  
**Feature Code**: DD  
**Platform**: Q-SYS iOS Viewer  
**Test Plan**: [test-plan.md](./test-plan.md)  
**Author**: QE Team  
**Date**: 2026-05-20  
**Build**: iOS Viewer 9.10.0 (build 1000)  

---

## Summary Table

| ID | Title | Priority | Category | Status |
|----|-------|----------|----------|--------|
| TC-DD-001 | Automatic mDNS discovery — device appears in list (iPad) | P1 | Functional | ⬜ Not Run |
| TC-DD-002 | Automatic mDNS discovery — device appears in list (iPhone) | P1 | Functional | ⬜ Not Run |
| TC-DD-003 | Connect to discovered device — reaches design control page | P1 | Functional | ⬜ Not Run |
| TC-DD-004 | Manual device entry by IP address | P1 | Functional | ⬜ Not Run |
| TC-DD-005 | Connect to passcode-protected design | P2 | Functional | ⬜ Not Run |
| TC-DD-006 | Recents list persists after app restart | P2 | Functional | ⬜ Not Run |
| TC-DD-007 | Pull-to-refresh updates the device list | P2 | Functional | ⬜ Not Run |
| TC-DD-008 | Empty state shown when no devices found | P2 | Negative | ⬜ Not Run |
| TC-DD-009 | Wrong passcode shows error; retry is allowed | P2 | Negative | ⬜ Not Run |
| TC-DD-010 | Invalid IP address shows validation error | P2 | Negative | ⬜ Not Run |
| TC-DD-011 | Connection timeout shows error message | P2 | Negative | ⬜ Not Run |
| TC-DD-012 | Core goes offline — device list reflects unavailability | P2 | Edge Case | ⬜ Not Run |
| TC-DD-013 | Duplicate device entry is handled gracefully | P3 | Edge Case | ⬜ Not Run |
| TC-DD-014 | Connection attempt in airplane mode | P2 | Network | ⬜ Not Run |
| TC-DD-015 | Wi-Fi disconnects during device list — app recovers on reconnect | P2 | Network | ⬜ Not Run |
| TC-DD-016 | App backgrounded during discovery — resumes on foreground | P2 | iOS Platform | ⬜ Not Run |
| TC-DD-017 | Device list renders correctly in landscape orientation (iPad) | P3 | iOS Platform | ⬜ Not Run |
| TC-DD-018 | VoiceOver navigation through device list and connection flow | P3 | Accessibility | ⬜ Not Run |
| TC-DD-019 | Dynamic Type (Accessibility XL) — no truncation or overlap | P3 | Accessibility | ⬜ Not Run |
| TC-DD-020 | Main navigation unchanged after using Device Discovery | P3 | Regression | ⬜ Not Run |

---

## Functional

---

### TC-DD-001: Automatic mDNS discovery — device appears in list (iPad)

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] Q-SYS Core 9.10 is powered on and connected to the test network
- [ ] The test network has mDNS enabled
- [ ] Q-SYS iOS Viewer 9.10.0 is installed on iPad Pro 12.9" running iOS 17.4
- [ ] The iPad is connected to the same subnet as the Core

**Steps**:
1. Launch the Q-SYS iOS Viewer app on the iPad.
2. Tap the **Devices** tab (or the initial device discovery screen if no devices are connected).
3. Wait up to 10 seconds for the automatic discovery scan to complete.
4. Observe the device list.

**Expected Result**:
> The Q-SYS Core 510i appears in the device list with its hostname (or IP address), Core version, and online status indicator. The device appears within 10 seconds without any manual configuration.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: If discovery takes >10 seconds, note the actual time as a latency observation.

---

### TC-DD-002: Automatic mDNS discovery — device appears in list (iPhone)

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] Q-SYS Core 9.10 is powered on and connected to the test network
- [ ] The test network has mDNS enabled
- [ ] Q-SYS iOS Viewer 9.10.0 is installed on iPhone 15 running iOS 17.4
- [ ] The iPhone is connected to the same subnet as the Core

**Steps**:
1. Launch the Q-SYS iOS Viewer app on the iPhone.
2. Observe the device discovery screen in portrait orientation.
3. Wait up to 10 seconds for automatic discovery to complete.
4. Observe the device list.

**Expected Result**:
> The Q-SYS Core 510i appears in the device list. The list is readable in portrait orientation; device name and status are fully visible without truncation.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-003: Connect to discovered device — reaches design control page

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] TC-DD-001 passed (Core is visible in the device list on iPad)
- [ ] `qe-test-design-9.10.qsys` is active and running on the Core (no passcode)

**Steps**:
1. In the device list, tap the Q-SYS Core 510i entry.
2. Observe the connection transition.
3. Observe the screen after connection.

**Expected Result**:
> The app transitions to the design's User Control Interface (UCI) or control page without prompting for a passcode. The navigation bar shows the connected Core's name. No error dialogs appear.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-004: Manual device entry by IP address

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] Q-SYS Core 9.10 is powered on; its IP address is known (e.g., `192.168.1.100`)
- [ ] Q-SYS iOS Viewer 9.10.0 is installed on iPad Pro 12.9"

**Steps**:
1. Launch the Q-SYS iOS Viewer app.
2. Tap **Add Device** (or the `+` button) in the device list screen.
3. In the manual entry form, enter the Core's IP address: `192.168.1.100`.
4. Tap **Add** (or **Connect**).
5. Observe the result.

**Expected Result**:
> The device is added to the list and shows as reachable. Tapping it initiates a connection. The manual entry persists in the device list after being added.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-005: Connect to passcode-protected design

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] A passcode-protected design is active on the Core (passcode: `1234`)
- [ ] The Core is visible in the device list

**Steps**:
1. Tap the Core entry in the device list.
2. Observe the passcode prompt.
3. Enter the correct passcode: `1234`.
4. Tap **Connect**.

**Expected Result**:
> A passcode dialog appears before connecting. After entering the correct passcode, the app connects and displays the design's control page.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-006: Recents list persists after app restart

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] TC-DD-003 passed (a successful connection was made to the Core in this session)

**Steps**:
1. Double-press the Home button (or swipe up from bottom) to open the App Switcher.
2. Swipe up on Q-SYS iOS Viewer to kill the app.
3. Relaunch Q-SYS iOS Viewer from the Home Screen.
4. Navigate to the **Recents** section of the device list.

**Expected Result**:
> The previously connected Core appears in the Recents list with its hostname and last-connected timestamp. Tapping it initiates a new connection attempt.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-007: Pull-to-refresh updates the device list

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] The device list screen is visible with at least one discovered device

**Steps**:
1. Power off the Q-SYS Core (or disconnect it from the network).
2. In the Q-SYS iOS Viewer, pull down on the device list to trigger a refresh.
3. Wait for the refresh animation to complete.
4. Observe the device list.

**Expected Result**:
> The device that was powered off is removed from the list or shown as offline after the pull-to-refresh completes. The refresh animation completes within 5 seconds.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Negative

---

### TC-DD-008: Empty state shown when no devices found

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] No Q-SYS Core devices are powered on or reachable on the network
- [ ] The device's Recents list is empty (first launch or cleared)

**Steps**:
1. Launch Q-SYS iOS Viewer.
2. Navigate to the device discovery screen.
3. Wait for the discovery scan to complete (up to 10 seconds).

**Expected Result**:
> The app displays a clear empty state message (e.g., "No devices found. Make sure your Q-SYS Core is on the same network.") with a visible option to add a device manually or refresh. The app does not crash or display a blank screen.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-009: Wrong passcode shows error; retry is allowed

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] A passcode-protected design is active on the Core (correct passcode: `1234`)
- [ ] The Core is visible in the device list

**Steps**:
1. Tap the Core entry in the device list.
2. When the passcode prompt appears, enter an incorrect passcode: `9999`.
3. Tap **Connect**.
4. Observe the error response.
5. Clear the passcode field and enter the correct passcode: `1234`.
6. Tap **Connect** again.

**Expected Result**:
> An error message is shown indicating the passcode is incorrect. The passcode field is cleared and the user can retry. On the second attempt with the correct passcode, the app connects successfully.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-010: Invalid IP address shows validation error

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] The manual device entry form is accessible

**Steps**:
1. Tap **Add Device** to open the manual entry form.
2. Enter an invalid IP address: `999.999.999.999`.
3. Tap **Add**.

**Expected Result**:
> An inline validation error appears indicating the IP address is invalid (e.g., "Please enter a valid IP address or hostname"). The form does not dismiss. No network request is made.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Also test with non-numeric text (e.g., `not-an-ip`).

---

### TC-DD-011: Connection timeout shows error message

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] A device has been manually added by IP at an address where no Core is running (e.g., `192.168.1.250`)

**Steps**:
1. In the device list, tap the unreachable device entry.
2. Observe the connection attempt.
3. Wait for the connection to time out.

**Expected Result**:
> After a reasonable timeout (≤15 seconds), the app displays an error message (e.g., "Unable to connect. Check that the device is on and reachable."). A retry button or option is available. The app does not hang indefinitely.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Note actual timeout duration.

---

## Edge Cases

---

### TC-DD-012: Core goes offline — device list reflects unavailability

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Negative  

**Preconditions**:
- [ ] Q-SYS Core is visible in the discovered device list
- [ ] Auto-refresh or real-time status updates are active

**Steps**:
1. Observe the device list with the Core showing as online.
2. Power off or disconnect the Q-SYS Core from the network.
3. Wait up to 30 seconds without manually refreshing.
4. Observe the device list entry.

**Expected Result**:
> The Core's status indicator changes to offline within 30 seconds, or the device is removed from the list. An offline indicator (e.g., greyed out, "Offline" badge) is clearly visible.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-013: Duplicate device entry is handled gracefully

**Priority**: P3  
**Category**: Edge Case  
**Test Type**: Negative  

**Preconditions**:
- [ ] A Q-SYS Core has already been added manually by IP address (`192.168.1.100`)

**Steps**:
1. Tap **Add Device** again.
2. Enter the same IP address: `192.168.1.100`.
3. Tap **Add**.

**Expected Result**:
> Either the app prevents adding a duplicate and shows an informational message ("This device is already in your list"), or the duplicate is silently merged. The device list does not contain two identical entries.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Network

---

### TC-DD-014: Connection attempt in airplane mode

**Priority**: P2  
**Category**: Network  
**Test Type**: Negative  

**Preconditions**:
- [ ] A Q-SYS Core entry exists in the device list (from Recents or manual entry)

**Steps**:
1. Enable **Airplane Mode** on the iOS device via **Settings → Airplane Mode**.
2. Return to Q-SYS iOS Viewer.
3. Tap the Core entry to attempt a connection.
4. Observe the result.

**Expected Result**:
> The app immediately shows a network unavailable error (e.g., "No network connection. Please check your Wi-Fi."). No crash or indefinite loading spinner occurs.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-015: Wi-Fi disconnects during device list — app recovers on reconnect

**Priority**: P2  
**Category**: Network  
**Test Type**: Negative  

**Preconditions**:
- [ ] Q-SYS iOS Viewer is open on the device list screen with at least one discovered device

**Steps**:
1. Note the current device list state.
2. On the iOS device, go to **Settings → Wi-Fi** and disable Wi-Fi.
3. Return to Q-SYS iOS Viewer and observe the device list.
4. Re-enable Wi-Fi from **Settings → Wi-Fi**.
5. Return to Q-SYS iOS Viewer and observe the device list.

**Expected Result**:
> When Wi-Fi is disabled, the device list shows a network error or empty state with an appropriate message. When Wi-Fi is re-enabled, the app resumes discovery and the Core reappears in the list without requiring an app restart.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## iOS Platform

---

### TC-DD-016: App backgrounded during discovery — resumes on foreground

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] Q-SYS iOS Viewer has just launched and the device discovery scan is in progress (spinner visible)

**Steps**:
1. While the discovery scan is running, press the **Home button** (or swipe up) to background the app.
2. Wait 15 seconds.
3. Return to Q-SYS iOS Viewer via the App Switcher.
4. Observe the device list state.

**Expected Result**:
> The app returns to the device discovery screen. Discovery either completed in the background (showing results) or resumes automatically upon foreground. The app does not crash or show a blank screen.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

### TC-DD-017: Device list renders correctly in landscape orientation (iPad)

**Priority**: P3  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" is being used
- [ ] At least two devices are in the device list

**Steps**:
1. Hold the iPad in portrait orientation and observe the device list.
2. Rotate the iPad to landscape orientation.
3. Observe the device list layout.

**Expected Result**:
> The device list adapts to landscape orientation without visual glitches, overlapping elements, or cut-off content. Device names, status badges, and action buttons remain fully visible and tappable.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Accessibility

---

### TC-DD-018: VoiceOver navigation through device list and connection flow

**Priority**: P3  
**Category**: Accessibility  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] **Settings → Accessibility → VoiceOver** is enabled on the test device
- [ ] At least one device is visible in the device list

**Steps**:
1. With VoiceOver active, swipe right to navigate forward through all elements on the device list screen.
2. Verify each element announces a meaningful label (e.g., "Q-SYS Core 510i, Online, button" not just "Button").
3. Double-tap the Core device entry to activate it.
4. If a passcode dialog appears, verify VoiceOver can navigate and fill the passcode field.
5. Complete the connection flow using only VoiceOver gestures.

**Expected Result**:
> All interactive elements (device rows, Add Device button, refresh control) have descriptive accessibility labels. The primary connection flow can be completed without vision. No element is unreachable or silently skipped by VoiceOver.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Note any elements with missing or unclear labels.

---

### TC-DD-019: Dynamic Type (Accessibility XL) — no truncation or overlap

**Priority**: P3  
**Category**: Accessibility  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] **Settings → Accessibility → Display & Text Size → Larger Text** is set to the **Accessibility XL** size
- [ ] At least two devices are in the device list

**Steps**:
1. Launch Q-SYS iOS Viewer with Accessibility XL text size active.
2. Navigate to the device list screen.
3. Inspect each device entry: device name, status label, and any sub-labels.
4. Tap **Add Device** and inspect the manual entry form at this text size.

**Expected Result**:
> No text is truncated with an ellipsis where it conveys critical information (device name, status). UI elements do not overlap each other. The layout adapts gracefully to the larger text size.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Regression

---

### TC-DD-020: Main navigation unchanged after using Device Discovery

**Priority**: P3  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] TC-DD-003 has been completed (connected to a Core and viewed the design)

**Steps**:
1. While connected to the Core's design, tap the **Back** or **Disconnect** button to return to the device list.
2. Navigate to the **Settings** screen.
3. Verify all settings sections are accessible.
4. Navigate to any other top-level tab or section.

**Expected Result**:
> All previously available navigation destinations (Settings, any other tabs) remain accessible and function normally. No navigation stack corruption or unexpected blank screens appear.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_

---

## Execution Log

| Date | Tester | Build | Devices Tested | Pass | Fail | Blocked | Notes |
|------|--------|-------|----------------|------|------|---------|-------|
| | | | | | | | |
