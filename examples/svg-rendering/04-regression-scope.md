# Regression Scope: SVG Rendering

**Feature**: SVG Rendering  
**Feature Code**: SVG  
**Platform**: Q-SYS iOS Viewer  
**Date**: 2026-05-28  
**Author**: QE Team  
**Artifacts**: [test-plan.md](test-plan.md) · [test-cases.md](test-cases.md)  

---

## 1. Feature Summary

SVG Rendering covers how the Q-SYS iOS Viewer parses, renders, and dynamically updates SVG graphics embedded in Q-SYS UCI designs. SVG elements are used for decorative graphics, dynamic fill/stroke indicators, and interactive button controls. Any change to SVG rendering logic touches the UCI Rendering Engine at its core and propagates risk to all subsystems that share the rendering surface, control interaction layer, or real-time data pipeline.

---

## 2. Impacted Subsystems

| Subsystem | Risk | Relationship |
|---|---|---|
| SYS-UCI | 🔴 HIGH | Direct — SVG rendering is a primary function of the UCI Rendering Engine |
| SYS-CTRL | 🔴 HIGH | Shared state — SVG-based button and indicator controls interact directly with the control binding layer |
| SYS-NET | 🔴 HIGH | Shared state — dynamic SVG property updates (fill, stroke, opacity) depend on real-time Core data over the network layer |
| SYS-POPUP | 🟡 MED | Shared surface — popups containing SVG elements will re-exercise the SVG rendering path; z-order and transparency interactions create risk |
| SYS-NAV | 🟡 MED | Shared lifecycle — page navigation triggers page load and SVG re-render; any regression in render-on-load affects every navigation event |
| SYS-A11Y | 🟡 MED | Shared surface — VoiceOver labels for SVG interactive controls are populated via the same UCI accessibility surface; changes to SVG element handling may silently drop accessibility metadata |
| SYS-ORIENT | 🟡 MED | Shared lifecycle — orientation changes trigger re-layout and SVG re-scaling; aspect ratio preservation logic is exercised on every rotation |
| SYS-CONN | 🟡 MED | Shared lifecycle — reconnect events trigger design reload and full SVG re-render; any regression in initial render will surface after a reconnect |
| SYS-DISC | 🟢 LOW | Isolated — device discovery precedes connection and has no interaction with the rendering layer |
| SYS-STORE | 🟢 LOW | Isolated — SVG rendering is stateless; no SVG state is persisted to disk or UserDefaults |
| SYS-ALERT | 🟢 LOW | Isolated — notification banners and error dialogs are rendered in a separate overlay layer and are not affected by SVG rendering changes |

### Risk Detail

**SYS-UCI — HIGH (Direct)**  
SVG rendering is implemented within the UCI Rendering Engine. Any code change to SVG parsing, layout, or property binding directly modifies the engine that renders all UCI pages. A regression here may silently break non-SVG controls on the same page (z-order, touch hit testing, layout reflow).

**SYS-CTRL — HIGH (Shared state)**  
SVG-based controls (buttons, toggles) share the same Named Control binding and touch dispatch infrastructure as standard UCI controls. If SVG control elements modify the binding lifecycle or touch event chain, non-SVG controls on the same page may receive incorrect values or miss tap events.

**SYS-NET — HIGH (Shared state)**  
Real-time SVG property updates (fill, stroke, opacity) are driven by Q-SYS Core change events received over the network layer. SVG property handling changes could modify how the app subscribes to or applies Core property change messages, potentially breaking non-SVG property subscriptions as well.

**SYS-POPUP — MED (Shared surface)**  
Popups that contain SVG elements will re-invoke the SVG rendering path when opened. The popup layer also shares the z-order management system with the SVG background layer feature; a regression in z-order handling could cause SVG backgrounds to render over popup content or vice versa.

**SYS-NAV — MED (Shared lifecycle)**  
Every page navigation event triggers a page load and render cycle. If SVG rendering changes alter the render-on-load timing or page lifecycle, every navigation event — including those for non-SVG pages — is indirectly affected.

**SYS-A11Y — MED (Shared surface)**  
VoiceOver accessibility labels for SVG interactive controls are populated by the UCI rendering surface. If SVG rendering changes alter how UCI elements are mapped to iOS accessibility elements, other controls' accessibility labels could also be disrupted.

**SYS-ORIENT — MED (Shared lifecycle)**  
Orientation changes invoke the layout engine, which calls into SVG scaling and aspect-ratio preservation logic. A regression here could affect how all UCI pages reflow on rotation, not just SVG-containing pages.

