# Regression Scope: Shared Layer Navigation

**Feature**: Shared Layer Navigation  
**Feature Code**: SLN  
**Platform**: Q-SYS iOS Viewer  
**Date**: 2026-05-28  
**Author**: QE Team  
**Artifacts**: [02-test-plan.md](02-test-plan.md) · [03-test-cases.md](03-test-cases.md)  

---

## 1. Feature Summary

Shared Layer Navigation covers how the Q-SYS iOS Viewer renders and manages UCI design layers that are assigned to multiple pages. These shared layers persist visually across page transitions and may contain interactive controls bound to Q-SYS Core Named Controls. Changes to shared layer management logic affect both the navigation stack and the UCI rendering engine simultaneously, making this one of the highest cross-subsystem risk surfaces in the iOS Viewer.

---

## 2. Impacted Subsystems

| Subsystem | Risk | Relationship |
|---|---|---|
| SYS-NAV | 🔴 HIGH | Direct — shared layer persistence is managed by the navigation stack; every page transition exercises shared layer lifecycle |
| SYS-UCI | 🔴 HIGH | Direct — shared layers are rendered and updated by the UCI Rendering Engine; all render-on-load and property-update paths are exercised |
| SYS-POPUP | 🔴 HIGH | Shared state — popup visibility interacts directly with shared layer z-ordering; a regression here risks popups rendering under shared layers or shared layers rendering over popup content |
| SYS-CTRL | 🔴 HIGH | Shared surface — interactive controls within shared layers share the same Named Control binding and touch dispatch infrastructure as page-specific controls |
| SYS-CONN | 🟡 MED | Shared lifecycle — reconnect events trigger a full design reload and shared layer re-render; any initial-render regression will be visible after every reconnect |
| SYS-NET | 🟡 MED | Shared state — real-time shared layer control updates depend on Core Named Control change events arriving over the network layer |
| SYS-A11Y | 🟡 MED | Shared surface — VoiceOver focus management during page navigation with shared layers depends on accessibility element ordering across both the navigation layer and the shared layer |
| SYS-ORIENT | 🟡 MED | Shared lifecycle — orientation changes trigger re-layout of both page-specific and shared layers simultaneously; shared layer scaling and position must be recalculated correctly |
| SYS-DISC | 🟢 LOW | Isolated — device discovery precedes connection and has no interaction with the layer management or rendering subsystems |
| SYS-STORE | 🟢 LOW | Isolated — shared layer navigation is stateless; no layer assignment or state is persisted beyond the active session |
| SYS-ALERT | 🟢 LOW | Isolated — notification banners and error dialogs are rendered in a separate system overlay layer above the UCI layer stack |

### Risk Detail

**SYS-NAV — HIGH (Direct)**  
Shared layers are owned by the navigation stack — they must be mounted, updated, and unmounted in sync with page transitions. Any change to the navigation lifecycle (push, pop, replace) directly affects when shared layers are rendered, refreshed, and removed. A regression here manifests as flickering, duplication, or ghost layers during navigation.

**SYS-UCI — HIGH (Direct)**  
The UCI Rendering Engine is responsible for rendering all shared layer controls and applying Core property updates. Changes to shared layer management within the UCI engine may also affect how page-specific controls are rendered and updated, since both share the same rendering pipeline and property subscription mechanism.

**SYS-POPUP — HIGH (Shared state)**  
Popup visibility is managed through the same layer z-ordering system as shared layers. Shared layers that use high z-index values risk rendering over popup content. A regression in z-order management could make popups invisible, unresponsive, or incorrectly clipped by a shared layer element.

**SYS-CTRL — HIGH (Shared surface)**  
Controls within shared layers use the same Named Control binding lifecycle and UIKit touch event chain as page-specific controls. If shared layer management alters the binding lifecycle on page transitions, page-specific controls' bindings may be incorrectly subscribed, unsubscribed, or duplicated.

**SYS-CONN — MED (Shared lifecycle)**  
A reconnect event triggers a full design reload, which re-executes the shared layer assignment and initial render. Any bug in shared layer initialization will be reproducible on every reconnect, making this a high-visibility regression surface even if the root cause is in the initial-load path.

**SYS-NET — MED (Shared state)**  
Shared layer controls receive real-time updates via Q-SYS Core Named Control change events delivered over the network layer. If shared layer navigation changes alter the subscription model (e.g., re-subscribing per page transition), unnecessary network traffic or missed updates may result.

**SYS-A11Y — MED (Shared surface)**  
During page navigation with VoiceOver active, the iOS accessibility system must correctly track which elements are focusable as pages transition. Shared layers introduce additional elements that exist across multiple pages. A regression here could cause VoiceOver to lose focus, announce stale elements, or skip shared layer controls entirely.

