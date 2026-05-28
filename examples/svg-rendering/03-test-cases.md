# Test Cases: SVG Rendering

**Feature**: SVG Rendering  
**Feature Code**: SVG  
**Platform**: Q-SYS iOS Viewer  
**Date**: 2026-05-28  
**Author**: QE Team  
**Test Plan**: [test-plan.md](test-plan.md)  

---

## Summary

| ID | Title | Priority | Category | Status |
|---|---|---|---|---|
| TC-SVG-001 | SVG renders on initial page load — iPad | P1 | Functional | ⬜ Not Run |
| TC-SVG-002 | SVG renders on initial page load — iPhone | P1 | Functional | ⬜ Not Run |
| TC-SVG-003 | SVG fill color updates from Core property change | P1 | Functional | ⬜ Not Run |
| TC-SVG-004 | SVG stroke color updates from Named Control change | P1 | Functional | ⬜ Not Run |
| TC-SVG-005 | SVG opacity transitions smoothly from Core value change | P1 | Functional | ⬜ Not Run |
| TC-SVG-006 | Multiple SVG elements on same page render without conflict | P2 | Functional | ⬜ Not Run |
| TC-SVG-007 | SVG button control triggers Core Named Control on tap | P2 | Functional | ⬜ Not Run |
| TC-SVG-008 | SVG background layer does not obscure foreground controls | P2 | Functional | ⬜ Not Run |
| TC-SVG-009 | SVG renders correctly in dark mode | P2 | Functional | ⬜ Not Run |
| TC-SVG-010 | Malformed SVG page loads without crash | P1 | Negative | ⬜ Not Run |
| TC-SVG-011 | SVG with unsupported filter degrades gracefully | P2 | Negative | ⬜ Not Run |
| TC-SVG-012 | SVG with missing external asset renders placeholder | P2 | Negative | ⬜ Not Run |
| TC-SVG-013 | SVG re-renders correctly after page navigation | P2 | Edge Case | ⬜ Not Run |
| TC-SVG-014 | Large SVG file renders within acceptable time | P2 | Edge Case | ⬜ Not Run |
| TC-SVG-015 | SVG renders after app returns from background mid-render | P2 | Edge Case | ⬜ Not Run |
| TC-SVG-016 | SVG property updates during degraded network | P2 | Edge Case | ⬜ Not Run |
| TC-SVG-017 | SVG on page in large design (> 20 pages) renders correctly | P3 | Edge Case | ⬜ Not Run |
| TC-SVG-018 | SVG renders correctly — iPad landscape | P1 | iOS Platform | ⬜ Not Run |
| TC-SVG-019 | SVG aspect ratio preserved after orientation change | P1 | iOS Platform | ⬜ Not Run |
| TC-SVG-020 | App backgrounded mid-SVG update returns to correct state | P2 | iOS Platform | ⬜ Not Run |
| TC-SVG-021 | VoiceOver announces SVG button with correct label | P2 | Accessibility | ⬜ Not Run |
| TC-SVG-022 | Dynamic Type large text does not break SVG layout | P3 | Accessibility | ⬜ Not Run |
| TC-SVG-023 | Non-SVG UCI controls on same page function correctly | P1 | Regression | ⬜ Not Run |
| TC-SVG-024 | Page navigation between SVG and non-SVG pages works | P1 | Regression | ⬜ Not Run |
| TC-SVG-025 | Device connection flow unaffected by SVG rendering changes | P1 | Regression | ⬜ Not Run |

**Totals**: 25 cases — P1: 9 · P2: 14 · P3: 2

---

## Functional

---

### TC-SVG-001: SVG renders on initial page load — iPad

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" running iOS 18.x, connected to same subnet as Q-SYS Core
- [ ] `qe-svg-rendering-test-design.qsys` loaded in Core 9.10+
- [ ] Design page "SVG-Main" contains at least one SVG decorative element and one SVG-based control
- [ ] iOS Viewer app is installed and the device is not currently connected

**Steps**:
1. Launch the iOS Viewer app on iPad.
2. Tap the saved device entry for the test Core or tap **Add Device** and enter the Core IP.
3. Tap **Connect**.
4. Observe the "SVG-Main" page as it loads.
5. Visually inspect all SVG elements on the page.