**SYS-CONN — MED (Shared lifecycle)**  
Reconnect events trigger a full design reload, which re-executes the SVG render path. Any initial-render regression will be reproducible after every reconnect, making this a high-visibility failure surface even for transient rendering bugs.

---

## 3. Integration Points

| Integration | Description |
|---|---|
| Q-SYS Core protocol — Named Control change events | Fill, stroke, and opacity SVG properties are bound to Core Named Controls. The iOS Viewer subscribes to property change notifications and applies them to SVG element attributes. |
| Q-SYS Core protocol — Design schema (SVG asset delivery) | SVG asset data is embedded in the Q-SYS UCI design file and delivered to the iOS Viewer on connect. The app parses and caches SVG assets at design load time. |
| iOS APIs — CoreGraphics / UIKit rendering pipeline | SVG elements are rasterized or vector-rendered via CoreGraphics. Any change to the rendering path interacts with UIView/CALayer draw cycles. |
| iOS APIs — UIAccessibility | SVG interactive controls must populate `accessibilityLabel`, `accessibilityTraits`, and `isAccessibilityElement` on the UIView representing each SVG control element. |
| iOS APIs — UIApplication lifecycle notifications | `applicationDidBecomeActive` and `applicationWillResignActive` notifications govern when SVG property subscriptions are paused and resumed. |

---

## 4. Smoke Suite

| ID | Title | Subsystem | TC Reference | Est. Time |
|---|---|---|---|---|
| SMOKE-SVG-001 | SVG visible on initial page load | SYS-UCI | TC-SVG-001 | < 2 min |
| SMOKE-SVG-002 | SVG fill color updates from Core change | SYS-UCI + SYS-NET | TC-SVG-003 | < 2 min |
| SMOKE-SVG-003 | SVG aspect ratio preserved after orientation change | SYS-ORIENT + SYS-UCI | TC-SVG-019 | < 2 min |
| SMOKE-SVG-004 | Non-SVG UCI controls on same page function correctly | SYS-UCI + SYS-CTRL | TC-SVG-023 | < 2 min |
| SMOKE-SVG-005 | Malformed SVG page loads without crash | SYS-UCI | TC-SVG-010 | < 1 min |

**Total estimated time**: ~9 minutes on a single device pair (iPad + iPhone run in sequence).

> Run SMOKE-SVG-001 through SMOKE-SVG-005 in order before beginning the full regression suite. If any smoke check fails, halt and file a P1 defect before proceeding.

---

## 5. Recommended Validation Areas

### P1 — Must validate before any sign-off

- **SYS-UCI + SYS-CTRL**: Confirm SVG-based button controls trigger Core Named Controls correctly and non-SVG controls on the same page are unaffected — TC-SVG-007, TC-SVG-023
- **SYS-UCI + SYS-NET**: Confirm SVG property updates (fill, stroke, opacity) fire correctly from Core change events — TC-SVG-003, TC-SVG-004, TC-SVG-005
- **SYS-UCI (crash safety)**: Confirm malformed SVG does not crash the app — TC-SVG-010
- **SYS-NAV**: Confirm page navigation between SVG and non-SVG pages is reliable and SVG re-renders correctly on return — TC-SVG-024
- **SYS-CONN**: Confirm device connection and discovery flow is unaffected — TC-SVG-025

### P2 — Validate as part of standard regression pass

- **SYS-POPUP**: Navigate to a popup page that contains an SVG element; confirm popup opens, SVG renders, and popup closes correctly — requires a popup+SVG page in the test design
- **SYS-ORIENT**: Execute TC-SVG-019 on iPad; confirm circular SVG elements remain circular after rotation
- **SYS-A11Y**: Execute TC-SVG-021 with VoiceOver enabled; confirm SVG button labels are announced — physical device required
- **SYS-NET (edge)**: Execute TC-SVG-016 with simulated packet loss; confirm no UI freeze or crash — physical device + traffic shaping tool required
- **SYS-CONN (reconnect)**: Disconnect and reconnect the iOS Viewer; confirm SVG page renders correctly on the first page after reconnect

### P3 — Spot-check or defer to next cycle

- **SYS-DISC**: Run a single device discovery scan; confirm discovery is unaffected — TC-SVG-025 (step 2)
- **SYS-STORE**: Confirm the saved device list is intact after connecting to a design with SVG content — implicit in TC-SVG-025
- **SYS-ALERT**: Confirm error banners (e.g., Core unreachable) still render above SVG background layers — spot-check during negative tests