**SYS-ORIENT — MED (Shared lifecycle)**  
Orientation changes invoke a re-layout pass that must correctly handle both page-specific and shared layer elements simultaneously. Shared layer controls that are anchored relative to the viewport may shift or overflow if the re-layout order or constraint calculation is regressed.

---

## 3. Integration Points

| Integration | Description |
|---|---|
| Q-SYS Core protocol — Design schema (layer assignment data) | The iOS Viewer receives the layer-to-page assignment map when the design loads. Shared layers are identified by their assignment to multiple pages in the design schema. |
| Q-SYS Core protocol — Named Control change events | Shared layer controls subscribe to Core Named Control change events for real-time updates. Subscriptions must remain active regardless of which page is currently displayed. |
| iOS APIs — UINavigationController / page transition | Page navigation is managed through UIKit's navigation or view controller transition system. Shared layer views must persist across these transitions without being deallocated or re-instantiated unnecessarily. |
| iOS APIs — CALayer / UIView z-order | Shared layer z-ordering is managed via UIView/CALayer hierarchy. Z-index conflicts with popup layers and page-specific controls are resolved at this layer. |
| iOS APIs — UIAccessibility | VoiceOver element ordering must account for shared layer elements being present on every assigned page. `UIAccessibilityElement` registration must not duplicate or drop shared layer elements on navigation. |
| iOS APIs — UIApplication lifecycle notifications | `applicationDidBecomeActive` / `applicationWillResignActive` govern when Named Control subscriptions for shared layer controls are paused and resumed. |

---

## 4. Smoke Suite

| ID | Title | Subsystem | TC Reference | Est. Time |
|---|---|---|---|---|
| SMOKE-SLN-001 | Shared layer renders on first page load | SYS-UCI + SYS-NAV | TC-SLN-001 | < 2 min |
| SMOKE-SLN-002 | Shared layer persists across page navigation | SYS-NAV + SYS-UCI | TC-SLN-003 | < 2 min |
| SMOKE-SLN-003 | Shared layer control state maintained after navigation | SYS-UCI + SYS-CTRL | TC-SLN-005 | < 2 min |
| SMOKE-SLN-004 | Page navigation without shared layers unaffected | SYS-NAV | TC-SLN-023 | < 2 min |
| SMOKE-SLN-005 | Popup visibility on page with shared layer unaffected | SYS-POPUP + SYS-NAV | TC-SLN-024 | < 2 min |

**Total estimated time**: ~10 minutes on a single device pair (iPad + iPhone run in sequence).

> Run SMOKE-SLN-001 through SMOKE-SLN-005 in order before beginning the full regression suite. If any smoke check fails, halt and file a P1 defect before proceeding.

---

## 5. Recommended Validation Areas

### P1 — Must validate before any sign-off

- **SYS-NAV + SYS-UCI**: Confirm shared layer persists correctly across all assigned pages without flickering or duplication — TC-SLN-003, TC-SLN-004, TC-SLN-012
- **SYS-CTRL**: Confirm shared layer interactive controls trigger Core Named Controls correctly from any page — TC-SLN-006
- **SYS-POPUP**: Confirm popup opens above shared layer without z-order corruption — TC-SLN-024
- **SYS-NAV (regression)**: Confirm page navigation on non-shared-layer pages is unaffected — TC-SLN-023
- **SYS-CTRL (regression)**: Confirm page-specific UCI controls are unaffected by shared layer presence — TC-SLN-025

### P2 — Validate as part of standard regression pass

- **SYS-NAV (rapid)**: Execute TC-SLN-012 rapid navigation test; capture video evidence of 5+ page sequence — physical device required
- **SYS-UCI + SYS-CONN**: Disconnect and reconnect; confirm shared layer re-renders correctly on first assigned page — TC-SLN-014
- **SYS-NET**: Confirm shared layer Named Control updates fire correctly while navigating between pages — TC-SLN-007
- **SYS-A11Y**: Execute TC-SLN-020 and TC-SLN-022 with VoiceOver; confirm focus is not lost on page transition — physical device required
- **SYS-ORIENT**: Execute TC-SLN-018 on iPad; confirm shared layer layout is correct in portrait and landscape

### P3 — Spot-check or defer to next cycle

- **SYS-DISC**: Run a single device discovery scan; confirm discovery is unaffected — implicit in TC-SLN-023 setup
- **SYS-STORE**: Confirm the saved device list and preferences are intact after navigating through shared layer pages — spot-check
- **SYS-ALERT**: Confirm error banners (e.g., Core unreachable) render above the shared layer without being obscured — observe during TC-SLN-014 disconnect step