**Expected Result**:
> All SVG graphics are fully visible with correct fill colors, correct dimensions, and no partial renders, blank regions, or visual artifacts. The page is interactive within 3 seconds of connection.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Baseline render check. Log any visual artifact with a screenshot.

---

### TC-SVG-002: SVG renders on initial page load — iPhone

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPhone 15 Pro running iOS 18.x, connected to same subnet as Q-SYS Core
- [ ] `qe-svg-rendering-test-design.qsys` loaded in Core 9.10+
- [ ] Design page "SVG-Main" contains at least one SVG decorative element and one SVG-based control
- [ ] iOS Viewer app is installed and the device is not currently connected

**Steps**:
1. Launch the iOS Viewer app on iPhone.
2. Tap the saved device entry for the test Core or tap **Add Device** and enter the Core IP.
3. Tap **Connect**.
4. Observe the "SVG-Main" page as it loads in portrait orientation.
5. Visually inspect all SVG elements on the page.

**Expected Result**:
> All SVG graphics are fully visible on the smaller form factor. Elements are proportionally scaled to the iPhone screen without overflow, clipping, or distortion.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Compare visually with TC-SVG-001 iPad result to confirm consistent rendering across form factors.

---

### TC-SVG-003: SVG fill color updates from Core property change

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page is displayed and an SVG indicator element with a fill-color binding is visible
- [ ] A Core Named Control ("SVG.FillColor") drives the fill color of the indicator (e.g., values 0 = blue, 1 = red)

**Steps**:
1. Note the current fill color of the SVG indicator on screen (should be blue).
2. Using Q-SYS Core scripting, QRC, or a connected Q-SYS control surface, change the value of `SVG.FillColor` to `1`.
3. Observe the SVG indicator in the iOS Viewer.

**Expected Result**:
> The SVG indicator fill color changes from blue to red within 500 ms of the Core value change. The transition is immediate and does not flicker or temporarily render an intermediate color.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  
**Notes**: Core value injection can be performed via Q-SYS scripting console or a dedicated control surface in the design.

---

### TC-SVG-004: SVG stroke color updates from Named Control change

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page is displayed and an SVG shape with a visible stroke/border is shown
- [ ] Core Named Control "SVG.StrokeColor" drives the stroke color (e.g., values 0 = gray, 1 = green)

**Steps**:
1. Note the current stroke color of the SVG shape (should be gray).
2. Change the value of `SVG.StrokeColor` to `1` via Core scripting or control surface.
3. Observe the SVG shape stroke in the iOS Viewer.

**Expected Result**:
> The stroke color of the SVG shape updates from gray to green within 500 ms. No visual artifacts (e.g., ghost stroke, doubled border) are present.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-005: SVG opacity transitions smoothly from Core value change

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page is displayed and an SVG element with opacity binding is visible
- [ ] Core Named Control "SVG.Opacity" drives opacity (range: 0.0 fully transparent, 1.0 fully opaque)

**Steps**:
1. Observe the SVG element at full opacity (1.0).
2. Change `SVG.Opacity` to `0.0` via Core scripting.
3. Observe the SVG element.
4. Change `SVG.Opacity` back to `1.0`.
5. Observe the SVG element again.

**Expected Result**:
> The SVG element fades to fully transparent when opacity is set to 0.0 and returns to fully opaque when set to 1.0. Transitions are smooth with no flicker or abrupt jump.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-006: Multiple SVG elements on same page render without conflict

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design page "SVG-MultiElement" contains at least 5 distinct SVG elements (icons, indicators, decorative shapes)

**Steps**:
1. Navigate to the "SVG-MultiElement" page in the iOS Viewer.
2. Visually inspect all SVG elements simultaneously.
3. Trigger all Core Named Controls bound to SVG elements to update values.
4. Observe the page while all SVG elements update.

**Expected Result**:
> All SVG elements render simultaneously with correct colors, positions, and sizes. Dynamic updates to all elements occur without any element flickering, disappearing, or showing another element's content.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-007: SVG button control triggers Core Named Control on tap

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design page "SVG-Controls" contains an SVG-based button bound to Core Named Control "SVG.Button1"
- [ ] A visible indicator on the page reflects the state of "SVG.Button1" (e.g., an LED that turns on when button is triggered)

**Steps**:
1. Navigate to "SVG-Controls" page in the iOS Viewer.
2. Observe the LED indicator (off state).
3. Tap the SVG-based button.
4. Observe the LED indicator.
5. Tap the SVG button again.
6. Observe the LED indicator.

**Expected Result**:
> Tapping the SVG button toggles the LED indicator state (off → on → off). The Core Named Control "SVG.Button1" reflects the correct value change. The button tap target is responsive and does not require multiple taps.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-008: SVG background layer does not obscure foreground controls

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design page "SVG-Layered" has an SVG decorative background layer and foreground interactive UCI controls (sliders, buttons) positioned over it

**Steps**:
1. Navigate to "SVG-Layered" page.
2. Inspect the z-order visually — SVG background should be behind foreground controls.
3. Tap each foreground UCI control.
4. Observe control response.

**Expected Result**:
> All foreground UCI controls (sliders, buttons) are fully visible above the SVG background. Each control responds to tap and value change as expected. The SVG background does not intercept touch events meant for foreground controls.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-009: SVG renders correctly in dark mode

**Priority**: P2  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page is displayed in light mode

**Steps**:
1. Note the appearance of all SVG elements in light mode.
2. Open iOS **Settings > Display & Brightness** and enable **Dark** appearance.
3. Return to the iOS Viewer app.
4. Observe all SVG elements on the "SVG-Main" page.

**Expected Result**:
> SVG elements that use system colors adapt correctly to dark mode. SVG elements with hard-coded colors remain unchanged. No SVG element becomes invisible against the dark background, and no rendering artifacts appear.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Negative

---

### TC-SVG-010: Malformed SVG page loads without crash

**Priority**: P1  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design page "SVG-Malformed" contains an SVG element with intentionally invalid/malformed SVG markup (e.g., unclosed tag, invalid attribute values)

**Steps**:
1. Navigate to the "SVG-Malformed" page in the iOS Viewer.
2. Observe the page load behavior.
3. Attempt to interact with any non-SVG controls on the page.

**Expected Result**:
> The page loads without crashing the app. The malformed SVG element is either rendered as blank, replaced with a placeholder, or silently skipped. All other non-SVG controls on the page remain functional.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  
**Notes**: This is a P1 crash safety test. Any crash is an automatic P1 defect.

---

### TC-SVG-011: SVG with unsupported filter degrades gracefully

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design page "SVG-UnsupportedFilter" contains an SVG element using a CSS `filter` or SMIL animation not supported by the iOS Viewer

**Steps**:
1. Navigate to "SVG-UnsupportedFilter" page.
2. Observe the SVG element rendering.
3. Interact with any adjacent controls.

**Expected Result**:
> The SVG element renders without the unsupported filter/animation applied (the filter is silently dropped). The app does not crash. Adjacent controls remain functional.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-012: SVG with missing external asset renders placeholder

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design page "SVG-MissingAsset" contains an SVG element referencing an external image or font that is not bundled with the design

**Steps**:
1. Navigate to "SVG-MissingAsset" page.
2. Observe the area where the SVG with the missing external asset is placed.

**Expected Result**:
> The SVG element renders as a blank placeholder or renders the SVG shape without the missing resource. The app does not crash, and no error dialog blocks the user.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Edge Case

---

### TC-SVG-013: SVG re-renders correctly after page navigation

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page and a second page "SVG-None" (no SVG content) are accessible

**Steps**:
1. Navigate to "SVG-Main" page. Note the state of all SVG elements.
2. Navigate to "SVG-None" page.
3. Navigate back to "SVG-Main" page.
4. Observe all SVG elements.

**Expected Result**:
> SVG elements on "SVG-Main" re-render correctly after returning from another page. The current property values (fill, stroke, opacity) match the current Core values — no stale or default values are shown.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-014: Large SVG file renders within acceptable time

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design page "SVG-Large" contains a single SVG element with a file size of approximately 500 KB (complex paths, many nodes)

**Steps**:
1. Navigate to "SVG-Large" page.
2. Measure the time from page tap to full SVG render (use a stopwatch or observe screen).
3. Interact with any controls on the page after it loads.

**Expected Result**:
> The large SVG element renders fully within 3 seconds on both iPad and iPhone. No partial render, loading spinner overstay, or UI freeze is observed. Controls remain responsive after load.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-015: SVG renders after app returns from background mid-render

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page is displayed and a Core Named Control is changing rapidly (simulated with a looping script)

**Steps**:
1. With "SVG-Main" visible and SVG elements actively updating, press the **Home** button (or swipe up) to send the app to the background.
2. Wait 10 seconds.
3. Return to the iOS Viewer app.
4. Observe all SVG elements.

**Expected Result**:
> After returning from the background, all SVG elements display their current Core values (not frozen/stale background values). Property updates resume correctly within 2 seconds of foreground restoration.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-016: SVG property updates during degraded network

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Negative  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page is displayed
- [ ] A Core Named Control is being updated at 1-second intervals via scripting

**Steps**:
1. Observe SVG fill color updating at 1-second intervals under normal network conditions.
2. Introduce packet loss or throttle the network using a Wi-Fi access point or proxy (e.g., 30% packet loss).
3. Continue observing the SVG element updates.
4. Restore network to normal.
5. Observe SVG element updates resume.

**Expected Result**:
> Under degraded network conditions, SVG property updates may lag but do not cause a crash or frozen UI. The app does not display stale or incorrect values indefinitely. After network restoration, updates resume to the correct current Core values within 5 seconds.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-017: SVG on page in large design renders correctly

**Priority**: P3  
**Category**: Edge Case  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with a large design (> 20 pages) including at least one SVG-containing page
- [ ] The SVG page is not the first page in the design

**Steps**:
1. Connect to Core and observe the first page.
2. Navigate to page 20 or later that contains SVG elements.
3. Observe SVG rendering.
4. Navigate back to earlier pages and return to the SVG page.

**Expected Result**:
> The SVG page renders correctly regardless of its position in the page order. No memory pressure artifacts (blank pages, missing SVG elements) are observed when navigating through a large design.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## iOS Platform

---

### TC-SVG-018: SVG renders correctly — iPad landscape

**Priority**: P1  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" running iOS 18.x, connected to Core, "SVG-Main" page displayed
- [ ] Device is in landscape orientation

**Steps**:
1. Confirm iPad is in landscape orientation (Home button or Face ID sensor on the side).
2. Observe all SVG elements on the "SVG-Main" page.
3. Compare layout to the expected design layout.

**Expected Result**:
> All SVG elements are fully visible, correctly positioned, and correctly sized for the iPad landscape viewport. No elements overflow or are clipped beyond the safe area.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-019: SVG aspect ratio preserved after orientation change

**Priority**: P1  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" running iOS 18.x, connected to Core, "SVG-Main" page displayed in landscape orientation

**Steps**:
1. Observe SVG elements in landscape orientation. Note width and height proportions of a circular SVG element.
2. Rotate the iPad to portrait orientation.
3. Observe all SVG elements.
4. Rotate back to landscape.
5. Observe SVG elements again.

**Expected Result**:
> SVG elements reflow correctly for the new viewport dimensions while preserving their aspect ratio. A circular SVG element remains circular (not stretched into an ellipse) in both orientations. No visual artifacts appear during or after rotation.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-020: App backgrounded mid-SVG update returns to correct state

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad Pro 12.9" running iOS 18.x, connected to Core, "SVG-Main" page displayed
- [ ] Core Named Control "SVG.FillColor" is being changed every 2 seconds via scripting

**Steps**:
1. Observe the SVG fill color cycling.
2. Note the current fill color state.
3. Press the **Home** button to background the app.
4. Change `SVG.FillColor` to a specific value (e.g., green) via Core scripting.
5. Return to the iOS Viewer app.
6. Observe the SVG fill color.

**Expected Result**:
> After returning from the background, the SVG element displays the current Core value (green) rather than the last value observed before backgrounding. No stale state is retained.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Accessibility

---

### TC-SVG-021: VoiceOver announces SVG button with correct label

**Priority**: P2  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Controls" page is displayed and contains an SVG-based button with accessibility label "Trigger Zone 1"
- [ ] VoiceOver is enabled (Settings > Accessibility > VoiceOver > On)

**Steps**:
1. With VoiceOver active, navigate to the "SVG-Controls" page.
2. Swipe right with one finger to move VoiceOver focus through elements.
3. Stop when VoiceOver focus lands on the SVG-based button.
4. Listen to the VoiceOver announcement.
5. Double-tap to activate the button.

**Expected Result**:
> VoiceOver announces the button with the label "Trigger Zone 1" and the trait "Button". Double-tapping activates the button and triggers the correct Core Named Control. VoiceOver does not skip or silently pass over the SVG button element.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-022: Dynamic Type large text does not break SVG layout

**Priority**: P3  
**Category**: Accessibility  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Main" page is displayed under standard text size

**Steps**:
1. Note the layout of SVG elements and any adjacent text labels in standard text size.
2. Open iOS **Settings > Accessibility > Display & Text Size > Larger Text** and drag the slider to the largest size.
3. Return to the iOS Viewer app.
4. Observe the "SVG-Main" page layout.

**Expected Result**:
> SVG elements maintain their positions and sizes regardless of the system Dynamic Type setting. Adjacent text labels scale without overlapping or clipping SVG elements. No SVG element is pushed off-screen.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Regression

---

### TC-SVG-023: Non-SVG UCI controls on same page function correctly

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] "SVG-Layered" page contains both SVG elements and standard UCI controls (slider, toggle button, text label)

**Steps**:
1. Navigate to "SVG-Layered" page.
2. Interact with the slider — drag it from minimum to maximum.
3. Tap the toggle button to change its state.
4. Observe the text label update reflecting the toggle state.
5. Confirm Core Named Control values updated correctly via Core scripting console.

**Expected Result**:
> The slider, toggle button, and text label all behave exactly as expected — identical to behavior on a page without SVG elements. SVG rendering has not introduced any touch interception, layout shift, or value binding issue on non-SVG controls.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-024: Page navigation between SVG and non-SVG pages works

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone connected to Core with `qe-svg-rendering-test-design.qsys` active
- [ ] Design contains "SVG-Main" (SVG content) and "No-SVG-Page" (standard UCI controls, no SVG)

**Steps**:
1. Navigate to "SVG-Main" page. Confirm SVG elements render correctly.
2. Navigate to "No-SVG-Page". Confirm the page loads and controls are interactive.
3. Navigate back to "SVG-Main". Confirm SVG elements are correct.
4. Repeat steps 2–3 three more times rapidly.

**Expected Result**:
> Navigation between pages with and without SVG content is smooth and reliable. Each page loads correctly on every visit. No page becomes blank, frozen, or shows content from the previously visited page.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

### TC-SVG-025: Device connection flow unaffected by SVG rendering changes

**Priority**: P1  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] iPad or iPhone with iOS Viewer installed, not yet connected to Core
- [ ] Q-SYS Core is running with `qe-svg-rendering-test-design.qsys`

**Steps**:
1. Launch the iOS Viewer app.
2. Browse the device list — confirm the Core appears via mDNS discovery.
3. Tap the Core device entry and tap **Connect**.
4. Confirm connection is established and the default page loads.
5. Tap **Disconnect**.
6. Confirm the app returns to the device list.
7. Reconnect using the same device entry.

**Expected Result**:
> mDNS discovery, connection establishment, design load, disconnection, and reconnection all complete successfully. No step is blocked or degraded by SVG rendering changes. The flow behaves identically to a design without SVG content.

**Actual Result**: _(leave blank — filled during execution)_  
**Status**: ⬜ Not Run  

---

## Execution Log

| Date | Build | Executed By | iPad Result | iPhone Result | Notes |
|---|---|---|---|---|---|
| | | | | | |
